---
layout: page
title: IOHexperimenter - C++ Interface
parent: IOHexperimenter
permalink: /IOHexp/Cpp/
--- 

## Installation

The following toolkits are needed for compiling IOHexperimenter:

* A `C++` compiler. The minimum compiler version is g++ 7 or equivalent, but we recommend g++ 9 or equivalent.
* [CMake](https://cmake.org), version 3.12 or higher

Please use the following commands to download, compile, and install this package:

```sh
> git clone --recursive https://github.com/IOHprofiler/IOHexperimenter.git
> cd IOHexperimenter
> mkdir build
> cd build
> cmake .. && make install
```

which installs all header files to `/usr/local/include/ioh` by default.
If you want to change this directory, please use the following flag `cmake -DCMAKE_INSTALL_PREFIX=your/path ..`

If you want to change build options, check the output of `cmake -L` or use `cmake-gui` or `ccmake`.


<a name="using-individual-problems"></a>
## Testing on Problems 

Users can test algorithms on built-in `problem` classes. Twenty-five pseudo-Boolean optimization problems (), twenty-four continuous optimization problems ([bbob](https://coco.gforge.inria.fr/downloads/download16.00/bbobdocfunctions.pdf)), and two classes of [W-Model](https://dl.acm.org/doi/pdf/10.1145/3205651.3208240) problems are available in __IOHexperimenter__. All these problems are built inheriting a [template class](https://github.com/IOHprofiler/IOHexperimenter/blob/master/include/ioh/problem/problem.hpp). You can also test new [customized problems](/IOHexp/extension/#adding-new-problems).

An example of testing a random search on OneMax, that belongs to [PBO](https://www.sciencedirect.com/science/article/pii/S1568494619308099) suite, is provided in [problem_example.h](https://github.com/IOHprofiler/IOHexperimenter/blob/master/example/problem_example.h).

We use the following statement to declare a `problem` class of the instance 1 of onemax in dimension 10. Instances of a problem are initiated with different transformations. Details of transformations can be found in [Section 3.2](https://www.sciencedirect.com/science/article/pii/S1568494619308099) of our publication.
```cpp
const auto om = std::make_shared<ioh::problem::pbo::OneMax>(1, 10);
```
Similarly, the following statement declares a class of `Sphere`, which belongs to the `bbob` space. 
```cpp
const auto sp = std::make_shared<ioh::problem::bbob::Sphere>(1, 5);
```

To evaluate solutions found by the algorithms, we can obtain the fitness values of `x` as below.
```cpp
auto y = (*om)(x).at(0)
```

In addition, the `meta_data()` class provides access to useful information about the problem including problem id, instance id, problem name, optimization type (e.g., minimization or maximization), and dimension. For example, we can use the following statement to output information.
```cpp
std::cout << om->meta_data() << std::endl;
```

Also, a `Constraint` struct provides bounds of variables. Precisely, vectors of `ub` and `lb` denote each variable's upper bounds and lower bounds, respectively. A `check(x)` function returns True if variables of `x` are in the decision domain of variables; otherwise false. You can also access information of bounds using
```cpp
std::cout << om->constraint() << std::endl;
```



<a name = "using-suites"></a>
## Testing Suites

Suites are collections of problems. Two suites, [PBO](https://www.sciencedirect.com/science/article/pii/S1568494619308099)) for the pseudo-Boolean optimization problems and [BBOB](https://coco.gforge.inria.fr/downloads/download16.00/bbobdocfunctions.pdf) for continuous optimization problems are available in __IOHexperimenter__. An example of using suites is provided in [suite_example.h](https://github.com/IOHprofiler/IOHexperimenter/blob/master/example/suite_example.h).

We declare a suite consisting of 8 problems of the BBOB suite, using the following statement:
1. problem 1, instance 1, dimension 5
2. problem 1, instance 3, dimension 5
3. problem 1, instance 1, dimension 10
4. problem 1, instance 3, dimension 10
5. problem 2, instance 1, dimension 5
6. problem 2, instance 3, dimension 5
7. problem 2, instance 1, dimension 10
8. problem 2, instance 3, dimension 10

```cpp
const auto &suite_factory = ioh::suite::SuiteRegistry<ioh::problem::Real>::instance();
const auto bbob = suite_factory.create("BBOB", {1, 2}, {1, 3}, {5, 10});
```

In the example, a random search is applied across all the problems within a loop :
```cpp
for (const auto &problem : *bbob) :
{
  ...
}
```
Usage of the `problem` class is introduced [above](#using-individual-problems).


<a name = "using-logger"></a>
## Using Logger

The default logger records evaluation information during the optimization process into tab-seperated files. The details of file format can be found [here](/IOHanalyzer/data/). An example of using the default logger is provided in [logger_example.h](https://github.com/IOHprofiler/IOHexperimenter/blob/master/example/logger_example.h)


We declare a default logger using the following statement.
```cpp
auto logger = ioh::logger::Default(
        fs::current_path(), /* output_directory : fs::current_path() -> working directory */
        "folder_name", /* folder_name */
        "random_search", /* algorithm_name : "random search'*/
        "a random search" /* algorithm_info : "a random search for testing the bbob suite" */
        );
```

We attach the `logger` to the `suite`, to perform logging process during optimization.
```cpp
suite->attach_logger(logger);
```

Alternatively, we can attach the logger to each new problem in a suite manually using
```cpp
problem->attach_logger(logger);
```
<a name = "tracking-parameters"></a>
### Tracking Parameters
The default loggers provides interfaces for tracking parameters of algorithms during optimization process. An example is provided in the [logger_example.h](https://github.com/IOHprofiler/IOHexperimenter/blob/da22d89fe1673ea67962829d12873e01387f6895/example/logger_example.h#L80)


`experiment_attribute` stores the fixed parameters for each experiment, `run_attributes` maintains the parameters of each run of an algorithm, and `logged_attributes` records the online parameters for each evaluation.
```cpp
// Add parameters fixed throughout the experiment.
  logger.add_experiment_attribute("meta_data_x", 69);
  logger.add_experiment_attribute("meta_data_y", 69);

  // Initialize parameters unique for each run. 
  logger.create_run_attributes({"iteration"});

  // Initialize parameters unique for each evaluation.
  logger.create_logged_attributes({"x1"});
```

The corresponding values of `run_attributes` and `logged_attributes` shall be set as below.
```cpp
// Update the variable for the run specific parameter
logger.set_run_attributes({"run_id"}, {static_cast<double>(run_id)});

// Update the variable for the evaluation specific parameter
logger.set_logged_attributes({"iteration"}, {static_cast<double>(i+1)});
```

In addition, the default logger supports tracking positions (values of decision variables). An example is given in the `get_logger_with_positions()` function.
```cpp
inline ioh::logger::Default get_logger_with_positions()
{
    /// Instantiate a logger that also stores evaluated search points
    return ioh::logger::Default(
        fs::current_path(), /* output_directory : fs::current_path() -> working directory */
        "logger_w_positions", /* folder_name : "logger_example" */
        "random_search", /* algorithm_name : "random search'*/
        "a random search", /* algorithm_info : "a random search for testing the bbob suite" */
        ioh::common::OptimizationType::Minimization,
        true /* store_positions: true*/
        );
}
```

<a name = "using-exp"></a>
## Using Experiment

The `Experimenter` class automatically tests a `solver` on a pre-defined `suite` and records the optimization process using a `logger`. The arguments of the `solver` must be a pointer of an ioh `problem` class, and the type of the `problem` must be identical to the type of the `suite`.
```cpp
// using ioh::problem::Real for continuous optimization.
void solver(const std::shared_ptr<ioh::problem::Real> p)
{
  ...
}
```

The following example tests the `solver` on a bbob suite. The `solver` performs 10 independent runs on each instance of problems.
```cpp
const auto &suite_factory = ioh::suite::SuiteRegistry<ioh::problem::Real>::instance();
const auto suite = suite_factory.create("BBOB", {1, 2}, {1, 2}, {5, 10});
const auto logger = std::make_shared<ioh::logger::Default>(std::string("logger-experimenter"));

ioh::experiment::Experimenter<ioh::problem::Real> f(suite, logger, solver, 10);
f.run();
```
