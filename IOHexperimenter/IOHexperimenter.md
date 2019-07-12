---
layout: page
title: IOHexperimenter
nav_order: 3
has_children: true
permalink: /IOHexperimenter/
---

IOHexperimenter
============================================

The __benchmarking platform__ for <b>I</b>terative <b>O</b>ptimization <b>H</b>euristics (IOHs).

* __Documentation__: [https://arxiv.org/abs/1810.05281](https://arxiv.org/abs/1810.05281)
* __Wiki page__: [https://iohprofiler.github.io/IOHanalyzer](https://iohprofiler.github.io/IOHanalyzer)
* __General Contact__: [iohprofiler@liacs.leidenuniv.nl](iohprofiler@liacs.leidenuniv.nl)
* __Mailing List__: [https://lists.leidenuniv.nl/mailman/listinfo/iohprofiler](https://lists.leidenuniv.nl/mailman/listinfo/iohprofiler)

<b>IOHexperimenter</b> <i>provides</i>:

* A framework for straightforward benchmarking of any iterative optimization heuristic
* A suite consisting of 23 pre-made Pseudo-Boolean benchmarking function, with easily accessible methods for adding custom functions and suites 
* Logging methods to effortlesly store benchmarking data in a format compatible with __IOHanalyzer__, with future support for additional data logging options
* (__Soon to come__:) A framework which significantly simplifies algorithm design

<b>IOHexperimenter</b> is <i>built on</i>:

* `C++` (tested on `gcc 5.4.0`)
* `boost.filesystem` library for logging files.

<b>IOHexperimenter</b> is available for:

* `C++`
* `R` package [https://github.com/IOHprofiler/IOHexperimenter/tree/R](https://github.com/IOHprofiler/IOHexperimenter/tree/R)
* `Python` interface (soon to come)
* `Java` interface (soon to come)

## Using IOHexperimenter

### Running Experiments

The __IOHexperimenter__ has been built on `C++` and tested on complier `gcc 5.4.0`. To use the logging of `csv` output files, `boost.filesystem` library is required (To install boost library, please visit [https://www.boost.org](https://www.boost.org)).

#### Using by C++

If you are using the tool for the first time, please download or clone this branch and run `make` at the root directory of the project. After running `make` to compile,
* object files will be generated in `build/c/obj`
* three exectuable files will be generated in `build/c/bin`

Afterwards, you can use the folder `build/c` and use the `Makefile` therein for your experiments.
For more details of how to use the `C++` version, please visit [this page](/IOHexperimenter/Experiments/).

#### Using by R
For the use of `R`, please visit [R branch](https://github.com/IOHprofiler/IOHexperimenter/tree/R).

### Creating test problems

Benchmarking problems in __IOHexperimenter__ are easy to create yourself. We provide support for any input type and any number of real-valued objectives. For a more detailed guidline of how to define a benchmarking problem within IOHexperimenter, please visit [this page](/IOHexperimenter/AddingProblems/).

### Configuring test suites
Suites are collections of benchmarking problems. By including problems into a suite, it is easier for users to maintain their experiments. If you create a set of similar problems, it is recommended to create a suite to collect them together, which can be done effortlesly within the IOHexperimenter. For detailed steps of creating and using suites, please visit [this page](/src/Suites).