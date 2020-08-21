# Perseus - Intel Cascade Lake

```
New nodes for testing 

Three new nodes will be available for testing.  As they are added, notes
will be added here.  Access to these nodes will be through the Feature request in
Slurm as well as a special partition.

#SBATCH -p test
#SBATCH -C <feature name see below>

1) Intel Cascade Lake AP (Platinum 9242 CPU @ 2.30GHz) 
   has 48 cores/socket, 768G RAM, with 12 memory channels per socket
   #SBATCH -C ap

2) Intel Cascade Lake (Gold 6242 CPU @ 2.80GHz)
   has 16 cores/socket, 192G RAM with 6 memory channels per socket
   #SBATCH -C cl

3) AMD  EPYC 7542 32-Core Processor, 2.9 GHz
   has 32 cores/socket (64/node), 256G RAM with 8 memory channels per socket
   #SBATCH -C amd
```

```
HOSTNAMES       STATE    CPUS S:C:T    CPUS(A/I/O/T)   CPU_LOAD MEMORY   GRES     PARTITION          AVAIL_FEATURES
perseus-ap1     idle     96   2:48:1   0/96/0/96       0.01     768000   (null)   test               ap
perseus-r1c1n2  idle     32   2:16:1   0/32/0/32       0.02     190000   (null)   test               cl
perseus-amd     idle     64   2:32:1   0/64/0/64       0.01     256000   (null)   test               amd
```

```
# cl
[jdh4@perseus-r1c1n2 ~]$ lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                32
On-line CPU(s) list:   0-31
Thread(s) per core:    1
Core(s) per socket:    16
Socket(s):             2
NUMA node(s):          2
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 85
Model name:            Intel(R) Xeon(R) Gold 6242 CPU @ 2.80GHz
Stepping:              7
CPU MHz:               1501.000
CPU max MHz:           3900.0000
CPU min MHz:           1200.0000
BogoMIPS:              5600.00
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              1024K
L3 cache:              22528K
NUMA node0 CPU(s):     0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30
NUMA node1 CPU(s):     1,3,5,7,9,11,13,15,17,19,21,23,25,27,29,31
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch epb cat_l3 cdp_l3 invpcid_single intel_ppin intel_pt ssbd mba ibrs ibpb stibp ibrs_enhanced tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm cqm mpx rdt_a avx512f avx512dq rdseed adx smap clflushopt clwb avx512cd avx512bw avx512vl xsaveopt xsavec xgetbv1 cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local dtherm ida arat pln pts pku ospke avx512_vnni md_clear spec_ctrl intel_stibp flush_l1d arch_capabilities
```

```
[jdh4@perseus-r1c1n2 ~]$ numactl -H
available: 2 nodes (0-1)
node 0 cpus: 0 2 4 6 8 10 12 14 16 18 20 22 24 26 28 30
node 0 size: 94957 MB
node 0 free: 89212 MB
node 1 cpus: 1 3 5 7 9 11 13 15 17 19 21 23 25 27 29 31
node 1 size: 96732 MB
node 1 free: 91811 MB
node distances:
node   0   1 
  0:  10  21 
  1:  21  10
```

## AP

```
[jdh4@perseus-ap1 ~]$ numactl -H
available: 4 nodes (0-3)
node 0 cpus: 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
node 0 size: 191929 MB
node 0 free: 190005 MB
node 1 cpus: 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47
node 1 size: 193515 MB
node 1 free: 190451 MB
node 2 cpus: 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71
node 2 size: 193515 MB
node 2 free: 191989 MB
node 3 cpus: 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95
node 3 size: 193499 MB
node 3 free: 191555 MB
node distances:
node   0   1   2   3 
  0:  10  21  21  21 
  1:  21  10  21  21 
  2:  21  21  10  21 
  3:  21  21  21  10 

$ lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                96
On-line CPU(s) list:   0-95
Thread(s) per core:    1
Core(s) per socket:    48
Socket(s):             2
NUMA node(s):          4
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 85
Model name:            Intel(R) Xeon(R) Platinum 9242 CPU @ 2.30GHz
Stepping:              7
CPU MHz:               999.932
CPU max MHz:           3800.0000
CPU min MHz:           1000.0000
BogoMIPS:              4600.00
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              1024K
L3 cache:              36608K
NUMA node0 CPU(s):     0-23
NUMA node1 CPU(s):     24-47
NUMA node2 CPU(s):     48-71
NUMA node3 CPU(s):     72-95
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch epb cat_l3 cdp_l3 invpcid_single intel_pt ssbd mba ibrs ibpb stibp ibrs_enhanced tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm cqm mpx rdt_a avx512f avx512dq rdseed adx smap clflushopt clwb avx512cd avx512bw avx512vl xsaveopt xsavec xgetbv1 cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local dtherm ida arat pln pts hwp hwp_act_window hwp_epp hwp_pkg_req pku ospke avx512_vnni md_clear spec_ctrl intel_stibp flush_l1d arch_capabilities
```


## Create 4 GB or 40 GB array of doubles in NumPy

```python
import numpy as np
from time import perf_counter

t0 = perf_counter()
N = 500_000_000
#N *= 10
x = np.random.randn(N)
print(x.mean())
print(perf_counter() - t0)
```

```
#!/bin/bash
#SBATCH --job-name=myjob         # create a short name for your job
#SBATCH --nodes=1                # node count
#SBATCH --ntasks=1               # total number of tasks across all nodes
#SBATCH --cpus-per-task=1        # cpu-cores per task (>1 if multi-threaded tasks)
#SBATCH --mem=5G
#SBATCH --time=00:05:00          # total run time limit (HH:MM:SS)
#SBATCH -p test
#SBATCH -C ap

module purge
module load anaconda3/2020.2

python random_array.py
```

| Node          | Run Time (s) for 4 GB array  | Run Time (s) for 40 GB array |
| ------------- |:----------------------------:|:----------------------------:|
| ap            | 17.8      | 174.6 |
| cl            | 17.3      | 169.3 |
| amd           | 18.8      | 189.1 |
| broadwell     | 18.4      | 184.8 |

## LU decomposition

```python
from time import perf_counter

import numpy as np
import scipy as sp
from scipy.linalg import lu

N = 10000
cpu_runs = 3

times = []
X = np.random.randn(N, N).astype(np.float64)
for _ in range(cpu_runs):
  t0 = perf_counter()
  p, l, u = lu(X, check_finite=False)
  times.append(perf_counter() - t0)
print("CPU time: ", min(times))
print("NumPy version: ", np.__version__)
print("SciPy version: ", sp.__version__)
print(p.sum())
print(times)
```

```
#!/bin/bash
#SBATCH --job-name=myjob         # create a short name for your job
#SBATCH --nodes=1                # node count
#SBATCH --ntasks=1               # total number of tasks across all nodes
#SBATCH --cpus-per-task=<T>      # cpu-cores per task (>1 if multi-threaded tasks)
#SBATCH --mem=7G
#SBATCH --time=00:05:00          # total run time limit (HH:MM:SS)
# SBATCH -p test
# SBATCH -C cl
# SBATCH -C ap

hostname

module purge
module load anaconda3/2020.2

OMP_NUM_TASKS=<T> python lu.py
```

| Node          | Run Time (s) with 1 core | Run Time (s) with 4 cores | 
| ------------- |:-------------:|:------------------------------------:|
| ap            | 9.4           | 3.5  | 
| cl            | 9.1           | 3.5  |
| amd           | 15.5          | 4.5  |
| broadwell     | 15.9          | 5.4  |

## SVD

```python
from time import perf_counter

N = 10000
cpu_runs = 3

times = []
import numpy as np
X = np.random.randn(N, N).astype(np.float64)
for _ in range(cpu_runs):
  t0 = perf_counter()
  u, s, v = np.linalg.svd(X)
  times.append(perf_counter() - t0)
print("CPU time: ", min(times), flush=True)
print("NumPy version: ", np.__version__)
print(s.sum())
print(times)
```


```
#!/bin/bash
#SBATCH --job-name=myjob         # create a short name for your job
#SBATCH --nodes=1                # node count
#SBATCH --ntasks=2               # total number of tasks across all nodes
#SBATCH --ntasks-per-socket=1    # total number of tasks across all nodes
#SBATCH --cpus-per-task=8        # cpu-cores per task (>1 if multi-threaded tasks)
#SBATCH --mem=100G
#SBATCH --time=00:05:00          # total run time limit (HH:MM:SS)
#SBATCH -p test
#SBATCH -C cl
# SBATCH -C ap
# SBATCH -C amd

hostname

numactl -s
taskset -c -p $$

module purge
module load anaconda3/2020.2

OMP_NUM_TASKS=16 python svd.py
```

| Node          | Run Time (s)  |
| ------------- |:-------------:|
| ap            | 38.8   |
| cl            | 36.9   |
| amd           | 92.4   |
| broadwell     | 54.2   |

## LAMMPS (simple atomic fluid)

```
ssh perseus
wget https://github.com/lammps/lammps/archive/stable_3Mar2020.tar.gz

module load intel/19.0/64/19.0.5.281 intel-mpi/intel/2018.3/64
```

```
units           lj
atom_style      atomic

lattice         fcc 0.8442
region          box block 0 30 0 30 0 30
create_box      1 box
create_atoms    1 box
mass            1 1.0

replicate       3 3 3

velocity        all create 1.0 87287

pair_style      lj/cut 2.5
pair_coeff      1 1 1.0 1.0 2.5

neighbor        0.3 bin
neigh_modify    every 20 delay 0 check no

fix             1 all nve
fix             2 all langevin 1.0 1.0 1.0 48279

timestep        0.005

thermo          100
run             1000
```

```
#!/bin/bash
#SBATCH --job-name=lj-melt       # create a short name for your job
#SBATCH --nodes=1                # node count
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=28       # cpu-cores per task (>1 if multi-threaded tasks)
#SBATCH --mem-per-cpu=4G         # memory per cpu-core (4G is default)
#SBATCH --time=00:10:00          # total run time limit (HH:MM:SS)

hostname

module purge
module load intel/19.0/64/19.0.5.281 intel-mpi/intel/2018.3/64

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

srun $HOME/.local/bin/lmp_perseus_broadwell -sf omp -in in.melt
```

### Broadwell (xHost = avx2)

```
cmake3 -D CMAKE_INSTALL_PREFIX=$HOME/.local -D LAMMPS_MACHINE=perseus_broadwell -D ENABLE_TESTING=yes \
-D BUILD_MPI=yes -D BUILD_OMP=yes -D CMAKE_CXX_COMPILER=icpc -D CMAKE_BUILD_TYPE=Release \
-D CMAKE_CXX_FLAGS_RELEASE="-Ofast -xHost -mtune=broadwell -DNDEBUG" \
-D PKG_KSPACE=yes -D FFT=MKL -D FFT_SINGLE=yes \
-D PKG_USER-OMP=yes -D PKG_MOLECULE=yes -D PKG_RIGID=yes ../cmake
```
ntasks=1, cpus-per-tasks=28: t=310.8 s  
ntasks=28, cpus-per-tasks=1: t=72.5 s  
ntasks=4, cpus-per-tasks=7: t=123.4 s  
ntasks=7, cpus-per-tasks=4: t=102.7 s  
ntasks=14, cpus-per-tasks=1: t=148.7 s  
ntasks=56, cpus-per-tasks=1, nodes=2: t=35.4 s 
ntasks=84, cpus-per-tasks=1, nodes=3: t=23.2 s 

### Cascade Lake AP (avx512)

```
cmake3 -D CMAKE_INSTALL_PREFIX=$HOME/.local -D LAMMPS_MACHINE=perseus_ap_avx512 -D ENABLE_TESTING=yes \
-D BUILD_MPI=yes -D BUILD_OMP=yes -D CMAKE_CXX_COMPILER=icpc -D CMAKE_BUILD_TYPE=Release \
-D CMAKE_CXX_FLAGS_RELEASE="-Ofast -xCORE-AVX512 -mtune=cascadelake -DNDEBUG" \
-D PKG_KSPACE=yes -D FFT=MKL -D FFT_SINGLE=yes \
-D PKG_USER-OMP=yes -D PKG_MOLECULE=yes -D PKG_RIGID=yes ../cmake
```

ntasks=96, cpus-per-tasks=1: t=18.7 s  
ntasks=1, cpus-per-tasks=96: t=323.0 s 
ntasks=64, cpus-per-tasks=1, ntasks-per-socket=32: t=28.7 s  
ntasks=48, cpus-per-tasks=1: t=38.7 s  
ntasks=48, cpus-per-tasks=1, ntasks-per-socket=24: t=38.7 s  
ntasks=32, cpus-per-tasks=1, ntasks-per-socket=16: 50.7 s  

ntasks=48, cpus-per-tasks=1, ntasks-per-socket=24: t=39.0 s (-Ofast -xCORE-AVX512 -mtune=cascadelake -qopt-zmm-usage=high -mcmodel=medium -shared-intel -qopt-streaming-stores=always)  

### no vec on AP

ntasks=48, cpus-per-tasks=1, ntasks-per-socket=24: t=41.5 s

### Cascade Lake (avx512)

Here we use the same avx512 build as for the AP node.

ntasks=32, cpus-per-tasks=1: t=54.9 s  
ntasks=16, cpus-per-tasks=1, ntasks-per-socket=8: t=93.4 s  


### Cascade Lake AP (avx2)

```
cmake3 -D CMAKE_INSTALL_PREFIX=$HOME/.local -D LAMMPS_MACHINE=perseus_ap_avx2 -D ENABLE_TESTING=yes \
-D BUILD_MPI=yes -D BUILD_OMP=yes -D CMAKE_CXX_COMPILER=icpc -D CMAKE_BUILD_TYPE=Release \
-D CMAKE_CXX_FLAGS_RELEASE="-Ofast -xCORE-AVX2 -mtune=cascadelake -DNDEBUG" \
-D PKG_KSPACE=yes -D FFT=MKL -D FFT_SINGLE=yes \
-D PKG_USER-OMP=yes -D PKG_MOLECULE=yes -D PKG_RIGID=yes ../cmake
```

ntasks=96, cpus-per-tasks=1: t=21.5 s  
ntasks=48, cpus-per-tasks=1: t=37.3 s  

### Cascade Lake (avx2)

Here we use the same avx2 build as for the AP node.

ntasks=32, cpus-per-tasks=1: t=56.4 s  
ntasks=16, cpus-per-tasks=1: t=105.1 s  

### AMD (avx2)

```
cmake3 -D CMAKE_INSTALL_PREFIX=$HOME/.local -D LAMMPS_MACHINE=perseus_amd_avx2 -D ENABLE_TESTING=yes \
-D BUILD_MPI=yes -D BUILD_OMP=yes -D CMAKE_CXX_COMPILER=icpc -D CMAKE_BUILD_TYPE=Release \
-D CMAKE_CXX_FLAGS_RELEASE="-ipo -O3 -no-prec-div -fp-model fast=2 -march=core-avx2 -DNDEBUG" \
-D PKG_KSPACE=yes -D FFT=MKL -D FFT_SINGLE=yes \
-D PKG_USER-OMP=yes -D PKG_MOLECULE=yes -D PKG_RIGID=yes ../cmake
```

ntasks=64, cpus-per-tasks=1: t=24.9 s  
ntasks=48, cpus-per-tasks=1, ntasks-per-socket=24: t=35.5 s  
ntasks=32, cpus-per-tasks=1, ntasks-per-socket=16: t=53.5 s  


# LAMMPS (solvated peptide)

2.44 GB: FFTs, collectives, long-range forces

```
units		real
atom_style	full

pair_style	lj/charmm/coul/long 8.0 10.0 10.0
bond_style      harmonic
angle_style     charmm
dihedral_style  charmm
improper_style  harmonic
kspace_style	pppm 0.0001

read_data	../data.peptide
replicate       5 5 5

neighbor	2.0 bin
neigh_modify	delay 5

timestep	2.0

thermo_style	multi
thermo		100

fix		1 all nvt temp 275.0 275.0 100.0 tchain 1
fix		2 all shake 0.0001 10 100 b 4 6 8 10 12 14 18 a 31

group		peptide type <= 12

run		1000
```

### Broadwell (xHost = avx2)

ntasks=28, cpus-per-tasks=1: t=78.1 s  
ntasks=56, cpus-per-tasks=1, nodes=2: t=41.7 s  
ntasks=84, cpus-per-tasks=1, nodes=3: t=30.3 s  

### Cascade Lake AP (avx512)

ntasks=96, cpus-per-tasks=1: t=21.8 s  
ntasks=64, cpus-per-tasks=1, ntasks-per-socket=32: t=29.9 s  
ntasks=48, cpus-per-tasks=1, ntasks-per-socket=24: t=39.4 s  
ntasks=32, cpus-per-tasks=1, ntasks-per-socket=24: t=51.1 s  

### Cascade Lake AP (avx2)

ntasks=96, cpus-per-tasks=1: t=22.0 s  
ntasks=48, cpus-per-tasks=1: t=41.1 s  

### Cascade Lake (avx512)

ntasks=32, cpus-per-tasks=1: t=55.1 s  
ntasks=16, cpus-per-tasks=1, ntasks-per-socket=8: t=92.4 s  


### AMD (avx2)

ntasks=64, cpus-per-tasks=1: t=28.7 s  
ntasks=48, cpus-per-tasks=1, ntasks-per-socket=24: t=36.5 s  
ntasks=32, cpus-per-tasks=1, ntasks-per-socket=16: t=53.5 s  

```
#!/bin/bash
#SBATCH --job-name=peptide       # create a short name for your job
#SBATCH --nodes=1                # node count
#SBATCH --ntasks=64
#SBATCH --cpus-per-task=1        # cpu-cores per task (>1 if multi-threaded tasks)
#SBATCH --mem=250G
#SBATCH --time=00:10:00          # total run time limit (HH:MM:SS)
#SBATCH -p test
#SBATCH -C amd

hostname

numactl -H

module purge
module load intel/19.0/64/19.0.5.281 intel-mpi/intel/2018.3/64

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

srun $HOME/.local/bin/lmp_perseus_amd_avx2 -sf omp -in ../in.peptide
```

## GROMACS (rnase)

### Broadwell

```
module load intel/19.0/64/19.0.5.281 rh/devtoolset/8
module load intel-mpi/intel/2018.3/64

OPTFLAGS="-Ofast -xCORE-AVX2 -mtune=broadwell -DNDEBUG"
-DGMX_SIMD=AVX2_256 -DGMX_DOUBLE=OFF \
```
ntasks=28, cpus-per-tasks=1: t=62.7 s (threaded)  
ntasks=28, cpus-per-tasks=1: t=64.6 s  
ntasks=56, cpus-per-tasks=1 (nodes=2): t=35.2 s 
ntasks=84, cpus-per-tasks=1 (nodes=3): t=25.1 s 

### AP (avx512)

```
module purge
module load intel/19.0/64/19.0.5.281 intel-mpi/intel/2018.3/64 rh/devtoolset/8
OPTFLAGS="-Ofast -xCORE-AVX512 -mtune=cascadelake -DNDEBUG"
```

ntasks=96, cpus-per-tasks=1: t=21.8 s (threaded)  
ntasks=64, cpus-per-tasks=1, ntasks-per-socket=32: t=47.9 s (threaded)  
ntasks=32, cpus-per-tasks=1, ntasks-per-socket=16: t=42.8 s (threaded)  
ntasks=16, cpus-per-tasks=4, ntasks-per-socket=8: t=116.2 s (threaded)  
ntasks=2, cpus-per-tasks=32, ntasks-per-socket=1, t=47.2 s (threaded)  

ntasks=96, cpus-per-tasks=1: t=21.4 s (mpi)
ntasks=64, cpus-per-tasks=1, ntasks-per-socket=32: t=30.2 s (mpi)  
ntasks=48, cpus-per-tasks=1, ntasks-per-socket=24: t=32.7 s (mpi)  
ntasks=36, cpus-per-tasks=1, ntasks-per-socket=18: t=40.1 s (mpi)  
ntasks=24, cpus-per-tasks=1, ntasks-per-socket=12: t=50.9 s (mpi)  

### CL (avx512)
ntasks=32, cpus-per-tasks=1, ntasks-per-socket=16: t=45.8 s (mpi)  
ntasks=24, cpus-per-tasks=1, ntasks-per-socket=12: t=55.8 s (mpi)  

### AMD (gcc/openmpi)
ntasks=32, cpus-per-tasks=1, ntasks-per-socket=16: t= 46.9 s (non-mpi)  
ntasks=32, cpus-per-tasks=1, ntasks-per-socket=16: t= 47.9 s (non-mpi gcc9)

### AMD (-GMX_SIMD=AVX2_128 and OPTFLAGS="-Ofast -DNDEBUG")
t=58.2 s (non-mpi)
t=64.1 s (mpi)
module load intel/19.1/64/19.1.1.217 rh/devtoolset/8
module load intel-mpi/intel/2018.3/64
```
CPU info:
    Vendor: AMD
    Brand:  AMD EPYC 7542 32-Core Processor                
    Family: 23   Model: 49   Stepping: 0
    Features: aes amd apic avx avx2 clfsh cmov cx8 cx16 f16c fma htt lahf misalignsse mmx msr nonstop_tsc pclmuldq pdpe1gb popcnt pse rdrnd rdtscp sha sse2 sse3 sse4a sse4.1 sse4.2 ssse3 x2apic
```
