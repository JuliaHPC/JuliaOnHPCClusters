+++
title = "User: FAQ"
hascode = true
literate_mds = true
showall = false
noeval = true
+++

# User: FAQ

---

\label{content}
**Content**

\toc

---

## Should I compile Julia from source?

**No, you should not.** Use the pre-built binaries from the website or, preferrably, a corresponding Lmod module provided by the cluster admins. Compiling Julia from source will generally **not** improve the performance of your code (Julia is a compiler in itself) but may very well be cumbersome. Unless you know what you're doing and have a very good reason (e.g. creating a debug build with flags like `USE_INTEL_JITEVENTS=1`) you should not compile Julia yourself.

[⤴ _**back to Content**_](#content)

## Where should I put the Julia depot?

Ideally, you should set `JULIA_DEPOT_PATH` to point to a place with the following properties:

* good (parallel) I/O
* no tight quotas
* read and write access
* no automatic deletion of unused files (or otherwise you have to find a workaround)

On some systems, the best place might be on the parallel file system. On others, it might simply be in `$HOME`.

[⤴ _**back to Content**_](#content)

## How should I start Julia within a job script?

Assuming that SLURM is used on your HPC cluster, you should generally start Julia under `srun`, e.g. `srun julia --project mycode.jl`. This is especially important if your code is MPI-parallel, in which case `srun` is a replacement for `mpirun`/`mpiexec`, but also recommended for serial code (there are at least [a few reasons](https://stackoverflow.com/a/53640511/2365675)).

[⤴ _**back to Content**_](#content)

## How can I force Julia to compile code for a heterogeneous cluster?

Set [`JULIA_CPU_TARGET`](https://docs.julialang.org/en/v1.10-dev/manual/environment-variables/#JULIA_CPU_TARGET) to a value that is generic enough to cover all the types of CPUs that you're targeting.

Example: `JULIA_CPU_TARGET="generic;skylake-avx512,clone_all;znver2,clone_all"`.

[⤴ _**back to Content**_](#content)