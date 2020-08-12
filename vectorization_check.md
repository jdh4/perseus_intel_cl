# Vectorization Check

The following was on the standard cascade lake test node on Perseus:

```
== 1/10 loops ==

gcc -g -O0 abc.c
real	0m8.601s
user	0m8.573s

gcc -O3 abc.c
real	0m2.124s
user	0m2.114s

gcc -O3 -mavx2 abc.c
real	0m0.943s
user	0m0.935s

=== 1/10 loops and module load rh8 ===

gcc -O3 abc.c
real	0m0.792s
user	0m0.785s

gcc -O3 -mavx2 abc.c
real	0m0.479s
user	0m0.471s

gcc -O3 -march=skylake-avx512 abc.c
real	0m0.468s
user	0m0.461s

=== 10x loops and module load intel/19.1/64/19.1.1.217 ===

icc -Ofast abc.c
real	0m2.641s
user	0m2.632s

icc -Ofast -xCORE-AVX2 abc.c
real	0m1.360s
user	0m1.351s

icc -Ofast -xCORE-AVX512 abc.c
real	0m1.361s
user	0m1.351s

icc -Ofast -xCORE-AVX512 -qopt-zmm-usage=high abc.c
real	0m0.704s
user	0m0.694s

== 10x loops on adroit compute nodes with load intel/19.1/64/19.1.1.217 ==

icc -Ofast abc.c

real	0m3.029s
user	0m3.020s

icc -Ofast -xCORE-AVX2 abc.c
real	0m1.563s
user	0m1.556s

icc -Ofast -xCORE-AVX512 abc.c
real	0m1.564s
user	0m1.559s

icc -Ofast -xCORE-AVX512 -qopt-zmm-usage=high abc.c
real	0m0.922s
user	0m0.918s
```

```
#!/bin/bash
#SBATCH --job-name=vector        # create a short name for your job
#SBATCH --nodes=1                # node count
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1        # cpu-cores per task (>1 if multi-threaded tasks)
#SBATCH --mem=190000
#SBATCH --time=00:02:00          # total run time limit (HH:MM:SS)
#SBATCH -p test
#SBATCH -C cl

time ./a.out
```

```
#include <stdio.h>
#define ARRAY_SIZE 4096
#define NUMBER_OF_TRIALS 10000000

void main(int argc, char *argv[]) {

    /* Declare arrays small enough to stay in L1 cache.
 *        Assume the compiler aligns them correctly. */
    double a[ARRAY_SIZE], b[ARRAY_SIZE], c[ARRAY_SIZE];
    int i, t;
    double m = 1.5, d;

    /* Initialize a, b, and c arrays */
    for (i=0; i < ARRAY_SIZE; i++) {
        a[i] = 0.0; b[i] = i*1.0e-9; c[i] = i*0.5e-9;
    }
    /* Perform operations with arrays many, many times */
    for (t=0; t < NUMBER_OF_TRIALS; t++) {
        for (i=0; i < ARRAY_SIZE; i++) {
            a[i] += m*b[i] + c[i];
        }
    }
    /* Print a result so the loops won't be optimized away */
    for (i=0; i < ARRAY_SIZE; i++) d += a[i];
    printf("%f\n",d/ARRAY_SIZE);
}
```
