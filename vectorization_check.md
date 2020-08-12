# Vectorization Check

The following was on the standard cascade lake test node on Perseus:

```
gcc -g -O0 abc.c
real	0m8.601s
user	0m8.573s

gcc -O3 abc.c
real	0m2.124s
user	0m2.114s

gcc -O3 -mavx2 abc.c
real	0m0.943s
user	0m0.935s

=== module load rh8 ===

gcc -O3 abc.c
real	0m0.792s
user	0m0.785s

gcc -O3 -mavx2 abc.c
real	0m0.479s
user	0m0.471s

gcc -O3 -march=skylake-avx512 abc.c
real	0m0.468s
user	0m0.461s
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
