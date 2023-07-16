---
layout: page
title: IOHanalyzer
nav_order: 4
has_children: true
permalink: /IOHanalyzer/
---

This is the __performance analyzer__ of **I**terative **O**ptimization **H**euristics (IOHs), e.g., Evolutionary Algorithms. **IOHanalyzer** takes as input the benchmark data from the user and provides very detailed analysis on, e.g., the empirical running time.

![]({{ site.url }}/assets/fig/demo.gif)

It _provides_:

* A web-based Graphical User Interface (GUI).
* Interactive plotting in two perspectives (fixed-budget and fixed-target) of both performance and tracked parameters.
* Statistical evaluation methods.
* `R` programming interfaces in the backend.

It _takes as input_:

* **COCO/BBOB** data format (see [https://github.com/numbbo/coco](https://github.com/numbbo/coco)),
* **IOHprofiler** data format, which is motivated and modified from **COCO/BBOB** data format (Please read the [data format section](/IOHanalyzer/data/) for specifications on the supported data format), or
* **Nevergrad** data format (explained in [https://github.com/facebookresearch/nevergrad](https://github.com/facebookresearch/nevergrad)).

It is _built on_:

* `R` packages [data.table](https://cran.r-project.org/web/packages/data.table/), [Shiny](https://shiny.rstudio.com/), [Plotly](https://plot.ly/) and [Rcpp](http://www.rcpp.org/).
<!-- * [scmacp](https://github.com/b0rxa/scmamp) package for Bayesian analysis. -->

It is _available through_:

* a free online service available at [http://iohanalyzer.liacs.nl](http://iohanalyzer.liacs.nl), and
* a local [installation](#install) of the package.

## <a name="install"></a>Installation

### Software dependency

* **[mandatory]** `R` As __IOHanalyzer__ is written as a `R` package, the `R` environment has to be installed first. The binary file and installation manual for R can be found at [https://cran.r-project.org/](https://cran.r-project.org/).
* **[optional]** `orca` required to download _plotly_ figures. Please see [https://github.com/plotly/orca](https://github.com/plotly/orca) for the installation instruction.

### Stable version

Please start up a `R` console and install the stable version as:

```r
install.packages('IOHanalyzer')
```

which is maintained on [CRAN](https://CRAN.R-project.org/package=IOHanalyzer) (Comprehensive R Archive Network).

### Lastest version

The lastest development is always hosted on Github. In case you'd like to try out this version, the `R` package <tt>devtool</tt> is needed:

```r
install.packages('devtools')
devtools::install_github('IOHprofiler/IOHanalyzer')
```

## Start up the Graphical User Interface

The IOHanalyzer package can be installed and loaded using the following commands:

```r
library('IOHanalyzer')
runServer()
```

Have fun! For the complete reference on usage, please continue reading this documentation.
