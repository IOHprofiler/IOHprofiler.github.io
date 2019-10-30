---
layout: page
title: Adding Functions
parent: IOHexperimenter
nav_order: 4
permalink: /IOHexperimenter/Adding-Functions/
--- 

Creating Problems
==================================
[IOHprofiler_problem]() is the base `class` of problems of __IOHexperimenter__. The property variables of problems include:
* `problem_id`, will be assigned if the problem is added to a suite, otherwise default by 0.
* `instance_id`,  sets transformation methods on problems. The original problem is with instance_id 1, <i>scale</i> and <i>shift</i> are applied on objectives for instance_id in [2,100], <i>XOR</i> is applied on variables for instance_id in [2,50], and <i>sigma</i> function is applied on variables for instance_id in [51,100].
* `problem_name`
* `problem_type`
* `lowerbound`, is a vector of lowerbound for variables.
* `upperbound`, is a vector of upperbound for variables.
* `number_of_variables`, is the dimension of the problem.
* `number_of_objectives`, is only available as 1 now. The functionality of multi-objectives is under development.
* `best_variables`, is a vector of optimal solution, which is used to calculate the optimum. If the best_variables is not given, the optimum will be set as __DBL_MAX__.
* `optimal`, is a vector of optimal objectives, but currently only single objective is supported.

* `evaluate_int_info`, is a vector of __int__ values that are iteratively used in <i>evaluate</i>.
* `evaluate_double_info`, is a vector of __double__ values that are iteratively used in <i>evaluate</i>.

And some functions for personal experiments are supplied:
* <i>evaluate(x)</i>, returns a vector of fitness values. The argument __x__ is a vector of variables.
* <i>evaluate(x,y)</i>, updates __y__ with a vector of fitness values, and __x__ is a vector of variables.
* <i>addCSVLogger(logger)</i>, assigns a __IOHprofiler_csv_logger__ class to the problem.
* <i>clearLogger()</i>, delete logger methods of the problem.
* <i>reset_problem()</i>, reset the history information of problem evaluations. You should call this function at first when you plan to do another test on the same problem class.
* <i>IOHprofiler_hit_optimal()</i>, returns true if the optimum of the problem has been found.
* <i>IOHprofiler_set_number_of_variables(number_of_variables)</i>, sets dimension of the problem.
* <i>IOHprofiler_set_instance_id(instance_id)</i>

__IOHexperimenter__ provides a variety of problems for testing algorithms, but it is also easy to add your own problems. Overall, to create a problem of __IOHexperimenter__, two functions need to be implemented: <i>construct functions</i> and <i>internel_evaluate</i>. Additionally, you can define <i>update_evaluate_double_info</i> and <i>update_evaluate_int_info</i> to make evluate process more efficiently.

Taking the implementation of __OneMax__ as an instance, <i>construct functions</i> are as below. `problem_name` and `number_of_objectives` __must__ be set. In general, two methods of construction of the problems are given. One is constructing without giving `instance_id` and `dimension`, and the other one is with.
```cpp
OneMax() {
  IOHprofiler_set_problem_name("OneMax");
  IOHprofiler_set_problem_type("pseudo_Boolean_problem");
  IOHprofiler_set_number_of_objectives(1);
  IOHprofiler_set_lowerbound(0);
  IOHprofiler_set_upperbound(1);
  IOHprofiler_set_best_variables(1);
}

OneMax(int instance_id, int dimension) {
  IOHprofiler_set_instance_id(instance_id);
  IOHprofiler_set_problem_name("OneMax");
  IOHprofiler_set_problem_type("pseudo_Boolean_problem");
  IOHprofiler_set_number_of_objectives(1);
  IOHprofiler_set_lowerbound(0);
  IOHprofiler_set_upperbound(1);
  IOHprofiler_set_best_variables(1);
  Initilize_problem(dimension);
}
  
~OneMax() {};

void Initilize_problem(int dimension) {
  IOHprofiler_set_number_of_variables(dimension);
  IOHprofiler_set_optimal((double)dimension);
};
```

The <i>internal_evaluate</i> __must__ be implemented as well. It is used during evaluate process, returning a vector of (real) objective values of the corresponding variables __x__.
```cpp
std::vector<double> internal_evaluate(std::vector<int> x) {
  std::vector<double> y;
  int n = x.size();
  int result = 0;
  for (int i = 0; i != n; ++i) {
    result += x[i];
  }
  y.push_back((double)result);
  return y;
};
```

If you want to register your problem by `problem_name` and add it into a suite, please add functions creating instance as following codes.
```cpp
static OneMax * createInstance() {
  return new OneMax();
};

static OneMax * createInstance(int instance_id, int dimension) {
  return new OneMax(instance_id, dimension);
};
```
To register the problem, you can use the <i>geniricGenerator</i> in [IOHprofiler_class_generator](https://github.com/IOHprofiler/IOHexperimenter/blob/developing/src/Template/IOHprofiler_class_generator.hpp). For example, you can use the following statement to register and create __OneMax__ ,
```cpp
// Register
static registerInFactory<IOHprofiler_problem<int>,OneMax> regOneMax("OneMax");
// Create
std::shared_ptr<IOHprofiler_problem<int>> problem = genericGenerator<IOHprofiler_problem<int>>::instance().create("OneMax");
```

Creating Suites
==================================
[IOHprofiler_suite]() is the base `class` of suites of __IOHexperimenter__. The property variables of problems include:
* `problem_id`, a vector containing the ids of the problems to be tested.
* `instance_id`, a vector containing the ids of the instances of the problems. Intance ids specify which transformations will be applied to the problem. The original problem has instance_id 1; <i>scale</i> and <i>shift</i> are applied on objectives for instance_id in [2,100]; <i>XOR</i> will be applied on variables for instance_id in [2,50], and <i>sigma</i> function is applied on variables for instance_id in [51,100].
* `dimension`, a vector containing the dimensions of the problems.
* `number_of_problems`
* `number_of_instances`
* `number_of_dimensions`


The following functions for experiments are available to a suite:
* <i>get_next_problem</i>, return a shared point of problems of the suite in order.
* <i>addCSVLogger(logger)</i>, assigns a __IOHprofiler_csv_logger__ class to the suite.
* <i>IOHprofiler_set_suite_problem_id(problem_id)</i>
* <i>IOHprofiler_set_suite_instance_id(instance_id)</i>
* <i>IOHprofiler_set_suite_dimension(dimension)</i>
* <i>mapIDTOName</i>, is to match problem id and name. 

__IOHexperimenter__ provides a __PBO_suite__ for pseudo Boolean problems, but it is also easy to add your own suite. Creating a suite is done by registering problems in the suite and assigning ids to them.

Taking the implementation of __PBO_suite__ as an example, <i>constructor functions</i> are as below. In the constructor functions, the range of allowed `problem_id`, `instance_id` and `dimension` should be identified. In addition, <i>registerProblem()</i> must be included in the constructor functions.
```cpp
PBO_suite() {
  std::vector<int> problem_id;
  std::vector<int> instance_id;
  std::vector<int> dimension;
  for (int i = 0; i < 23; ++i) {
    problem_id.push_back(i+1);
  }
  for (int i = 0; i < 1; ++i) {
    instance_id.push_back(i+1);
  }
  dimension.push_back(100);
  
  IOHprofiler_set_suite_problem_id(problem_id);
  IOHprofiler_set_suite_instance_id(instance_id);
  IOHprofiler_set_suite_dimension(dimension);
  IOHprofiler_set_suite_name("PBO");
  registerProblem();
};

PBO_suite(std::vector<int> problem_id, std::vector<int> instance_id, std::vector<int> dimension) {
  for (int i = 0; i < problem_id.size(); ++i) {
    if (problem_id[i] < 0 || problem_id[i] > 23) {
      IOH_error("problem_id " + std::to_string(problem_id[i]) + " is not in PBO_suite");
    }
  }
  
  for (int i = 0; i < instance_id.size(); ++i) {
    if (instance_id[i] < 0 || instance_id[i] > 100) {
      IOH_error("instance_id " + std::to_string(instance_id[i]) + " is not in PBO_suite");
    }
  }

  for (int i = 0; i < dimension.size(); ++i) {
    if (dimension[i] < 0 || dimension[i] > 20000) {
      IOH_error("dimension " + std::to_string(dimension[i]) + " is not in PBO_suite");
    }
  }

  IOHprofiler_set_suite_problem_id(problem_id);
  IOHprofiler_set_suite_instance_id(instance_id);
  IOHprofiler_set_suite_dimension(dimension);
  IOHprofiler_set_suite_name("PBO");
  registerProblem();
}
```

<i>registerProblem()</i> is a virtual function of the base `IOHprofiler_suite` class. When you create a suite, it __must__ be implemented. Problems to be included in the suite can be registered by name. Afterwards, problem id and name should be mapped through <i>mapIDTOName></i> function, which enables the suite to recognize problems by problem id. Following is the <i>registerProblem()</i> function of __PBO_suite__.
```cpp
  registerInFactory<IOHprofiler_problem<int>,OneMax> regOneMax("OneMax");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Dummy1> regOneMax_Dummy1("OneMax_Dummy1");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Dummy2> regOneMax_Dummy2("OneMax_Dummy2");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Epistasis> regOneMax_Epistasis("OneMax_Epistasis");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Neutrality> regOneMax_Neutrality("OneMax_Neutrality");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Ruggedness1> regOneMax_Ruggedness1("OneMax_Ruggedness1");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Ruggedness2> regOneMax_Ruggedness2("OneMax_Ruggedness2");
  registerInFactory<IOHprofiler_problem<int>,OneMax_Ruggedness3> regOneMax_Ruggedness3("OneMax_Ruggedness3");

  registerInFactory<IOHprofiler_problem<int>,LeadingOnes> regLeadingOnes("LeadingOnes");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Dummy1> regLeadingOnes_Dummy1("LeadingOnes_Dummy1");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Dummy2> regLeadingOnes_Dummy2("LeadingOnes_Dummy2");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Epistasis> regLeadingOnes_Epistasis("LeadingOnes_Epistasis");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Neutrality> regLeadingOnes_Neutrality("LeadingOnes_Neutrality");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Ruggedness1> regLeadingOnes_Ruggedness1("LeadingOnes_Ruggedness1");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Ruggedness2> regLeadingOnes_Ruggedness2("LeadingOnes_Ruggedness2");
  registerInFactory<IOHprofiler_problem<int>,LeadingOnes_Ruggedness3> regLeadingOnes_Ruggedness3("LeadingOnes_Ruggedness3");
  
  registerInFactory<IOHprofiler_problem<int>,Linear> regLinear("Linear");
  registerInFactory<IOHprofiler_problem<int>,MIS> regMIS("MIS");
  registerInFactory<IOHprofiler_problem<int>,LABS> regLABS("LABS");
  registerInFactory<IOHprofiler_problem<int>,NQueens> regNQueens("NQueens");
  registerInFactory<IOHprofiler_problem<int>,Ising_1D> regIsing_1D("Ising_1D");
  registerInFactory<IOHprofiler_problem<int>,Ising_2D> regIsing_2D("Ising_2D");
  registerInFactory<IOHprofiler_problem<int>,Ising_Triangle> regIsing_Triangle("Ising_Triangle");

  mapIDTOName(1,"OneMax");
  mapIDTOName(2,"LeadingOnes");
  mapIDTOName(3,"Linear");
  mapIDTOName(4,"OneMax_Dummy1");
  mapIDTOName(5,"OneMax_Dummy2");
  mapIDTOName(6,"OneMax_Neutrality");
  mapIDTOName(7,"OneMax_Epistasis");
  mapIDTOName(8,"OneMax_Ruggedness1");
  mapIDTOName(9,"OneMax_Ruggedness2");
  mapIDTOName(10,"OneMax_Ruggedness3");
  mapIDTOName(11,"LeadingOnes_Dummy1");
  mapIDTOName(12,"LeadingOnes_Dummy2");
  mapIDTOName(13,"LeadingOnes_Neutrality");
  mapIDTOName(14,"LeadingOnes_Epistasis");
  mapIDTOName(15,"LeadingOnes_Ruggedness1");
  mapIDTOName(16,"LeadingOnes_Ruggedness2");
  mapIDTOName(17,"LeadingOnes_Ruggedness3");
  mapIDTOName(18,"LABS");
  mapIDTOName(22,"MIS");
  mapIDTOName(19,"Ising_1D");
  mapIDTOName(20,"Ising_2D");
  mapIDTOName(21,"Ising_Triangle");
  mapIDTOName(23,"NQueens");
```

If you want to register your suite called `suite_name`, please add following codes and modify names.
```cpp
static PBO_suite * createInstance() {
  return new PBO_suite();
};

static PBO_suite * createInstance(std::vector<int> problem_id, std::vector<int> instance_id, std::vector<int> dimension) {
  return new PBO_suite(problem_id, instance_id, dimension);
};
```
To register the suite, you can use the <i>geniricGenerator</i> in [IOHprofiler_class_generator](https://github.com/IOHprofiler/IOHexperimenter/blob/developing/src/Template/IOHprofiler_class_generator.hpp). For example, you can use the following statement to register and create __PBO_suite__ ,
```cpp
// Register
static registerInFactory<IOHprofiler_suite<int>,PBO_suite> regPBO("PBO");
// Create
std::shared_ptr<IOHprofiler_suite<InputType>> suite = genericGenerator<IOHprofiler_suite<int>>::instance().create("PBO");
);
```