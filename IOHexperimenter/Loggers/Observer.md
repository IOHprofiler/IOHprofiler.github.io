---
layout: page
title: Observer
parent: Loggers
nav_order: 1
permalink: /IOHexperimenter/Loggers/Observer/
--- 

## IOHprofiler_observer

`IOHprofiler_observer` defines triggers of recording evaluations, and __logger__ classes inherit it to set up the time of recording.

Four strategies of recording evaluations are available,
* __complete tracking__, provides the highest granularity, by storing information for each function evaluation. Use <i>set_complete_flag(true)</i> to enable this strategy,
* __interval tracking__, stores information for each $\tau$-th function evaluation. Use <i>set_interval()</i> to set $\tau > 0$ to enable this stragety.
* __target-based tracking__, stores information for each iteration in which the best-so-far fitness improved. Use <i>set_update_flag(true)</i> to enbale this stragety.
* __time-based tracking__, stores information when the user-specified running time budgets are reached. These budget are evenly spaced in log-10 scale, taking the form $v10^i,  i=0,1,2,...$ or $10^{i/t}, i = 0,1,2,...$ . You can use <i>set_time_points(v, t)</i> to set $v$ and $t$.