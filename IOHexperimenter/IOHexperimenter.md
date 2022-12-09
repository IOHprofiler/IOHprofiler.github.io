---
layout: page
title: IOHexperimenter
has_children: true
permalink: /IOHexp/
description: Iterative Optimization Heuristics Profiler
---

**Experimenter** for **I**terative **O**ptimization **H**euristics (IOHs), built natively in* `C++`.

* **Documentation**: [https://iohprofiler.github.io/IOHexperimenter](https://iohprofiler.github.io/IOHexperimenter)
* **Publication**: [https://arxiv.org/abs/2111.04077](https://arxiv.org/abs/2111.04077)

**IOHexperimenter** *provides*:

* A framework to ease the benchmarking of any iterative optimization heuristic
<!-- * Continuous and discrete benchmarking problems -->
* [Pseudo-Boolean Optimization (PBO)](https://iohprofiler.github.io/IOHproblem/) problem set (25 pseudo-Boolean problems)
* Integration of the well-known [Black-black Optimization Benchmarking (BBOB)](https://github.com/numbbo/coco) problem set (24 continuous problems)
* [W-model](https://dl.acm.org/doi/abs/10.1145/3205651.3208240?casa_token=S4U_Pi9f6MwAAAAA:U9ztNTPwmupT8K3GamWZfBL7-8fqjxPtr_kprv51vdwA-REsp0EyOFGa99BtbANb0XbqyrVg795hIw) problem sets constructed on OneMax and LeadingOnes
* Integration of the [Tree Decomposition (TD) Mk Landscapes](https://github.com/tobiasvandriessel/problem-generator) problems
* Integration of the submodular optimization problems in [Competition - Evolutionary Submodular Optimisation GECCO 2022](https://cs.adelaide.edu.au/~optlog/CompetitionESO2022.php)
* Interface for adding new problems and suite/problem set
* Advanced logging module that takes care of registering the data in a seamless manner
* Data format is compatible with [IOHanalyzer](https://iohprofiler.github.io/IOHanalyzer/)


<!-- ## Get Started -->

### C++ Interface

* Instructions regarding installation and a quickstart guide can be found on [GitHub](https://github.com/IOHprofiler/IOHexperimenter/tree/master/example)
* A full API documentation is available [here](https://iohprofiler.github.io/IOHexperimenter/cpp.html)

### Python Interface

* [pip package](https://pypi.org/project/ioh)
* A extensive tutorial can be found in the python [notebook](https://github.com/IOHprofiler/IOHexperimenter/blob/master/example/tutorial.ipynb)
* A more condensed set of examples can be found [here](https://github.com/IOHprofiler/IOHexperimenter/blob/master/ioh/README.md)
* A full API documentation is available [here](https://iohprofiler.github.io/IOHexperimenter/python.html)
