# Polyglot Jet Finding

This is the proceedings paper for Polyglot Jet Finding at CHEP2023, Indico reference: <https://indico.jlab.org/event/459/contributions/11540/>

## Benchmarks

### Code

The benchmarks in this paper are obtained from running the following codes:

| Language | Repository | Release/Commit |
|---|---|---|
| C++ | <http://fastjet.fr/all-releases.html> | v3.4.1 |
| Python | https://github.com/graeme-a-stewart/antikt-python | `5f3b5d636a4d12156f3925eeb7aa0a454ae0746a` |
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

#### Compilation and Installation Notes

The **FastJet** libraries were compiled with the default`-O2` level of optimisation. In order to benchmark FastJet a small application was written, which can be found [here](https://github.com/graeme-a-stewart/antikt-python/tree/main/fastjet).

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
