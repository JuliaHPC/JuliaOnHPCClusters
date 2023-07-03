+++
title = "FAQ"
hascode = true
literate_mds = true
showall = false
noeval = true
+++

# FAQ

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

## Should I use Distributed.jl or MPI.jl for large-scale parallelism?

While the Distributed standard library provides some convenient tools and has its use cases you should generally use MPI.jl to scale your code up (e.g. to thousands of compute nodes). Not only is MPI the established standard for distributed computing in any programming language, it also makes use of fast interconnects in HPC clusters (which the Distributed standard library currently doesn't).

[⤴ _**back to Content**_](#content)

## Should I use Julia artifacts (JLLs) or system software?

If JLLs work fine for you then use them. JLLs have the big advantage that they are convenient and, in many cases, "just work" out of the box. System software can (but doesn't necessarily) give better performance but overriding the relevant bits of JLLs can be cumbersome. Generelly speaking, we recommend to only manually replace JLL libraries by system software if JLLs don't work (e.g. if a vendor specific MPI is required). However, in such a case it would be even better to nudge the HPC admins and make this setup permanent and generally available in form of a Julia Lmod module.

[⤴ _**back to Content**_](#content)

## How to cope with a large number of MPI processes accessing the same Julia depot?

In a distributed computing scenario with, e.g., multiple thousands of Julia (MPI) processes, accessing the same Julia depot on a shared file system - when loading packages and precompiled cache files on `using PackageX` - can become (very) time consuming. A workaround is to bundle up the Julia depot (e.g. as a `.tar.gz`), distribute it to the local node storage (if available) or local memory (often times mounted as `/tmp`) of all assigned compute nodes, and then set the `JULIA_DEPOT_PATH` accordingly.

[⤴ _**back to Content**_](#content)
