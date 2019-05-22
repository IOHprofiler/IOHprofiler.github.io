---
layout: page
title: AddingProblems
parent: IOHexperimenter
nav_order: 2
permalink: /IOHexperimenter/AddingProblems/
--- 

Adding Problems
==================================
`IOHprofiler_problem` class ([src](https://github.com/IOHprofiler/IOHexperimenter/blob/NewStructure/src/IOHprofiler_problem.hpp)) presents a common template class for problems using in **IOHexperimenter**, users can add new problems by creating new class inheriting **IOHprofiler_problem**. `IOHprofiler_problem` supports problems with variables being arbitrary but unique type and objectives as real numbers. Taking [OneMax](https://github.com/IOHprofiler/IOHexperimenter/blob/NewStructure/src/Problems/f_one_max.hpp) as an example, `InputType`, which is the type of variables, set as `int`. 


With must header file, `problem_id`, `instance_id`, `problem_name`, `problem_type` and `number_of_objectives` must be set by the constructor function. But to specify the problem to be tested, interfaces for setting `number_of_variables`, `lowerbound` and `upperbound` of variables, `best_variables`. `optimal` is an optional setting, since for some problem optimal is unknown.


Definition of the problem is implemented in the function `internal_evaluate`, which will be used in the `evaluate` function of `IOHprofiler_problem`. The argument of the function is input variables, and the function returns a vector of objectives.
```c
#include "../IOHprofiler_problem.hpp"

class OneMax : public IOHprofiler_problem<int> {
  public:
    OneMax() {
      IOHprofiler_set_problem_id(1);
      IOHprofiler_set_instance_id(1);
      IOHprofiler_set_problem_name("OneMax");
      IOHprofiler_set_problem_type("pseudo_Boolean_problem");
      IOHprofiler_set_number_of_objectives(1);
  }
  OneMax(int instance_id, int dimension) {
    IOHprofiler_set_problem_id(1);
    IOHprofiler_set_instance_id(instance_id);
    IOHprofiler_set_problem_name("OneMax");
    IOHprofiler_set_problem_type("pseudo_Boolean_problem");
    IOHprofiler_set_number_of_objectives(1);

    Initilize_problem(dimension);
  }

  void Initilize_problem(int dimension) {
    IOHprofiler_set_number_of_variables(dimension);
    IOHprofiler_set_lowerbound(0);
    IOHprofiler_set_upperbound(1);
    IOHprofiler_set_best_variables(1);
    IOHprofiler_set_optimal((double)dimension);
  };

  std::vector<double> internal_evaluate(std::vector<int> x) {
    std::vector<double> y;
    int n = x.size();
    int result = 0;
    for(int i = 0; i != n; ++i) {
      result += x[i];
    }
    y.push_back((double)result);
    return y;
  };
}
``` 
