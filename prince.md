
# Modules:

```bash
  module avail
  module load <package-name>
  module unload <package-name>
  module list
```

- Compilers:
  - intel/17.0.1
  - gcc/6.3.0
- MPI:
  - openmpi/gnu/2.0.2
  - openmpi/gnu/cuda/2.0.3
  - openmpi/intel/2.0.3
- BLAS, LAPACK:
  - intel/17.0.1 (MKL)
- FFTW3:
  - fftw/intel/3.3.6-pl2
- PETSc:
  - petsc/openmpi/intel/3.7.6
- CUDA:
  - cuda/9.2.88
- Profiling:
  - gperftools/intel/2.5
- Debugging:
  - valgrind/gnu/3.12.0


# SLURM User Guide

## Interactive Job:

```bash
  srun <options> --pty /bin/bash
```

IvyBridge CPU nodes (20-cores @3.0GHz, 62GB memory):
```bash
  srun --nodes 4 --ntasks-per-node=1 --cpus-per-task=20 --mem=62GB --time=5:00:00 --partition="c28,c29,c30,c31,c32_41" --pty /bin/bash
```

Broadwell CPU nodes (28-cores @ 2.6GHz, 125GB memory):
```bash
  srun --nodes 2 --ntasks-per-node=1 --cpus-per-task=28 --mem=125GB --time=5:00:00 --partition="c01_17" --pty /bin/bash
```

Broadwell GPU nodes (28-cores @2.6GHz, 125GB memory, 4 Tesla P40):
```bash
  srun --nodes 1 --ntasks-per-node=1 --time=5:00:00 --gres=gpu:p40:4 -c28 --pty /bin/bash
```

Skylake GPU nodes (40-cores @2.4GHz, 384GB memory, 4 Tesla V100 SXM2):
```bash
  srun --nodes 1 --ntasks-per-node=1 --time=1:00:00 --partition="v100_sxm2_4" --gres=gpu:4 -c20 --pty /bin/bash
```


## Job Scripts:

```bash
  sbatch <script-file-name>
```

Script file structure:
```bash
  #!/bin/bash
  #SBATCH <options>
  mpirun ... # TODO
```

## Job Options:

-  --nodes=1
-  --ntasks-per-node=1
-  --cpus-per-task=20
-  --time=5:00:00
-  --mem=2GB
-  --partition="partition-list"
-  --gres=gpu:k80:3 -c3


## Cancel Job:

```bash
  scancel jobid
```

## Miscellaneous:

```bash
  squeue -u $USER # list jobs and status
  sinfo           # list node-partitions and status
```

## References:

* https://wikis.nyu.edu/display/NYUHPC/Getting+started+on+Prince
* https://wikis.nyu.edu/display/NYUHPC/Accessing+the+Prince+cluster
* https://wikis.nyu.edu/display/NYUHPC/Slurm+Tutorial
* https://wikis.nyu.edu/display/NYUHPC/Requesting+resources+via+Slurm
* https://wikis.nyu.edu/display/NYUHPC/Running+interactive+jobs
* https://wikis.nyu.edu/display/NYUHPC/Putting+all+pieces+together
* https://github.com/cvalenzuela/hpc




# Profiling Tools

## gperftools (Google performance tools):

```bash
icc -I $GPERFTOOLS_ROOT/include test.c -L $GPERFTOOLS_ROOT/lib -lprofiler
pprof --pdf ./a.out profile.log > profile.pdf
pprof --callgrind ./a.out profile.log > profile.callgrind
kcachegrind profile.callgrind
```

## gprof:

```bash
icc -p test.c
./a.out
gprof ./a.out
```

```bash
gcc -pg test.c
./a.out
gprof ./a.out
```

## References

- ** http://gernotklingler.com/blog/gprof-valgrind-gperftools-evaluation-tools-application-level-cpu-profiling-linux/ **
- http://wwwpub.zih.tu-dresden.de/~mlieber/practical_debugging/05_ddt_totalview.pdf
- https://portal.tacc.utexas.edu/documents/13601/1041435/29-Overview_of_Profiling.pdf/84359111-d21a-4618-9d90-ca878c1e37ab
- https://gperftools.github.io/gperftools/cpuprofile.html
- http://heather.cs.ucdavis.edu/~matloff/pardebug.html

