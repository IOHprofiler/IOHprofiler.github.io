---
layout: page
title: IOHexperimenter
has_children: true
permalink: /IOHexp/
description: Experimenting IOHs
---

__Experimenter__ for **I**terative **O**ptimization **H**euristics (IOHs), built natively in* `C++`.

* __Documentation__: [https://arxiv.org/abs/1810.05281](https://arxiv.org/abs/1810.05281)
* __Wiki page__: [https://iohprofiler.github.io](https://iohprofiler.github.io/)
* __General Contact__: [iohprofiler@liacs.leidenuniv.nl](iohprofiler@liacs.leidenuniv.nl)
<!-- * __Mailing List__: [https://lists.leidenuniv.nl/mailman/listinfo/iohprofiler](https://lists.leidenuniv.nl/mailman/listinfo/iohprofiler) -->

**IOHexperimenter** *provides*:

* A framework to ease the benchmarking of any iterative optimization heuristic
* Benchmarking problems consisting of [pseudo-Boolean Optimization (PBO)](https://iohprofiler.github.io/IOHproblem/) problem set (25 pseudo-Boolean problems) and integration of the well-known [Black-black Optimization Benchmarking (BBOB)](https://github.com/numbbo/coco) problem set (24 continuous problems)
* Interface for adding new problems and suite/problem set
* Advanced logging module that takes care of registering the data in a seamless manner
* Data format is compatible with [IOHanalyzer](https://github.com/IOHprofiler/IOHanalyzer)

**IOHexperimenter** is available for:

* `C++` manual can be found [here](https://iohprofiler.github.io/IOHexp/Cpp/)
* `Python`: please see [here](https://github.com/IOHprofiler/IOHexperimenter/tree/master/ioh) for details user manual
* or as a [pip package](https://pypi.org/project/ioh); [Wiki Page](https://iohprofiler.github.io/IOHexp/python/).