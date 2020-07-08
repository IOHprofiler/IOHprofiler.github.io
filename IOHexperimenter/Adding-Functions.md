---
layout: page
title: Extending IOHexperimenter
parent: IOHexperimenter
permalink: /IOHexperimenter/extension/
--- 


## <a name="adding-new-problems"></a>Add new problems

To add your own problem to the __IOHexperimenter__, first make sure that the required preparation steps have been followed, as described [Here](/IOHexperimenter/Cpp//).
Once it is succesfully installed, please navigate to the folder `src/problems` and create the `.hpp` file of your problem there.

For this tutorial, we will use the `f_one_max_dummy1.hpp` file as an example. Use this file as the baseline for your own problem. 

The structure of any problem within the __IOHexperimenter__ is based on the `IOHprofiler_problem`-class, which is a template class which should be instantiated with the required variable type (`int` or `double`). The class contains the following variables:

* `problem_id` (optional), will be assigned if the problem is added to a suite, otherwise default by 0.
* `instance_id` (optional),  sets transformation methods on problems. Methods of transformation are implemented in __IOHprofiler_transformation__ class, as described [here 2.2](https://arxiv.org/pdf/1810.05281.pdf).
* `problem_name`
* `problem_type` (optional)
* `lowerbound`, is a vector of lowerbound for variables.
* `upperbound`, is a vector of upperbound for variables.
* `number_of_variables` (optional), is the dimension of the problem.
* `number_of_objectives`, is only available as 1 now. The functionality of multi-objectives is under development.
* `best_variables` (optional), is a vector of optimal solution, which is used to calculate the optimum. If both best_variables and optimum are not given, the optimum will be set as `std::numeric_limits<double>::max()` for maximization optimization, and as `std::numeric_limits<double>::lowest()` for minimization optimization.
* `optimal` (optional), is a vector of optimal objectives, but currently only single objective is supported. If both best_variables and optimum are not given, the optimum will be set as `std::numeric_limits<double>::max()` for maximization optimization, and as `std::numeric_limits<double>::lowest()` for minimization optimization.
* `maximization_minimization_flag`, sets as 1 for maximization, otherwise for minimization.

To create a problem of __IOHexperimenter__, the correct `IOHprofiler_problem<T>` needs to be inherited, and two functions need to be implemented: <i>construct functions</i> and <i>internel_evaluate</i>. Additionally, you can add pre-processing codes of allocating a problem in the virtual <i>prepare_problem</i> function, to make evluate process more efficient.

Taking the implementation of __OneMax__ with reduction transformation as an example, <i>construct functions</i> are as below. `problem_name` and `number_of_objectives` __must__ be set.
```cpp
OneMax_Dummy1(int instance_id = DEFAULT_INSTANCE, int dimension = DEFAULT_DIMENSION) {
  IOHprofiler_set_instance_id(instance_id);
  IOHprofiler_set_problem_name("OneMax_Dummy1");
  IOHprofiler_set_problem_type("pseudo_Boolean_problem");
  IOHprofiler_set_number_of_objectives(1);
  IOHprofiler_set_lowerbound(0);
  IOHprofiler_set_upperbound(1);
  IOHprofiler_set_best_variables(1);
  IOHprofiler_set_number_of_variables(dimension);
}

~OneMax_Dummy1() {};
```

The <i>internal_evaluate</i> __must__ be implemented as well. It is used during evaluate process, returning a (real) objective values of the corresponding variables __x__. In this case, the evaluate function applies a variable `info`. To avoid wasting time on calculating `info` within <i>internal_evaluate</i> for each evaluation, `info` is prepared in the <i>prepare_problem</i> function.
```cpp
std::vector<int> info;
void prepare_problem() {
  info = dummy(IOHprofiler_get_number_of_variables(),0.5,10000);
}

double internal_evaluate(const std::vector<int> &x) {
  int n = this->info.size();
  int result = 0;
  for (int i = 0; i != n; ++i) {
    result += x[this->info[i]];
  }
  return (double)result;
};
```

If you want to register your problem using `problem_name` and add it into a suite, please add functions for creating instances as following codes.

```cpp
static OneMax_Dummy1 * createInstance(int instance_id = DEFAULT_INSTANCE, int dimension = DEFAULT_DIMENSION) {
    return new OneMax_Dummy1(instance_id, dimension);
  };
};
```

To register the problem, you can use the <i>geniricGenerator</i> in [IOHprofiler_class_generator](https://github.com/IOHprofiler/IOHexperimenter/blob/developing/src/Template/IOHprofiler_class_generator.hpp). For example, you can use the following statement to register and create __OneMax__ with reduction transformation,

```cpp
// Register
static registerInFactory<IOHprofiler_problem<int>,OneMax_Dummy1> regOneMax_Dummy1("OneMax_Dummy1");
// Create
std::shared_ptr<IOHprofiler_problem<int>> problem = genericGenerator<IOHprofiler_problem<int>>::instance().create("OneMax_Dummy1");
```

## <a name="adding-new-transformations"></a>Adding new transformations
Transformations methods are applied during evaluate process of __IOHexperimenter__. Different transfromation interfaces have been create for integer (`int`) type variables and real (`double`) type variables. One class of transformation methods are available for new added function with int variables and `pseudo_Boolean_problem` problem type. the original problem is with instance_id 1, scale and shift are applied on objectives for instance_id in [2,100], XOR is applied on variables for instance_id in [2,50], and sigma function is applied on variables for instance_id in [51,100]. 
If you want to adapt specfic transformation methods for new added problems, methods shoud be implemented in `IOHprofiler_transformation.hpp` and revoked by the corresponding <i>variables_transformation</i> function and <i>objectives_transformation</i> function.


## <a name="adding-new-suites"></a>Adding new suites

[IOHprofiler_suite]() is the base `class` of suites of __IOHexperimenter__. The property variables of problems include:
* `problem_id`, a vector containing the ids of the problems to be tested.
* `instance_id`, a vector containing the ids of the instances of the problems. Intance ids specify which transformations will be applied to the problem. Methods of transformation are implemented in __IOHprofiler_transformation__ class, as described [here 2.2](https://arxiv.org/pdf/1810.05281.pdf).
* `dimension`, a vector containing the dimensions of the problems.
* `number_of_problems`
* `number_of_instances`
* `number_of_dimensions`

__IOHexperimenter__ provides [__PBO_suite__](/Suites/PBO/) for pseudo Boolean problems [__BBOB suite__](https://coco.gforge.inria.fr/downloads/download16.00/bbobdocfunctions.pdf) for continuous problems of COCO, but it is also easy to add your own suite. Creating a suite is done by registering problems in the suite and assigning ids to them.

Taking the implementation of [__PBO_suite__](/Suites/PBO/) as an example, <i>constructor functions</i> are as below. In the constructor functions, the range of allowed `problem_id`, `instance_id` and `dimension` should be identified. In addition, <i>registerProblem()</i> must be included in the constructor functions.

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
  loadProblem();
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
  loadProblem();
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