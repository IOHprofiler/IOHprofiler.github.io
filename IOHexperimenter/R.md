---
layout: page
title: IOHexperimenter - R Interface
parent: IOHexperimenter
permalink: /IOHexperimenter/R/
--- 


## Installation
To use this package, either clone this repository from [GitHub](https://github.com/IOHprofiler/IOHexperimenter.git) and install locally, or use the following commands to use devtools to install the package directely:

If devtools is not yet installed, please first use

```r
install.packages('devtools')
```

Error messages will be shown in your R console if there is any installation issue.
Now, the IOHexperimenter package can be installed and loaded using the following commands:

```r
devtools::install_github('IOHprofiler/IOHexperimenter@R')
library('IOHexperimenter')
```

This will install the package and all required dependencies.

## Usage

To benchmark your algorithm, you should first create a wrapper around it which accepts an `IOHproblem` object as its first parameter. This object contains the following information about the current problem:

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

Several example algorithms with corresponding wrappers have been implemented in the `algorithms.R` file.

Once your algorithm is compatible with an IOHproblem, you can benchmark it using the `benchmark_algorithm` function, with as the first parameter your (wrapped) algorithm. For information about how to configure this benchmarking procedure, please refer to the internal documentation in R, accesible by using `??benchmark_algorithm`.