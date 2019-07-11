---
layout: page
title: CSVLogger
parent: Loggers
nav_order: 2
permalink: /IOHexperimenter/Loggers/CSVLogger/
---
## IOHprofiler_csv_logger

`IOHprofiler_csv_logger` provides methods storing output into csv files. For each problems, __.info__ is created to store best found fitness and the first evaluation time that the best fitness has been found for each run. In addition, some data files are generated in sub-folder of each problem to store details of optimization process. The type of generated files is decided by the strategies of `IOHprofiler_observer` being used. The relationship between these strategies and files are as follow.
* .dat files store evaluation information with __target-based tracking strategy__
* .cdat files store evaluation information with __complete tracking strategy__
* .idat files store evaluation information with __interval tracking strategy__
* .tdat files store evaluation information with __time-based tracking_ strategy__

For the complete list of setting parameters of `IOHprofiler_csv_logger`, please visit the [github page](https://github.com/IOHprofiler/IOHexperimenter/blob/developing/src/Template/Loggers/IOHprofiler_csv_logger.cpp).