---
layout: page
title: IOHanalyzer
nav_order: 4
has_children: true
permalink: /IOHanalyzer(Post-Processing)/
---

IOHanalyzer
============================================

In this page, an example on **IOHanalyzer** is given based on the benchmark data generated in the previous section. In case it occurs to be  time-consuming for the reader to execute the benchmarking example, the exactly same data set can also be downloaded from [the site](https://github.com/IOHprofiler/IOHdata). We provide and maintain two versions of the **IOHanalyzer** package:
+ a CRAN (Comprehensive R Archive Network) version [R package](https://cran.r-project.org/web/packages/IOHanalyzer)
+ a latest stable version that is hosted on [Github page](https://github.com/IOHprofiler/IOHanalyzer).

The CRAN version can be obtained by:
```R
>install.packages('IOHanalyzer')
```

To install the latest stable version, \pkg{devtools} is required again:
```R
>devtools::install_github('IOHprofiler/IOHanalyzer')
```
And please import the package after installation:
```R
>library('IOHanalyzer')
```
Note that those two versions only differ in some *aesthetic aspects* of the plotting method, which undergos a continuous and constant improvement.

The example here is mainly two-fold: the usage of the *programming interface* and the *Graphical User Interface*.

To use IOHexperimenter, please use the following tutorials:
* [Programming Interface](ProgrammingInterface/)
* [Graphic User Interface](GraphicUserInterface/)
* [Supported Data Format](dataformat/)