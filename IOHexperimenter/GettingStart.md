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

To use all source files of **IOHexperimenter** directly by C++, an [example](https://github.com/IOHprofiler/IOHexperimenter/blob/NewStructure/src/build/C/IOHprofiler_experiment.cpp) file is supplied. 

### Experiments on specific problem.
A number of problems have been provided by **IOHprofiler**, and each of them has been implemented as seperate `class` (visit [here](https://github.com/IOHprofiler/IOHexperimenter/tree/NewStructure/src/Problems)). Users can operate on specific problem by declaring the corresponding `class`. For instance (`_run_problem` funtion in the [file](https://github.com/IOHprofiler/IOHexperimenter/blob/NewStructure/src/build/C/IOHprofiler_experiment.cpp), we are working on *OneMax*, a `OneMax` class variable 'om' is declared and the `dimension` set as 1000. 



To record the optimization evaluations of the algorithm, a `logger` is added to output csv files logging evaluation information during optimization process. The `logger` is an optional setting for users, users can handle evaluations by themselves and omit `logger` settings.


Following codes present an evolutionary algorithm with static mutation rate. With the `om` variable, `om.evaluate(x)` returns fitness of the solution `x`, which also executes the logging operation if `logger` is added. In addtion, a member function `IOHprofiler_hit_optimal()` is used to detect if the optimal of `om` is found.
```c
void _run_problem() {

  OneMax om;
  int dimension = 1000;
  om.Initilize_problem(dimension);
  
  std::vector<int> time_points{1,2,5};
  IOHprofiler_csv_logger logger("./","run_problem","EA","EA",true,true,2,time_points,3);
  om.addCSVLogger(logger);

  std::vector<int> x;
  std::vector<int> x_star;
  std::vector<double> y;
  double best_value;
  double mutation_rate = 1.0/dimension;

  x = Initialization(dimension);
  copyVector(x,x_star);
  y = om.evaluate(x);
  best_value = y[0];

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
}
```

### Experiments on suites (collections of problems).
If users are planning to test an algorithm on several problems, `suite` implements an interface to multiple problems. In addition, with a `suite` and `logger`, users can handle the output of experiment on different problems with the same configuration. For instance (`_run_suite` funtion in the [file]https://github.com/IOHprofiler/IOHexperimenter/blob/NewStructure/src/build/C/IOHprofiler_experiment.cpp), We plan to test the first two problems in `PBO_suite` in dimension 100,200,300 without any [transformation](http://localhost:4000/Benchmark/Transformation/), a suite `pbo` is declared and assigned.



To record the optimization evaluations of the algorithm, a `logger` is added to output csv files logging evaluation information during optimization process. The `logger` is an optional setting for users, users can handle evaluations by themselves and omit `logger` settings.


After wards, we can get and test `problem` from the suite, `evaluation_algorithm` is a function used to solve problems, you can find it [here](#EA ).
```c
void _run_suite() {
  
  std::vector<int> problem_id = {1,2};
  std::vector<int> instance_id ={1};
  std::vector<int> dimension = {100,200,300};
  PBO_suite pbo(problem_id,instance_id,dimension);

  std::vector<int> time_points{1,2,5};
  IOHprofiler_csv_logger logger1("./","run_suite","EA","EA",true,true,2,time_points,3);
  pbo.addCSVLogger(logger1);
  std::shared_ptr<IOHprofiler_problem<int>> problem;

  int index = 0;
  while(index < pbo.get_size()){
    problem = pbo.get_next_problem(index);
    evolutionary_algorithm(problem);
    index++;
  }
}
```

### Experiments with configuration
`IOHprofiler_experimenter` provides a pre-defined structure for using **IOHexperimenter**. By using the class, users can concern on algorithm and maintain settings of experiments with configuration files. For example, an evolutionary algorithm has been implemented as below (argument of the `function` must follow the same). A `IOHprofiler_problem` is the argument of the function, and the algorithm achieves fitness by the statement `y = problem->evaluate(x);`.
 <a name="EA"></a>
```c
std::vector<int> Initialization(int dimension) {
  std::vector<int> x;
  x.reserve(dimension);
  for(int i = 0; i != dimension; ++i){
      x.push_back(rand()% 2);
  }
  return x;
};

int mutation(std::vector<int> &x, double mutation_rate) {
  int result = 0;
  int n = x.size();
  for(int i = 0; i != n; ++i) {
    if(rand() / double(RAND_MAX) < mutation_rate) {
      x[i] = (x[i] + 1) % 2;
      result = 1;
    }
  }
  return result;
}

// This is an (1+1)_EA with static mutation rate = 1/n.
void evolutionary_algorithm(std::shared_ptr<IOHprofiler_problem<int>> problem) {
  std::vector<int> x;
  std::vector<int> x_star;
  std::vector<double> y;
  double best_value;
  double mutation_rate = 1.0/problem->IOHprofiler_get_number_of_variables();

  x = Initialization(problem->IOHprofiler_get_number_of_variables());
  copyVector(x,x_star);
  y = problem->evaluate(x);
  best_value = y[0];

  while(!problem->IOHprofiler_hit_optimal()) {
    copyVector(x_star,x);
    if(mutation(x,mutation_rate)) {
      y = problem->evaluate(x);
    }
    if(y[0] > best_value) {
      best_value = y[0];
      copyVector(x,x_star);
    }
  }
}
```
With a configuration of `suite` and `logger` by a file `configuration.ini` (see [details](#Configuration) ), the experiment can be run as:
```c
void _run_experiment() {
  std::string configName = "./configuration.ini";
  IOHprofiler_experimenter<int> experimenter(configName,evolutionary_algorithm);
  experimenter._run();
}
```

### Configuration file <a name="Configuration"></a>
`Configuration.ini` consists of three parts: **[suite]**, **[observer]** and **[triggers]**.

* **[suite]** is the session that collects problems to be tested in the experiment. 
  * `suite_name`: Currently, ONLY `PBO` suite is avaiable,  please do not modify the value of `suite_name`, unless a new suite is created. 
  * `problem_id`: presents id of problems of the suite. The format of `function_id` can be `1,2,3,4` using comma `,` to separate problems' id, or be `1-4` using an en-dash `-` to present the range of problems' id. 
  * `instance_id`: presents id of instances. Instanes 1 means there is no transformer operations on the problem. For instances 2-50, XOR and SHIFT operations are applied on the problem. For instances 5-100, SIGMA and SHIFT operations are applied on the problem. Larger instances ID will be considered as 1.
  * `dimension`: presents dimensions of problems. The format of `dimensions` is as `500,1000,1500`.

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
  * `complete_triggers` is the switch of `.cdat` files, which will store evaluations of all iterations. Set `complete_triggers` as `TRUE` or `true` if you want to output `.cdat` files
  * `update_triggers` is the switch of `.dat` files, which will store evaluations when a better solution is found. Set `complete_triggers` as `TRUE` or `true` if you want to output `.dat` files
  * `number_interval_triggers` works on the `*.idat` files. `*.idat` files log evaluations in a fixed frequecny. number_target_triggers sets the value of the frequecny. If you do not want to generate `.idat` files, set `number_target_triggers` as 0. 
  * both `number_target_triggers` and `base_evaluation_triggers` effect `.tdat`. `number_target_triggers` is a value defines the number of evaluations to be logged between 10^i and 10^(i+1). If you do not want to generate `.tdat` files, set `number_target_triggers` as 0. `base_evaluation_triggers` defines the base evaluations used to produce an additional evaluation-based logging. For example, if `base_evaluation_triggers` = `1,2,5`, the logger will be triggered by evaluations $1n10^i, 2n*10^i, 5n10^i$ . If you do not want to generate `.tdat` files, set `base_evaluation_triggers` as ``.


Getting started by Python<a name="startPython"></a>
---------------


Getting started by R<a name="startR"></a>
---------------