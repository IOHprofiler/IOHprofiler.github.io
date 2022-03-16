---
layout: page
title: Extending IOHexperimenter
parent: IOHexperimenter
permalink: /IOHexp/extension/
--- 

<a name="adding-new-problems"></a>
## Adding new problems

We offer a very simple and convenient interface for integrating new benchmark problems/functions. First, you could define a new `test_problem` as you like. Note that the `<vector>` header is already imported in "ioh.hpp".

```C++
#include "ioh.hpp"

std::vector<double> new_problem(const std::vector<double> &)
{
    // the actual function body start here
    // ...
}
```

Then, you only need to "wrap" this new function as follows:

```c++
ioh::problem::wrap_function<double>(&new_problem,
                                    "new_problem" // name for the new function
                                    );
```

In addition, you can define transformation methods for generating various instances of the function.

```c++
std::vector<double> new_transform_variables_function(const std::vector<double> &x, int instance_id) {
  // The instance_id can be used as seed for possible randomizing process.
  // ...
}

double new_transform_objective_functions(double y, int instance_id) {
  // The instance_id can be used as seed for possible randomizing process.
  // ...
}

ioh::problem::wrap_function<double>(&new_problem,  // the new function
                                    "new_problem", // name of the new function
                                    ioh::common::OptimizationType::Minimization, // optimization type
                                    0,  // lowerbound  
                                    1,  // upperbound
                                    &new_transform_variables_function, // the variable transformation method. Optional argument when transformation is applied.
                                    &new_transform_objective_functions // the objective transformation method. Optional argument when transformation is applied.
                                    );
```

After wrapping, we could create this `new_problem` from the problem factory. Note that,
the instance id is ineffective in this approach since we haven't implemented any transformations for the wrapped problem.

```c++
auto &factory = ioh::problem::ProblemRegistry<ioh::problem::Real>::instance();
auto new_problem_f = factory.create(
  "new_problem",  // create by name
  1,               // instance id
  10               // number of search variables
);
```

Alternatively, one might wish to create the new problem by subclassing the abstract problem class
in IOHexperimenter, taking benefits of implementing more details, e.g., aforementioned transformations.
This can be done by inheriting the corresponding problem registration class, which is

* `ioh::problem::IntegerProblem` for pseudo-Boolean problems, and
* `ioh::problem::RealProblem` for continuous problems.

In the below example, we show how to do this for pseudo-Boolean problems.

```C++
class NewBooleanProblem final : public ioh::problem::IntegerProblem<NewBooleanProblem>
{
protected:
    // [mandatory] The evaluate method is mandatory to implement
    std::vector<int> evaluate(const std::vector<int> &x) override
    {
        // the function body goes here
    }

    // [optional] If one wish to implement transformations on objective values
    std::vector<double> transform_objectives(std::vector<double> y) override
    {

    }

    // [optional] If one wish to implement transformations on search variables
    std::vector<double> transform_objectives(std::vector<double> y) override
    {

    }

public:
    /// [mandatory] This constructor always take `instance` as input even
    /// if it is ineffective by default. `instance` would be effective if and only if
    /// at least one of `transform_objectives` and `transform_objectives` is implemented
    NewBooleanProblem(const int instance, const int n_variables) :
        IntegerProblem(
          ioh::problem::MetaData(
            1,                     // problem id, which will be overwritten when registering this class in all pseudo-Boolean problems
            instance,              // the instance id
            "NewBooleanProblem",   // problem name
            n_variables,           // search dimensionality
            1,                     // number of objectives, only support 1 for now
            ioh::common::OptimizationType::Minimization
            )
          )
    {
    }
};
```

Please check [this example](https://github.com/IOHprofiler/IOHexperimenter/blob/8a49d76d591c52b4ae8ed0991d4b6ea8d5c3adaa/example/problem_example.h#L52) for adding continuous problems in this manner.

<a name="adding-new-loggers"></a>
## Adding new loggers

We offer a simple base class for customized loggers. You can define a new logger class inheriting the `logger` class in [loggers.hpp](https://github.com/IOHprofiler/IOHexperimenter/blob/master/include/ioh/logger/loggers.hpp).

First, you need to define a new `attach_problem` function to access the [metadata](https://github.com/IOHprofiler/IOHexperimenter/blob/e28136a6c700d0c8d50855fe5724eec8e734c12e/include/ioh/logger/loggers.hpp#L190) of the problem to be tested. The `attach_problem` function will be revoked in the `attach_logger` function of the `problem` class. You should 
also define a `track_suite` function for tracking information of suites for your experiment.
```C++
void track_problem(const problem::MetaData& problem) 
{

}

void track_suite(const std::string& suite_name) 
{

}

```

Then you should define a `log` function checking if the logger should be triggered, if so, `call(log_info)` will be revoked. `call(log_info)` is the main entry performing logging process, for example, writing csv files. The `log` function will be revoked in the `evaluate` function of the `problem` class. The `logger::Info` class provides variables of the optimization state.

```C++
void log(const logger::Info& log_info)
{

}

void call(const logger::Info& log_info){

}
```

```C++
struct Info
{
    //! Number of evaluations of the objective function so far.
    size_t evaluations;

    //! The current best internal objective function value (since the last reset).
    double raw_y_best; // was y_best
    
    //! The current transformed objective function value.
    double transformed_y;
    
    //! The current best transformed objective function value (since the last reset).
    double transformed_y_best;
    
    //! Currently considered solution with the corresponding transformed objective function value.
    problem::Solution<double> current;
    
    //! Optimum to the current problem instance, with the corresponding transformed objective function value.
    problem::Solution<double> optimum; // was objective
};
```

Please check [this example](https://github.com/IOHprofiler/IOHexperimenter/blob/master/include/ioh/logger/analyzer.hpp) for recording evaluation information during the optimization process into csv files. 