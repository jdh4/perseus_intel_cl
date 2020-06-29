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

[coming soon...]
3) AMD Rome ()
   #SBATCH -C amd
```

## Create 4 GB array of doubles in NumPy

```
import numpy as np
from time import perf_counter

t0 = perf_counter()
N = 500_000_000
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

ap
17.8 s

cl
17.3

old perseus
18.4

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
