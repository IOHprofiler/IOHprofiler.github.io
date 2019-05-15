---
layout: page
title: Getting Started
parent: IOHexperimenter
nav_order: 1
permalink: /IOHexperimenter/GettingStarted/
--- 

Getting Started
==============================================

* [Getting started by C](#startC)
* [Getting started by Python](#startPython)
* [Getting started by R](#startR)

Getting started by C<a name="startC"></a>
---------------

See [here](../../../README.md#Getting-Started) for the first steps and execute the 
```python
python do.py build-c
``` 

To run your experiment using the tool, following files are necessary:
- `IOHprofiler.h` Headfile for source code.
- `IOHprofiler.c` source code for the project.
- `readcfg.c`, `configuration.ini` Configs of problems.
- `user_experiment.c` An interface to assign problems for the experiment.
- `user_algorithm.c` the file including implementation of the algorithm to be tested.
- `Makefile.in`

With these files, you can invoke **make** to compile, then run your experiment. To specify the experiment, you need to take care of [_Configuration_](#Configuration) and [_Implementation of Algorithms_](#Algorithms). 

### Configuration <a name="Configuration"></a>
`Configuration.ini` consists of three parts: **[suite]**, **[observer]** and **[triggers]**.

* **[suite]** is the session that collects problems to be tested in the experiment. 
  * `suite_name`: Currently, ONLY `PBO` suite is avaiable,  please do not modify the value of `suite_name`, unless a new suite is created. 
  * `function_id`: presents id of problems of the suite. The format of `function_id` can be `1,2,3,4` using comma `,` to separate problems' id, or be `1-4` using an en-dash `-` to present the range of problems' id. 
  * `instances_id`: presents id of instances. Instanes 1 means there is no transformer operations on the problem. For instances 2-50, XOR and SHIFT operations are applied on the problem. For instances 5-100, SIGMA and SHIFT operations are applied on the problem. Larger instances ID will be considered as 1.
  * `dimensions`: presents dimensions of problems. The format of `dimensions` is as `500,1000,1500`.

* **[observer]** is about the setting of output files. 
  * `observer_name`: Currently, ONLY `PBO` observer is avaiable, please do not modify the value of `observer_name`, unless a new observer is created.
  * `result_folder`: Directory where stores output files.
  * `algorithm_name`: used for .info files.
  * `algorithm_info`: user for .info files.
  * `parameters_name`: names for recording parameters in algorithms.

* **[triggers]** are parameters for different output files, see [documentation](https://arxiv.org/pdf/1810.05281.pdf) to know technique of recording evluations.
  * `number_target_triggers`, `base_evaluation_triggers`: are for .tdat files.
  * `complete_triggers`: is for .cdat files.
  * `number_interval_triggers`: is for .idat files.

### Implementation of Algorithm <a name="Algorithms"></a>
`user_algorithm.c` includes the implementation of algorithms to be tested. Take the exiting `user_algorithm.c` as an example, a random local search algorithm is implemented in the function **User_Algorithm()**. 

```c
static const size_t BUDGET_MULTIPLIER = 50;
```
configs the maximal budget for evaluations as dimension * 50.

```c
static const size_t INDEPENDENT_RESTARTS = 1;
```
configs that the algorithm will be tested once for each problem.

```c
evalute(parent,y)
```
returns the fitness of **parent** to **y**. The size of **parent**
should be equal to the dimension of the problem.

To test your algorithm, please replace the content of **User_Algorithm()**.

Getting started by Python<a name="startPython"></a>
---------------

See [here](../../../README.md#Getting-Started) for the first steps and execute the 
```python
python do.py build-python
``` 

To run your experiment using the tool, following files are necessary:
- `readcfg.cfg`, `configuration.ini` Configs of problems.
- `user_experiment.py` An interface to assign problems for the experiment.
- `user_algorithm.py` the file including implementation of the algorithm to be tested.

With these files, you can invoke **python experiment.py** to run your experiment. To specify the experiment, you need to take care of [_Configuration_](#Configuration) and [_Implementation of Algorithms_](#Algorithms). 

### Configuration <a name="Configuration"></a>
`Configuration.ini` consists of three parts: **[suite]**, **[observer]** and **[triggers]**.

* **[suite]** is the session that collects problems to be tested in the experiment. 
  * `suite_name`: Currently, ONLY `PBO` suite is avaiable,  please do not modify the value of `suite_name`, unless a new suite is created. 
  * `function_id`: presents id of problems of the suite. The format of `function_id` can be `1,2,3,4` using comma `,` to separate problems' id, or be `1-4` using an en-dash `-` to present the range of problems' id. 
  * `instances_id`: presents id of instances. Instanes 1 means there is no transformer operations on the problem. For instances 2-50, XOR and SHIFT operations are applied on the problem. For instances 5-100, SIGMA and SHIFT operations are applied on the problem. Larger instances ID will be considered as 1.
  * `dimensions`: presents dimensions of problems. The format of `dimensions` is as `500,1000,1500`.

* **[observer]** is about the setting of output files. 
  * `observer_name`: Currently, ONLY `PBO` observer is avaiable, please do not modify the value of `observer_name`, unless a new observer is created.
  * `result_folder`: Directory where stores output files.
  * `algorithm_name`: used for .info files.
  * `algorithm_info`: user for .info files.
  * `parameters_name`: names for recording parameters in algorithms.

* **[triggers]** are parameters for different output files, see [documentation](https://arxiv.org/pdf/1810.05281.pdf) to know technique of recording evluations.
  * `number_target_triggers`, `base_evaluation_triggers`: are for .tdat files.
  * `complete_triggers`: is for .cdat files.
  * `number_interval_triggers`: is for .idat files.

### Implementation of Algorithm <a name="Algorithms"></a>
`user_algorithm.py` includes the implementation of algorithms to be tested. Take the exiting `user_algorithm.py` as an example, a random local search algorithm is implemented in the function **User_Algorithm()**. 

```python
budget = 50
```
configs the maximal budget for evaluations as dimension * 50.

```python
independent_restart = 1
```
configs that the algorithm will be tested once for each problem.

```python
best_value = fun(parent)
```
returns the fitness of **parent** to **best_value**. The size of **parent**
should be equal to the dimension of the problem.

```python
fun.set_parameters(para)
```
sets the parameters (**para**) to be logged in output files. If you don't want to record paramters, just erase the statement. If you set parameters, it will be better to configure names of paramters in `configuration.ini` with the same order.

To test your algorithm, please replace the content of **user_algorithm()**.

Getting Started by R<a name="startR"></a>
--------
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


### Getting Started <a name="Getting-Started"></a>

0. Install the package using one of the methods mentioned [_package_](#package) above.

1. Create you own algorithm, or use the example algorithm provided in the documentation accessed by:
```r
?benchmark_algorithm
```

Note that your algorithm will need to accept exactly one parameter: An IOHproblem object, which contains the following information about the current problem:

* dimension
* function_id
* instance
* fopt (if known)
* xopt (if known)

And the following functions:

* obj_func()
* target_hit()
* set_parameters()

2. Run the benchmarks using the function 'benchmark_algorithm'
  
3. Using the analyzer by visiting [IOHAnalyzer page](https://github.com/IOHprofiler/IOHAnalyzer)