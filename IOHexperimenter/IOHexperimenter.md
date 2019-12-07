---
layout: page
title: IOHexperimenter
has_children: true
permalink: /IOHexperimenter/
description: Experimenting IOHs
---

This is the **benchmarking framework** for <b>I</b>terative <b>O</b>ptimization <b>H</b>euristics (IOHs).
<b>IOHexperimenter</b> provides easy-to-use benchmarking functionalities, including:

* A framework for straightforward benchmarking of any iterative optimization heuristic
* A generic framework to generate benchmarking suite for the optimization task you're insterested in,
* A _Pseudo-Boolean Optimization_ ([PBO](/Suites/PBO/)) benchmark suite, containing 23 test problems of the kind $f\colon \\{0,1\\}^d \rightarrow \mathbb{R}$, and
* The integration of 24 noiseless, single-objective _Black-Box Optimization Benchmarking_ ([BBOB](https://coco.gforge.inria.fr/downloads/download16.00/bbobdocfunctions.pdf)) functions on the continuous domain, namely $f\colon \mathbb{R}^d \rightarrow \mathbb{R}$. <b>We directly take the <tt>C</tt> implementation of BBOB test functions from [https://github.com/numbbo/coco](https://github.com/numbbo/coco), with some modifications to accommodate our framework.</b>
* Logging methods to effortlesly store benchmarking data in a format compatible with __IOHanalyzer__, with future support for additional data logging options
<!-- * (__Soon to come__) A framework which significantly simplifies algorithm designa -->

<b>IOHexperimenter</b> is available for:

* `C++` on [GitHub](https://github.com/IOHprofiler)
* `R`, as a package on [GitHub](https://github.com/IOHprofiler/IOHexperimenter/tree/R) (the stabble release will be pushed to CRAN soon)
* `Python` (under development)
* `Java` (under development)


## Prerequisite

Before installing <b>IOHexperimenter</b>, it is necessary to install the following dependencies:

* `C++` (tested on `gcc 5.4.0`)
* `boost.filesystem` library for logging files.

To install boost library for `C++`, please visit [https://www.boost.org](https://www.boost.org). Once boost is installed, you are now able to use the IOHexperimenter in the programming language of your choise.

## Links

* __Code repository__ : [https://github.com/IOHprofiler/IOHexperimenter](https://github.com/IOHprofiler/IOHexperimenter)
* __Bug reports__: [IOHexperimenter](https://github.com/IOHprofiler/IOHexperimenter/issues)
<!-- * __General Contact__: [iohprofiler@liacs.leidenuniv.nl](mailto:iohprofiler@liacs.leidenuniv.nl) -->
<!-- * __Mailing List__: [IOHprofiler mailing list](https://lists.leidenuniv.nl/mailman/listinfo/iohprofiler) -->
