---
layout: page
title: Loggers
parent: IOHexperimenter
nav_order: 5
has_children: true
permalink: /IOHexperimenter/Loggers/
--- 

Loggers
=======================

__IOHexperimenter__ allows multiple ways to track optimization process, `IOHprofiler_observer` defines the triggers to track, and `class` of __loggers__ implements details of tracking process. For now, we supply `IOHprofiler_csv_logger` to store function evaluations in csv files.