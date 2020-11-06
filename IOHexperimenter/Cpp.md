---
layout: page
title: IOHexperimenter - C++ Interface
parent: IOHexperimenter
permalink: /IOHexp/Cpp/
--- 

## Important note
The IOHexperimenter has recently been restructured. While this significantly improves the usability of the tool, it does mean that existing code will need to be updated in order to work with the newest version. This update includes the removing of many of the prefixes in function names in favour of better namespacing. 

Our apologies for the inconveniences these changes this update may cause, but we hope you see the benefits of this improved structuring when writing your code with IOHexperimenter in the future. 

## Prerequisites

Before installing <b>IOHexperimenter</b>, it is necessary to install the following dependencies:

* A `C++` compiler. The minimum compiler version is g++ 7 or equivalent, but we recommend g++ 9 or equivalent.
* [Cmake](https://cmake.org), version 3.10 or higher

## Installation the package

If you are using the tool for the first time, please download or clone this branch, go to the directory where the project root is located and run the following: 
```
cmake .
make install
```

Note: If you want to set up the install directory, replace the first command with `cmake -DCMAKE_INSTALL_PREFIX=your/path .` where 'your/path' is the required installation directory

After installation, three exectuable files will be generated in the `example` folder. These can be used for testing the IOHexperimenter, and their source provides an easy starting point for running the IOHexperimenter in the three most common ways:
* Running an algorithm on a [single function](#using-individual-problems)
* Running an algorithm on a [suite of functions](#using-suites)
* Using the [IOHexperimenter class to benchmark based on a configuration file](#using-conf)

## Use cases

After installation, you can compile your project as follow (with linking IOH library):
```
g++ $CMPL_FLAGS -o run_experiment run_experiment.cpp -lIOH
```


## <a name="using-individual-problems"></a>Using individual test problems

__NOTE: this section is in the process of being updated after the restructure__

To use __IOHexperimenter__ to run benchmarking on a specific problem, the template-file `run_problem.cpp` is provided. Since all problems within the __IOHexperimenter__ are defined as specific derived `class` inheriting problem `ioh::problem` class, it is quite straightforward to use them.


An example testing evolutionary algorithm with mutation operator on __OneMax__ is implemented in `run_problem.cpp`. To use a different function, modify the include-statement to include the problem to use, and use the corresponding class-name instead of __OneMax__.

For this example, a `OneMax` class is declared and initialized with dimension 1000 on the default instance of the probelem.

```cpp
OneMax om;
int dimension = 1000;
om.set_number_of_variables(dimension);
```

During the optimization process, the algorithm can acquire the fitness value through <i>evaluate()</i> function. In the example below, <i>om.evaluate(x)</i> returns the fitness of `x`. Another option is the statement <i>om.evaluate(x,y)</i>, which stores the fitness of `x` in `y`. `logger` is an __csv_logger__ class, which stores function evaluations in a format compatible with __IOHanalyzer__. <i>logger.do_log(om.loggerInfo())</i> deliveries the lastest information of tested `om` to the `logger`.  In addition, <i>om.hit_optimal()</i> is an indicator you can use to check if the optimum has been found.
```cpp
while (!om.hit_optimal()) {
  x = x_star;
  if (mutation(x, mutation_rate)) {
    y = om.evaluate(x);
    logger.do_log(om.loggerInfo());
  }
  if (y[0] > best_value) {
    best_value = y;
    x_star = x;
  }
}
```

If, for your experiment, you want to generate data to be used in the __IOHanalyzer__, a `csv_logger` should be added to the problem you are testing on. The arguments of `csv_logger` are directory of result folder, name of result folder, name of the algorithm and infomation of the algorithm. With different setting of triggers (observer), mutilple data files are to be generated for each experiment. More details on the available triggers are available [here A.3](https://arxiv.org/pdf/1810.05281.pdf). Before optimizing a problem, `logger` must set to track the problem using the statement <i>logger.track_problem()</i>.

```cpp
std::vector<int> time_points{1,2,5};
std::shared_ptr<csv_logger> logger(new csv_logger("./","run_problem","EA","EA"));
logger->set_complete_flag(true);
logger->set_interval(0);
logger->set_time_points(time_points,10);
logger->activate_logger();
logger.track_problem(om);
```

## <a name="using-suites"></a>Using pre-installed Benchmark Suites

Suites are collections of test problems. The idea behind a suite is that packing problems with similar properties together makes it easier to test an algorithm on a set of problems. Currently, two pre-defined suites are available: [__PBO__](/Suites/PBO), consisting of 23 __pseudo Boolean problems__, and [__BBOB__](https://coco.gforge.inria.fr/downloads/download16.00/bbobdocfunctions.pdf), consisting of 24 __real-valued problems__. To find out how to create your own suites, please visit [this page](/IOHexp/extension/#adding-new-suites).

An example of testing an evolutionary algorithm with mutation operator on the __PBO__ suite is implemented in `run_suite.cpp`. __PBO__ suite includes pointers to 23 problems. To instantiate problems you want to test, the vectors of problem id, instances and dimensions need to be given as follows:
```cpp
std::vector<int> problem_id = {1,2};
std::vector<int> instance_id ={1,2};
std::vector<int> dimension = {100,200,300};
PBO_suite pbo(problem_id,instance_id,dimension);
```

With the suite, you can test problems of the suite one by one, until all problems have been tested. In this example, the order of problem is as follow, and an `evolutionary_algorithm` is applied:

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

If, for your experiment, you want to generate data to be used in the __IOHanalyzer__, a `csv_logger` should be added to the suite. The arguments of `csv_logger` are the directory of result folder, name of result folder, name of the algorithm and infomation of the algorithm. In addition, you can set up mutilple triggers of recording evaluations. For the details of triggers, please visit [here A.3](https://arxiv.org/pdf/1810.05281.pdf).
```cpp
std::vector<int> time_points{1,2,5};
std::shared_ptr<csv_logger> logger(new csv_logger("./","run_suite","EA","EA"));
logger->set_complete_flag(true);
logger->set_interval(2);
logger->set_time_points(time_points,3);
logger->activate_logger();
logger->target_suite(pbo.suite_get_suite_name());
```

## <a name="using-conf"></a>Conducting experiments with a configuration file

By using the provided `experiment` class, you can use a configuration file to configure both the suite and the logger for csv files.

To use the provided experiment structure, you need to provide both the path to the configuration file and the pointer to your optimization algorithm to the <i>experimenter._run()</i> function, which will execute all tasks of the experiment.

In addition, you can set the number of repetitions for all problems by using <i>experimenter._set_independent_runs(2)</i>.
```cpp
std::string configName = "./configuration.ini";
experimenter<int> experimenter(configName,evolutionary_algorithm);
experimenter._set_independent_runs(2);
experimenter._run();
```

For the content of configuration file, it consists of three sections:

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

__observer__ configures parameters of `server`, which is used in `csv_logger`,
* __complete_triggers__ is the switch of `.cdat` files, which works with __complete tracking__ strategy. Set it as `TRUE` or `true` if you want to output `.cdat` files.
* __update_triggers__ is the switch of `.dat` files, which works with __target-based strategy__ strategy. Set it as `TRUE` or `true` if you want to output <i>.dat</i>` files.
* __number_interval_triggers__ configures the `.idat` files, which works with __interval tracking__  number_target_triggers sets the value of the frequecny. If you do not want to generate `.idat` files, set `number_target_triggers` as 0.
* __number_target_triggers__ configures the `.tdat` files, which works with __time-based tracking__ strategy.
* __base_evaluation_triggers__ configures the `.tdat` files, which works with __time-based tracking__ strategy. To switch off `.tdat` files, set both __number_target_triggers__ and __base_evaluation_triggers__ as 0.

## <a name="memberfunctions"></a>Useful functions
`problem` and `suite` provide public member functions so that the optimizer can acquire useful information during optimization process.

A list of useful member functions of `problem` is below:
* <i>evaluate(x)</i>, returns a fitness values. The argument __x__ is a vector of variables.
* <i>evaluate(x,y)</i>, updates __y__ with a fitness values, and __x__ is a vector of variables.
* <i>reset_problem()</i>, reset the history information of problem evaluations. You should call this function at first when you plan to do another test on the same problem class.
* <i>hit_optimal()</i>, returns true if the optimum of the problem has been found.
* <i>get_number_of_variables(number_of_variables)</i>, returns dimension of the problem.
* <i>get_evaluations()</i>, returns the number of function evaluations that has been used.
* <i>loggerInfo</i>, returns a vector of information of function evaluations, which consists of evaluations, current found raw objective, best so far found raw objective, current found transformed objective, and best of far best found transformed objective.
* <i>loggerCOCOInfo</i>, returns a vector of information of function evaluations, which consists of evaluations, precision of current found objective, best so far found precision, current found objective, and best so far found objective.
* <i>get_problem_id()</i>
* <i>get_instance_id()</i>
* <i>get_problem_name()</i>
* <i>get_problem_type()</i>
* <i>get_lowerbound()</i>
* <i>get_upperbound()</i>
* <i>get_number_of_objectives()</i>
* <i>get_optimization_type()</i>

A list of useful member functions of `suite` is below:
* <i>get_next_problem</i>, return a shared point of problems of the suite in order.
* <i>get_current_problem()</i>, returns current problem and reset it.
* <i>get_problem(problem_name,instance,dimension)</i>, returns the specific problem.
* <i>suite_get_number_of_problems</i>
* <i>suite_get_number_of_instances</i>
* <i>suite_get_number_of_dimensions</i>
* <i>suite_get_problem_id</i>
* <i>suite_get_instance_id()</i>
* <i>suite_get_dimension()</i>
* <i>suite_get_suite_name()</i>

## Full function documentation

**If you are interested in the coding details or would like to contribute to the `C++` code, please check out the [full documentation](https://iohprofiler.github.io/IOHexperimenter) of the code base.**