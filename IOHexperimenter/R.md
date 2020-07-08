---
layout: page
title: IOHexperimenter - R Interface
parent: IOHexperimenter
permalink: /IOHexperimenter/R/
--- 


## Installation
The IOHexperimenter is now available on [CRAN](https://CRAN.R-project.org/package=IOHexperimenter), and can be installed using:

```r
install.packages('IOHexperimenter')
```

Alternatively, the development version can be downloaded by either cloning this repository from [GitHub](https://github.com/IOHprofiler/IOHexperimenter.git) and installing locally, or use the following commands to use devtools to install latest version from our GitHub:

If devtools is not yet installed, please first use

```r
install.packages('devtools')
```

Error messages will be shown in your R console if there is any installation issue.
Now, the development version of IOHexperimenter can be installed and loaded using the following commands:

```r
devtools::install_github('IOHprofiler/IOHexperimenter@R')
```

You can then verify wether the pacakge can be loaded by using:

```r
library('IOHexperimenter')
```
## Usage

To benchmark your algorithm, you should first create a wrapper around it which accepts an `IOHproblem` object as its first parameter. This is an S3-object which contains the following information about the current problem:

* dimension
* function_id
* instance
* fopt (if known)
* xopt (if known)
* lower bound
* upper bound
* maximization / minimization
* suite

And the following functions:

* obj_func()
* target_hit()
* set_parameters()

Several example algorithms with corresponding wrappers have been implemented (in the `algorithms.R` file). These algorithms are:
- IOH_random_search (can work on functions from either PBO or COCO suites)
- IOH_random_local_search (only for PBO functions)
- IOH_self_adaptive_GA (only for PBO functions)
- IOH_two_rate_GA (only for PBO functions)

Once your algorithm is compatible with an IOHproblem, you can benchmark it using the `benchmark_algorithm` function, with as the first parameter your (wrapped) algorithm. 
For information about how to configure this benchmarking procedure, please refer to the internal documentation in R, accesible by using `??benchmark_algorithm`.
