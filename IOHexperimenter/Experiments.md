---
layout: page
title: Experiments
parent: IOHexperimenter
nav_order: 1
permalink: /IOHexperimenter/Experiments/
--- 

Getting Started
==============================================

* [Getting started by C++](#startC++)
* [Getting started by R](#startR)

<a name="startC++"></a>
## Using IOHexperimenter by C++

After compiling the tool by running `make` in the root directory of the project, `/bin` and `/obj` are to be created in the '/build/Cpp' folder. To use __IOHexperimenter__ to test your algorithms, you can work in this `Cpp` folder and replace the `Makefile` by the `Makefile-local`. Afterwards, you can edit your algorithm in the provided `cpp` files and compile them through `make`.

There are three ways to test algorithms:
* Test through problem class.
* Test through collections of problems (suite)
* Test through a designed experiment class and edit by configuration files

<a name="Tclass"></a>
### Test through problem class
An example testing evolutionary algorithm with mutation operator on __OneMax__ is implemented in `IOHprofiler_run_problem.cpp`. 

For being tested, a `OneMax` class is declared and set with 1000 dimension.
```cpp
OneMax om;
int dimension = 1000;
om.Initilize_problem(dimension);
```

During the optimization process, the algorithm can acquire the fitness value through <i>evaluate()</i> function. In the example below, <i>om.evaluate(x)</i> returns the fitness of `x`. You can also use the statement <i>om.evaluate(x,y)</i> to store the fitness of `x` in `y`. In addition, <i>om.IOHprofiler_hit_optimal()</i> is a detector to check if the optimum has been found.
```cpp
while(!om.IOHprofiler_hit_optimal()) {
  copyVector(x_star,x);
  if(mutation(x,mutation_rate)) {
    y = om.evaluate(x);
  }
  if(y[0] > best_value) {
    best_value = y[0];
    copyVector(x,x_star);
  }
}
```

If you want to generate result data for __IOHanalyzer__, a `IOHprofiler_csv_logger` should be added. The arguments of `IOHprofiler_csv_logger` are directory of result folder, name of result folder, name of the algorithm and infomation of the algorithm. In addition, you can set up mutilple triggers of recording evaluations. For the details of triggers, please visit the introduction of [`IOHprofiler_observer`](/IOHexperimenter/Loggers/Observer).
```cpp
std::vector<int> time_points{1,2,5};
std::shared_ptr<IOHprofiler_csv_logger> logger(new IOHprofiler_csv_logger("./","run_problem","EA","EA"));
logger->set_complete_flag(true);
logger->set_interval(0);
logger->set_time_points(time_points,10);
logger->activate_logger();
om.addCSVLogger(std::move(logger));
```

<a name="Tsuite"></a>
### Test through suite
Suites are collections of test problems. The idea is packing problems with similar/same properties toghther and making it easier to test algorithms on a class of problems. Currently a suite called __PBO__ consisting of 23 __pseudo Boolean problems__ are provied by __IOHexperimenter__. To create your own suites, please visit [here](/IOHexperimenter/CreatingSuites/).

An example testing evolutionary algorithm with mutation operator on __PBO__ suite is implemented in `IOHprofiler_run_suite.cpp`. __PBO__ suite includes pointers for 23 problems. To instantiate problems you want to test, the vectors of problem id, instances and dimensions need to be given.
```cpp
std::vector<int> problem_id = {1,2};
std::vector<int> instance_id ={1,2};
std::vector<int> dimension = {100,200,300};
PBO_suite pbo(problem_id,instance_id,dimension);
```

With the suite, you can test problems of the suite one by one, until all problems have been tested. In this example, the order of problem is as follow, and an `evlutionary_algorithm` is applied:

1. problem id 1, instance 1, dimension 100
2. problem id 1, instance 2, dimension 100
3. problem id 1, instance 1, dimension 200
4. problem id 1, instance 2, dimension 200
5. problem id 1, instance 1, dimension 300
6. problem id 1, instance 2, dimension 300
7. problem id 2, instance 1, dimension 100
8. problem id 2, instance 2, dimension 100
9. problem id 2, instance 1, dimension 200
10. problem id 2, instance 2, dimension 200
11. problem id 2, instance 1, dimension 300
12. problem id 2, instance 2, dimension 300

```cpp
while (problem = pbo.get_next_problem()) {
  evolutionary_algorithm(problem);
}
```

If you want to generate result data for __IOHanalyzer__, a `IOHprofiler_csv_logger` should be added. The arguments of `IOHprofiler_csv_logger` are directory of result folder, name of result folder, name of the algorithm and infomation of the algorithm. In addition, you can set up mutilple triggers of recording evaluations. For the details of triggers, please visit the introduction of [IOHprofiler_observer](/IOHexperimenter/Loggers/).
```cpp
std::vector<int> time_points{1,2,5};
std::shared_ptr<IOHprofiler_csv_logger> logger(new IOHprofiler_csv_logger("./","run_suite","EA","EA"));
logger->set_complete_flag(true);
logger->set_interval(2);
logger->set_time_points(time_points,3);
logger->activate_logger();
pbo.addCSVLogger(logger);
```

<a name="Tconfig"></a>
### Test through configuration file

By using the provided `IOHprofiler_experiment` class, you can use a configuration file to configure both the suite and the logger for csv files. With a `function` of optimizer algorithm and assiging the path of configuration file, <i>experimenter._run()</i> will execute all tasks of the experiment.

In addition, you can set the test times on each problem by <i>experimenter._set_independent_runs(2)</i>.

```cpp
std::string configName = "./configuration.ini";
IOHprofiler_experimenter<int> experimenter(configName,evolutionary_algorithm);
experimenter._set_independent_runs(2);
experimenter._run();
```

For the detail of configuration files, it consists of three sections:

__suite__ configures the problems to be tested.
* __suite_name__, is the name of the suite to be used. Please make sure that the suite with the corresponding name is registered.
* __problem_id__, configures problems to be tested. Note that id of problems are configured by the suite, please make sure that id is within the valid range.
* __instance_id__, configures the transformation methods applied on problems. 
  For `PBO`:
  * `1` : no transformer operations on the problem.
  * `2-50` :  XOR and SHIFT operations are applied on the problem.
  * `51-100`: SIGMA and SHIFT operations are applied on the problem.
* __dimension__, configures dimension of problems to be tested. Note that allowed dimension is not larger than 20000.

__logger__ configures the setting of output csv files.
* __result_foler__ is the directory of the folder where sotres output files.
* __algorithm_name__, is the name of the algorithm, which is used when generating '.info' files.
* __algorithm_info__, is additional information of the algorithm, which is used when generating '.info' files.

__observer__ configures parameters of `IOHprofiler_server`, which is used in `IOHprofiler_csv_logger`,
* __complete_triggers__ is the switch of `.cdat` files, which works with __complete tracking__ strategy. Set it as `TRUE` or `true` if you want to output `.cdat` files.
* __update_triggers__ is the switch of `.dat` files, which works with __target-based strategy__ strategy. Set it as `TRUE` or `true` if you want to output <i>.dat</i>` files.
* __number_interval_triggers__ configures the `.idat` files, which works with __interval tracking__  number_target_triggers sets the value of the frequecny. If you do not want to generate `.idat` files, set `number_target_triggers` as 0.
* __number_target_triggers__ configures the `.tdat` files, which is the $t$ of __time-based tracking__ strategy.
* __base_evaluation_triggers__ configures the `.tdat` files, which is the $v$ of __time-based tracking__ strategy. To switch off `.tdat` files, set both __number_target_triggers__ and __base_evaluation_triggers__ as 0.



<a name="startR"></a>
## Using IOHexperimenter by R
For the use of `R`, please visit [R branch](https://github.com/IOHprofiler/IOHexperimenter/tree/R).