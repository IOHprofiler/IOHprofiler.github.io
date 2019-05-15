---
layout: page
title: IOHexperimenter
nav_order: 3
has_children: true
permalink: /IOHexperimenter/
---

IOHprofiler: A Benchmarking and Profiling Tool for Iterative Optimization Heuristics
============================================

IOHprofiler is a new tool for analyzing and comparing iterative optimization heuristics.
Given as input algorithms and problems written in C, C++, or Python*, it provides as output an in-depth statistical evaluation of the algorithms’ fixed-target and fixed-budget running time distributions. In addition to these performance evaluations, IOHprofiler also allows to track the evolution of algorithm parameters, making our tool particularly useful for the analysis, comparison, and design of (self-)adaptive algorithms.

IOHprofiler is a ready-to-use software. It consists of two parts: IOHExperimenter, which generates the running time data; and IOHanalyzer, which produces the summarizing comparisons and statistical evaluations. Currently IOHExperimenter is built on the [COCO](https://github.com/numbbo/coco) software, but a new C++ based version is developing and will be released soon.

[This code] implements the experimentation tool of IOHprofiler. 
For the analyzer part, please visit [IOHanalyzer page](https://github.com/IOHprofiler/IOHanalyzer).



Requirements  <a name="Requirements"></a>
------------
1. The experimentation of IOHprofiler has been tested with:
  - gcc 5.4.1
  - python 2.7.12
  - R 3.4.4 and above (should also work on other versions)
2. For a machine running the anylizing process,
  - Visit [IOHanalyzer page](https://github.com/IOHprofiler/IOHanalyzer)


Getting Started <a name="Getting-Started"></a>
---------------
0. Check out the [_Requirements_](#Requirements) above.

1a. To use the R-version of the IOHexperimenter, please look at the R-package branch of this repository [link](https://github.com/IOHprofiler/IOHexperimenter/tree/R-package)

1b. For the other versions of the IOHexperimenter, **Download** IOHprofiler experimentation code from github [link](https://github.com/IOHprofiler/IOHExperimenter) and unzip the `zip` file, or **type** `git clone https://github.com/IOHprofiler/Experimentation.git` (`git` needs to be installed)

2. In a system shell, **`cd` into** the `Experimenter` folder, 
  where the file `do.py` can be found. **execute** the following statement:
  ```
    python do.py `options`
  ```  
  Available options are:

    - `build-c` Builds the C module. Two files, IOHprofiler.c and IOHprofiler.h, will be generated at `code-experiment/build/c`

    - `run-c` Builds the C module and runs as an example some experiments in C.

    - `build-python` Builds the python module. A python package named IOHprofiler will be installed

    - `run-python` Builds the python module and runs as an example some experiments in python.

  
3. Using the analyzer by visiting [IOHanalyzer page](https://github.com/IOHprofiler/IOHanalyzer)
