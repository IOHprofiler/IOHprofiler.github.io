---
layout: page
title: IOHexperimenter
nav_order: 3
has_children: true
permalink: /IOHexperimenter/
description: Configure your experiments easily
---

IOHexperimenter
============================================

The __benchmarking platform__ for <b>I</b>terative <b>O</b>ptimization <b>H</b>euristics (IOHs).

* __Code repository__ : [IOHexperimenter GitHub Page](https://github.com/IOHprofiler/IOHexperimenter)
* __Documentation__: [Arxiv Verion of "IOHprofiler: A Benchmarking and Profiling Tool for Iterative Optimization Heuristics"](https://arxiv.org/abs/1810.05281)
* __Bug reports__: [IOHexperimenter](https://github.com/IOHprofiler/IOHexperimenter/issues)
* __General Contact__: [iohprofiler@liacs.leidenuniv.nl](mailto:iohprofiler@liacs.leidenuniv.nl)
* __Mailing List__: [IOHprofiler mailing list](https://lists.leidenuniv.nl/mailman/listinfo/iohprofiler)

<b>IOHexperimenter</b> provides easy-to-use benchmarking functionality, including:
* A framework for straightforward benchmarking of any iterative optimization heuristic
* Generic benchmarking procedure using suites, with two pre-installed suites: [PBO](Benchmark/) for pseudo-boolean optimization and [BBOB](https://coco.gforge.inria.fr/downloads/download16.00/bbobdocfunctions.pdf) for continuous.
* Logging methods to effortlesly store benchmarking data in a format compatible with __IOHanalyzer__, with future support for additional data logging options
* (__Soon to come__) A framework which significantly simplifies algorithm design

## Usage

To use IOHexperimenter, please use the following tutorials:
* [Preparation](Preparation/)
* [Benchmarking using C++](Cpp/)
* [Benchmarking using R](R/)
* [Adding new Functions / Suites](Adding-Functions/)

<!-- <b>IOHexperimenter</b> is <i>built on</i>:

* `C++` (tested on `gcc 5.4.0`)
* `boost.filesystem` library for logging files.

<b>IOHexperimenter</b> is available for:

* `C++` on [GitHub](https://github.com/IOHprofiler)
* `R`, as a package on [GitHub](https://github.com/IOHprofiler/IOHexperimenter/tree/R) (or on on CRAN in the future)
* `Python` (under development)
* `Java` (at a later date) -->

<!-- #### Using IOHexperimenter in C++

If you are using the tool for the first time, please download or clone this branch and run `make` at the root directory of the project. After running `make` to compile,
* object files will be generated in `build/c/obj`
* three exectuable files will be generated in `build/c/bin`

Afterwards, you can use the folder `build/c` and use the `Makefile` therein for your experiments.
For more details of how to use the `C++` version, please visit [this page](/IOHexperimenter/Experiments/).

#### Using IOHexperimenter in R
For the use of `R`, please visit [R branch](https://github.com/IOHprofiler/IOHexperimenter/tree/R).

### Creating test problems

Benchmarking problems in __IOHexperimenter__ are easy to create yourself. We provide support for any input type and any number of real-valued objectives. For a more detailed guidline of how to define a benchmarking problem within IOHexperimenter, please visit [this page](/IOHexperimenter/AddingProblems/).

### Configuring test suites
Suites are collections of benchmarking problems. By including problems into a suite, it is easier for users to maintain their experiments. If you create a set of similar problems, it is recommended to create a suite to collect them together, which can be done effortlesly within the IOHexperimenter. For detailed steps of creating and using suites, please visit [this page](/src/Suites). -->