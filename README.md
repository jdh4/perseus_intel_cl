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
