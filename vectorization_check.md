# USER-INTEL underperforming on Cascade Lake

## Adroit (skylake)

lmp_cascade which was built on X works on adroit (32 s). BIG PIECE OF PUZZLE  
lmp_adroit_uintel works on adroit (32 s).  
lmp_adroit_d works as expected (61 s).

## Della (mix)

lmp_della_uintel does not work (54 s) on della cascade.  
lmp_cascade does not work (56 s) on della skylake.  
lmp_adroit_uintel does not work (57 s) on della skylake

# Vectorization Check

The following was on the standard cascade lake test node on Perseus:

```
== 1/10 loops ==

gcc -g -O0 abc.c
real	0m8.601s

gcc -O3 abc.c
real	0m2.124s

gcc -O3 -mavx2 abc.c
real	0m0.943s

=== 1/10 loops and module load rh8 ===

gcc -O3 abc.c
real	0m0.792s

gcc -O3 -mavx2 abc.c
real	0m0.479s

gcc -O3 -march=skylake-avx512 abc.c
real	0m0.468s

=== 10x loops and module load intel/19.1/64/19.1.1.217 ===

icc -Ofast abc.c
real	0m2.641s

icc -Ofast -xCORE-AVX2 abc.c
real	0m1.360s

icc -Ofast -xCORE-AVX512 abc.c
real	0m1.361s

icc -Ofast -xCORE-AVX512 -qopt-zmm-usage=high abc.c
real	0m0.704s

== 10x loops on adroit compute nodes with load intel/19.1/64/19.1.1.217 ==

icc -Ofast abc.c
real	0m3.029s

icc -Ofast -xCORE-AVX2 abc.c
real	0m1.563s

icc -Ofast -xCORE-AVX512 abc.c
real	0m1.564s

icc -Ofast -xCORE-AVX512 -qopt-zmm-usage=high abc.c
real	0m0.922s
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

Be weary of the number of context switches:

```
/usr/bin/time -v ./a.out 
40.950000
	Command being timed: "./a.out"
	User time (seconds): 3.53
	System time (seconds): 0.00
	Percent of CPU this job got: 99%
	Elapsed (wall clock) time (h:mm:ss or m:ss): 0:03.54
	Average shared text size (kbytes): 0
	Average unshared data size (kbytes): 0
	Average stack size (kbytes): 0
	Average total size (kbytes): 0
	Maximum resident set size (kbytes): 752
	Average resident set size (kbytes): 0
	Major (requiring I/O) page faults: 0
	Minor (reclaiming a frame) page faults: 274
	Voluntary context switches: 2
	Involuntary context switches: 8
	Swaps: 0
	File system inputs: 0
	File system outputs: 0
	Socket messages sent: 0
	Socket messages received: 0
	Signals delivered: 0
	Page size (bytes): 4096
	Exit status: 0
```

```
$ perf stat -r 5 -d ./a.out 
40.950000
40.950000
40.950000
40.950000
40.950000

 Performance counter stats for './a.out' (5 runs):

          3,548.80 msec task-clock:u              #    1.000 CPUs utilized            ( +-  0.05% )
                 0      context-switches:u        #    0.000 K/sec                  
                 0      cpu-migrations:u          #    0.000 K/sec                  
               238      page-faults:u             #    0.067 K/sec                    ( +-  0.31% )
                 0      cycles:u                  #    0.000 GHz                    
    14,080,354,932      instructions:u                                                ( +-  0.00% )
     1,280,053,143      branches:u                #  360.701 M/sec                    ( +-  0.00% )
             7,148      branch-misses:u           #    0.00% of all branches          ( +-  0.21% )
            87,442      L1-dcache-loads:u         #    0.025 M/sec                    ( +-  0.23% )
             8,114      L1-dcache-load-misses:u   #    9.28% of all L1-dcache hits    ( +-  0.75% )
                 0      LLC-loads:u               #    0.000 K/sec                  
                 0      LLC-load-misses:u         #    0.00% of all LL-cache hits   

           3.54934 +- 0.00188 seconds time elapsed  ( +-  0.05% )
```

## Useful Links

[https://stackoverflow.com/questions/47878352/how-to-check-if-compiled-code-uses-sse-and-avx-instructions/50056059](https://stackoverflow.com/questions/47878352/how-to-check-if-compiled-code-uses-sse-and-avx-instructions/50056059)
