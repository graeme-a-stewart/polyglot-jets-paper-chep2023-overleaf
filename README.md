# Polyglot Jet Finding

This is the proceedings paper for Polyglot Jet Finding at CHEP2023, Indico reference: <https://indico.jlab.org/event/459/contributions/11540/>

## Benchmarks

### Code

The benchmarks in this paper are obtained from running the following codes:

| Language | Repository | Release/Commit |
|---|---|---|
| C++ | <http://fastjet.fr/all-releases.html> | v3.4.1 |
| Python | https://github.com/graeme-a-stewart/antikt-python | `e607f119f2d061723606a95f9cb1428cff53025f` |
| Julia | https://github.com/JuliaHEP/JetReconstruction.jl | `b79b941764f29b996fc7fe49c96cd29b439fbeaf` |

### Host

The machine on which benchmark numbers were obtained was a AMD EPYC 7302 \@ 3.00GHz with 24GB RAM,
running CentOS7. The LCG_104 release was used to compile FastJet.

### Software Versions

| Software | Version |
|---|---|
| C++ Compiler | gcc 11.3.0 |
| Python | Python 3.11.4 |
| numba | 0.57.1 |
| numpy | 1.24.4 |
| Julia | 1.9.2 |
### Compilation and Installation Notes

Both the Python and Julia repositories have the input HepMC3 event sample stored in a gzip format. This just needs to be unpacked in the usual way, e.g.,

```sh
gunzip -c events.hepmc3.gz > events.hepmc3
```

#### C++ FastJet

The FastJet libraries were compiled with the default`-O2` level of optimisation.

The FastJet profiling binary is in the [antikt-python](https://github.com/graeme-a-stewart/antikt-python/tree/main/fastjet) repository. To compile it one needs to have the FastJet and HepMC3 libraries available (any version of HepMC3 should work, but for this study `3.2.6` was used). A minimal `Makefile` should work for most cases - just edit it for your system to point to where FastJet and HepMC3 are installed by changing `HEPMC3_DIR` and `FASTJET_DIR`. Depending on your system setup you may need to define `LD_LIBRARY_PATH` (Linux) or `DYLD_LIBRARY_PATH` (OS X) to run the `chep-polyglot-jets` binary.

#### Python

The Python repository contains a conda-style set of dependencies in `environment.yml`. You should be able to setup a correct Python environment as follows:

```sh
conda create -f environment.yml
conda activate antikt-python
```

[Mamba](https://mamba.readthedocs.io/en/latest/) and [micromamba](https://mamba.readthedocs.io/en/latest/micromamba-installation.html) work equally well.

#### Julia

The Julia repository has dependencies in the usual `Project.toml` file. The environment can be initialised in the usual Julia manner from the git checkout, e.g.,

```julia
julia --project=.
               _
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.9.2 (2023-07-05)
 _/ |\__'_|_|_|\__'_|  |  Official https://julialang.org/ release
|__/                   |

julia> using Pkg

julia> Pkg.instantiate()
    Updating registry at `~/.julia/registries/General.toml`
    Updating `~/tmp/JetReconstruction.jl/Project.toml`
...

julia> 
```

(Or enter the package manager from the REPL, with `]`, and just issue `instantiate` there.)

### Benchmark Commands

| Language | Algorithm | Command |
|---|---|---|
| C++ | N2Basic | `./chep-polyglot-jets ../data/events.hepmc3 100 100 N2Plain` |
| C++ | N2Tiled | `./chep-polyglot-jets ../data/events.hepmc3 100 100 N2Tiled` |
| Python (Pure) | N2Basic | `./antikt-basic.py --maxevents=100 --trials 16 ../data/events.hepmc3` |
| Python (Accelerated) | N2Basic | `./antikt-basic.py --maxevents=100 --numba --trials 16 ../data/events.hepmc3` |
| Python (Pure) | N2Tiled | `./antikt-tiledN2cluster.py --maxevents=100 --trials 16 ../data/events.hepmc3` |
| Python (Accelerated) | N2Tiled | `./antikt-tiledN2cluster.py --maxevents=100 --trials 16 --numba ../data/events.hepmc3` |
| Julia | N2Basic | `julia --project=. chep.jl --maxevents=100 --nsamples=100 --strategy=N2Plain test/data/events.hepmc3 --gcoff` |
| Julia | N2Tiled | `julia --project=. chep.jl --maxevents=100 --nsamples=100 --strategy=N2TiledLL test/data/events.hepmc3 --gcoff` |

### CHEP Results

The proceedings paper has slightly updated versions of the Julia N2Tiled algorithm, giving an additional speed up over the results presented in the CHEP talk.

For the CHEP talk the results were obtained by running the original implementation of the code:

| Repository | Commit |
|---|---|
| <https://github.com/grasph/AntiKt.jl> | `ce22a34d67763ce758c7e232b9f7f44bc2506e61` |

The code is run as follows:

```sh
julia --project=. ./run_antikt_jl --nsamples=100 ../data/events.hepmc3 --gcoff
```
