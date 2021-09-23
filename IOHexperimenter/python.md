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
#Show the state of the optimization
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
#Create a basic random search algorithm
import numpy as np

def random_search(func, param_1 = None, budget = None):
    if budget is None:
        budget = int(func.meta_data.n_variables * 1e4)

    f_opt = np.Inf
    x_opt = None

    for i in range(budget):
        x = np.random.uniform(func.constraint.lb, func.constraint.ub)
        f = func(x)
        if f < f_opt:
            f_opt = f
            x_opt = x
    return f_opt, x_opt
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

If we want to track parameters of the algorithm, we need to slightly restructure it by turning it into a class, where the variables we want to track are properties


```python
class opt_alg:
    def __init__(self, budget):
        self._budget = budget
        self.f_opt = np.Inf
        self.x_opt = None
        self._seed = np.random.get_state()[1][0]
        self._rng = np.random.default_rng(self._seed)

    def __call__(self, func):
        for i in range(self._budget):
            x = self._rng.random.uniform(func.constraint.lb, func.constraint.ub)
            f = func(x)
            if f < self.f_opt:
                self.f_opt = f
                self.x_opt = x
        #Set new seed for future runs        
        self._seed = np.random.get_state()[1][0]
        self._rng = np.random.default_rng(self._seed)
        return self.f_opt, self.x_opt
    
    @property
    def param_rate(self):
        return np.random.randint(100)

    @property
    def seed(self):
        return self._seed
    
#create an instance of this algorithm
o = opt_alg(1000)
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

## Using the experimenter module

In addition to creating each problem individually, we can make use of the built-in experimenter module, which can be imported as follows:

```python
from ioh import Experiment
```

```python
?Experiment
```

At its core, the Experimenter object contains three parts: 
- An optimization algorithm (which takes a problem as input)
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

## Using custom functions
In addition to the interfaces to the built-in functions, IOHexperimenter provides an easy way to wrap any problem into the same ioh-problem structure for easy use with the logging and experiment modules. This can be done using the 'wrap_real_problem' and 'wrap_integer_problem' functions. An example is shown here:

```python
from ioh import problem, OptimizationType

#Define an evaluation method
def f_custom(x):
    return np.sum(x)
#Call the wrapper
problem.wrap_real_problem(f_custom, "custom_name", n_variables=5, optimization_type=OptimizationType.Minimization)

#Call get_problem to instantiate a version of this problem
f = get_problem('custom_name', iid=0, dim=5)
```

Note that changing the iid of the problem is not yet supported, but changing the dimensionality does work, assuming the evaluate function can handle inputs of the specified size.

```python
f = get_problem('custom_name', iid=0, dim=10)
```

## Using the W-model functions
In addition to the PBO and BBOB functions, the W-model problem generators (one based on OneMax and one based on LeadingOnes) are also avalable. 

```python
?problem.WModelOneMax
```

```python
f = problem.WModelLeadingOnes(iid = 1, dim = 100, dummy_select_rate = 0.5, epistasis_block_size = 1, neutrality_mu = 0, ruggedness_gamma = 0 )
```


