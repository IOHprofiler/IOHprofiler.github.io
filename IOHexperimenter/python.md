---
layout: page
title: IOHexperimenter - Python Interface
parent: IOHexperimenter
permalink: /IOHexp/python/
--- 

## Overview

This document shows the basic usecases of IOHexperimenter in python. 
Note that the examples shown are also available as an Ipython notebook, which can be found at: https://github.com/IOHprofiler/IOHprofiler.github.io/blob/62a1d4f688dfec357630deef36d9ad2adfb190ae/IOH_Tutorial_python.ipynb

## Installation
This package can be installed directly from pip, using:

```cmd
pip install ioh
```

## Usage

There are a few main ways of using the IOHexperimenter. All of them are based on the 'problem' as defined by IOHexperimenter. 
Then, one can either use this function directly, or, using the 'experimenter'-class, automate the entire benchmarking pipeline.
On this page, we will first introduce the basic aspects of this 'problem' object, which will be the part of the codebase which your algorithm needs to interact with. Then, we introduce several usecases of IOHexperimenter, ranging from running an algorithm on one existing problem to complete benchmarking pipelines.

### Create a function object
The structure of the IOHexperimenter in python is almost equivalent to the C++ version, but with a few ease-of-use features added, such as easy access to any existing benchmark problem usin the 'get_problem' function:
```python
# Import the get_problem function
from ioh import get_problem
```
To check the usage and parameterization of this (and most other) functionality, we provide built-in docstrings. These are available in Ipython as follows:

```python
#View docstring of get_problem
?get_problem
```

If you are not using Ipython, you can access the docstrings using pythons built-in help-function:

```python
#View docstring of get_problem
help(get_problem)
```

This will output the docstrings as follows:
```
Signature:
ioh.get_problem(
    fid: Union[int, str],
    instance: int = 1,
    dimension: int = 5,
    problem_type: str = 'Real',
) -> Union[ioh.iohcpp.problem.Real, ioh.iohcpp.problem.Integer]
Docstring:
Instantiate a problem based on its function ID, dimension, instance and suite

Parameters
----------
fid: int or str
    The function ID of the problem in the suite, or the name of the function as string
dimension: int
    The dimension (number of variables) of the problem
instance: int
    The instance ID of the problem
problem_type: str
    Which suite the problem is from. Either 'BBOB' or 'PBO' or 'Real' or 'Integer'
    Only used if fid is an int.
```

Based on this, you can then access a problem, for example from the 'BBOB' suite of continuous problems:

```python
#Create a problem object, either by giving the problem id from within the suite
f = get_problem(7, dimension=5, instance=1, problem_type = 'Real')

#Or by giving the problem name
f2 = get_problem("Sphere", dimension=5, instance=1)
```
This problem contains a meta-data attributes, which consists of many standard properties, such as number_of_variables (dimension), name,...

```python
#Print some properties of the problem
print(f.meta_data)
```

Additionally, the problem contains information on its bounds / conditions

```python
#Access the box-constrains for this problem
f.constraint.lb, f.constraint.ub
```
The problem also tracks the current state of the optimization, e.g. number of evaluations done so far

```python
#Show the state of the optimization
print(f.state)
```
And of course, the problem can be evaluated easily:

```python
#Evaluate the problem
f([0,0,0,0,0])
```

## Running an algorithm

To show how to use IOHexperimenter to run an algorithm on a built-in problem, we can construct a simple random-search example wich accepts an IOHprofiler problem as its argument.


```python
#Create a basic random search algorithm
import ioh
import numpy as np

def random_search(problem: ioh.problem.Real, seed: int = 42, budget: int = None) -> ioh.RealSolution:
    np.random.seed(seed)
    
    if budget is None:
        budget = int(problem.meta_data.n_variables * 1e4)

    for _ in range(budget):
        x = np.random.uniform(problem.constraint.lb, problem.constraint.ub)
        
        # problem automatically tracks the current best search point
        f = problem(x)
        
    return problem.state.current_best
```

To record data, we need to add a logger to the problem


```python
#Import the ioh logger module
from ioh import logger
```
Within IOHexperimenter, several types of logger are available. Here, we will focus on the default logger (called Analyzer as of version 0.32, Default for version 0.31 and earlier), as described [in this section](/data_format). Note that the logging can be customized by adding new triggers. Additionally, starting in version 0.32, the ability to store search points directly is added by using the store_positions-parameter. 


```python
#Create default logger compatible with IOHanalyzer
l = logger.Analyzer(root="data", folder_name="run", algorithm_name="random_search", algorithm_info="test of IOHexperimenter in python")
```

This can then be attached to the problem


```python
#Add the logger to the problem
f.attach_logger(l)
```

Now, we can run the algorithm. The logger will automatically store the relevant performance data.


```python
random_search(f)
```

For versions of ioh prior to 0.31, we need to explicitly ensure all data is written, so we should clear the logger after running our experiments. This is no longer be required after version 0.32.

```python
l.flush()
```

## Tracking algorithm parameters

If we want to track adaptive parameters of the algorithm, we require an object in which the parameters of the algorithm are stored. In the below example, the random search algorithm is restructured into a class for this purpose. Alternatively, we could also create a seperate object which holds the parameters.

```python
class RandomSearch:
    def __init__(self, budget: int):
        self._budget = budget
        self.seed = np.random.get_state()[1][0]
        self._rng = np.random.default_rng(self.seed)

    def __call__(self, func: ioh.problem.Real):
        for i in range(self._budget):
            x = self._rng.uniform(func.constraint.lb, func.constraint.ub)
            f = func(x)
        #Set new seed for future runs        
        self.seed = np.random.get_state()[1][0]
        self._rng = np.random.default_rng(self.seed)
        return
    
    @property
    def param_rate(self) -> int:
        return np.random.randint(100)

    
#create an instance of this algorithm
o = RandomSearch(1000)
```

We can then identify three different levels at which to track parameters:

### Tracking adaptive parameters
The first type of parameters are the most common: parameters which we want to track during the search procedure, e.g. an adaptive stepsize. To track this type of parameter, we can make use of the 'watch' function of the logger as follows:

```python
l.watch(o, ['param_rate'])
```

### Tracking run parameters
The second type of parameter is a per-run parameter. This can be something like the used random seed. To track this, we can use the following:

```python
l.add_run_attributes(o, ['seed'])
```

### Tracking experiment parameters
The final type of parameters to track is the most high-level. This can be for example static algorithm parameters or other information about the experiment, which can be added as follows:

```python
l.add_experiment_attribute('budget', '1000')
```

---
**NOTE**

The methods for tracking parameters, e.g. `watch`, `add_run_attributes` and `add_experiment_attribute` can only be called before `f.attach_logger(l)` is called. Otherwise, the function will have no effect. 

---

## Using the experimenter module

In addition to creating each problem individually, we can make use of the built-in experimenter module, which can be imported as follows:

```python
from ioh import Experiment
```

At its core, the Experimenter object contains three parts: 
- An optimization algorithm (which takes a ioh-based problem as input)
- Information on the collection of problems to be executed
- Information on the logging procedure

The suite object can be created using the suite-module from ioh as follows:


```python
exp = Experiment(algorithm = o, #Set the optimization algorithm
fids = [1,2,3], iids = [1,2,3,4,5], dims = [5,10], reps = 5, problem_type = 'BBOB', #Problem definitions
njobs = 4, #Enable paralellization
logged = True, folder_name = 'IOH_data', algorithm_name = 'Random_Search', store_positions = True, #Logging specifications
experiment_attributes = {'budget' : '1000'}, run_attributes = ['seed'], logged_attributes = ['param_rate'], #Attribute tracking
merge_output = True, zip_output = True, remove_data = True #Only keep data as a single zip-file
)

```

This can be run as follows:

```python
exp.run()
```

## Using custom problems
In addition to the interfaces to the built-in problems, IOHexperimenter provides an easy way to wrap any problem into the same ioh-problem structure for easy use with the logging and experiment modules. This can be done using the 'wrap_real_problem' and 'wrap_integer_problem' functions. An example is shown here:

```python
from ioh import problem, OptimizationType

#Define an evaluation method
def f_custom(x):
    return np.sum(x)
#Call the wrapper
problem.wrap_real_problem(f_custom, "custom_name",  optimization_type=OptimizationType.Minimization)

#Call get_problem to instantiate a version of this problem
f = get_problem('custom_name', instance=0, dimension=5)
```

Note that you can also add transformations based on the instance id, for example as follows:

```python
# Transformation function of x-attributes based on the instance id (numeric, default is 0). Note that argument order is fixed, but names are flexible.
def transform_vars(x, instance):
    x[1] += instance
    return x

# Transformation function of x-attributes based on the instance id (numeric, default is 0). Note that argument order is fixed, but names are flexible.
def transform_obj(y, instance):
    return y * instance

# Function to calculate the objective (both x and corresponding objective value) based on the instance id (numeric, default is 0). Note that argument order is fixed, but names are flexible.
def calc_obj(instance, dimension):
    return [instance]*dimension, instance


#We can then add these transformations when wrapping the problem:
problem.wrap_real_problem(f_custom, name="custom_name2",
optimization_type=OptimizationType.Minimization, 
         transform_variables=transform_vars, transform_objectives=transform_obj, 
         calculate_objective=calc_obj)
```



```python
f = get_problem('custom_name2', instance=3, dimension=10)
```

When using custom problems, they can be used with the Experiment class just the same as pre-defined problems. Note that you can see the problem id as follows:

```python
f.meta_data.problem_id
```
```python
exp = Experiment(algorithm = o, #Set the optimization algorithm
fids = [1,25], iids = [0], dims = [5,10], reps = 5, problem_type = 'BBOB', #Problem definitions
njobs = 4, #Enable paralellization
logged = True, folder_name = 'IOH_data', algorithm_name = 'Random_Search', store_positions = True, #Logging specifications
experiment_attributes = {'budget' : '1000'}, run_attributes = ['seed'], logged_attributes = ['param_rate'], #Attribute tracking
merge_output = True, zip_output = True, remove_data = True #Only keep data as a single zip-file
)
```

Alternatively, we can use custom problems without first wrapping them, by using the 'add_custom_problem' function of Experiment:

```python
exp = ioh.Experiment(0, fids=[], iids=[1], dims=[10], njobs=4)
    exp.add_custom_problem(problem, "problem", 
         transform_variables=tx, transform_objectives=ty, calculate_objective=co)
    exp()
```

## Using the W-model problems
In addition to the PBO and BBOB problems, the W-model problem generators (one based on OneMax and one based on LeadingOnes) are also avalable. 

```python
?problem.WModelOneMax
```

```python
f = problem.WModelLeadingOnes(instance = 1, n_variables = 100, dummy_select_rate = 0.5, epistasis_block_size = 1, neutrality_mu = 0, ruggedness_gamma = 0 )
```
