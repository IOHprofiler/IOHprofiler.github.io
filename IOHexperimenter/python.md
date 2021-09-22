---
layout: page
title: IOHexperimenter - Python Interface
parent: IOHexperimenter
permalink: /IOHexp/python/
--- 


## Installation
This package can be installed directly from pip, using:

```cmd
pip install ioh
```

## Usage

The following walks through the typical use cases of the IOHexperimenter. 
You can find this in a jupyter notebook [on this page]() to run it in an interactive manner.

### Create a function object
The structure of the IOHexperimenter in python is almost equivalent to the C++ version, but with a few ease-of-use features added, such as easy access to any existing benchmark problem usin the 'get_problem' function:
```python
# Import the get_problem function
from ioh import get_problem
```
To check the usage and parameterization of this (and most other) functionality, we provide built-in docstrings, acessible as usual:

```python
#View docstring of get_problem
?get_problem
```
Based on this, you can then create a problem:


```python
#Create a function object, either by giving the function id from within the suite
f = get_problem(7, dim=5, iid=1, problem_type = 'BBOB')

#Or by giving the function name
f2 = get_problem("Sphere", dim=5, iid=1)
```
This problem contains a meta-data attributes, which consists of many standard properties, such as number_of_variables (dimension), name,...

```python
#Print some properties of the function
print((f.meta_data.name, f.meta_data.n_variables))
```

Additionally, the problem contains information on its bounds / conditions

```python
#Access the box-constrains for this function
f.constraint.lb, f.constraint.ub
```
The problem also tracks the current state of the optimization, e.g. number of evaluations done so far

```python
f.state.optimum_found, f.state.evaluations
```
And of course, the function can be evaluated easily:

```python
#Evaluate the function
f([0,0,0,0,0])
```

## Running an algorithm

We can construct a simple random-search example wich accepts an IOHprofiler problem as its argument.


```python
import numpy as np

def random_search(func, param_1 = None, budget = None):
    if budget is None:
        budget = int(func.number_of_variables * 1e4)

    f_opt = np.Inf
    x_opt = None

    for i in range(budget):
        x = np.random.uniform(func.lowerbound, func.upperbound)
        f = func(x)
        if f < f_opt:
            f_opt = f
            x_opt = x
    return f_opt, x_opt
```

To record data, we need to add a logger to the problem


```python
from ioh import logger
```
Within IOHexperimenter, several types of logger are available. Here, we will focus on the default logger, as described [in this section](/data_format)


```python
l = logger.Default(output_directory="data", folder_name="run", algorithm_name="random_search", algorithm_info="test of IOHexperimenter in python")
```

This can then be attached to the problem


```python
f.attach_logger(l)
```

Now, we can run the algorithm. The logger will automatically store the relevant performance data.


```python
random_search(f)
```

To ensure all data is written, we should clear the logger after running our experiments (this will no longer be required after version 0.32)

```python
l.flush()
```

## Tracking algorithm parameters

If we want to track parameters of the algorithm, we need to slightly restructure it by turning it into a class, where the variables we want to track are properties


```python
class opt_alg:
    def __init__(self, budget):
        self.budget = budget
        self.f_opt = np.Inf
        self.x_opt = None

    def __call__(self, func):
        for i in range(self.budget):
            x = np.random.uniform(func.constraint.lb, func.constraint.ub)
            f = func(x)
            if f < self.f_opt:
                self.f_opt = f
                self.x_opt = x
        return self.f_opt, self.x_opt
    
    @property
    def param_rate(self):
        return np.random.randint(100)
```

We could then use the function declare_logged_attributes of the logger to register the parameter to be tracked. Additionally, we can keep track of algorithm settings on a per-experiment or per-run level using declare_experiment_attributes and declare_run_attributes respectively.


```python
?l.declare_logged_attributes
```

Alternatively, we can use the experimenter class to easily run the benchmarking in multiple functions


```python
from ioh import experimenter
```

```python
?experimenter.Real
```

This can be initialized to a suite (PBO or BBOB are available) by providing lists of function ids (or names), dimensions, instance ids and a number of independent repetitions as follows:


```python
s = suite.BBOB([1,2,3], [1,2,3,4,5], [5,10])
l = logger.Default("temp")
o = opt_alg(1000)
exp = experimenter.Real(s, l, o, 5)
e.run()
```

## W-model and custom functions
As well as the PBO and BBOB suites, the IOHexperimenter provides access to the W-model functions ('W_model_function') and even an option to 
convert any python function into an IOH problem object, such that the logging functionality can be used. This can be done using the 'wrap_real_problem' and 'wrap_integer_problem' functions.

```python
def f_custom(x):
    return np.sum(x)
f_wrapped = problem.wrap_real_problem(f_custom, "custom_name", n_variables=5, optimization_type=OptimizationType.Minimization)
```