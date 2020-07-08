---
layout: page
title: IOHexperimenter - Python Interface
parent: IOHexperimenter
permalink: /IOHexperimenter/python/
--- 


## Installation
This package can be installed directly from pip, using:

```cmd
pip install IOHexperimenter
```

## Usage

The following walks through the typical use cases of the IOHexperimenter. 
You can find this in a jupyter notebook [on this page]() to run it in an interactive manner.

### Create a function object
The IOHexperiment contains a few main classes, the most important of which is the IOH_function, which is the standard benchmark function.
```python
# Import the IOH_function class
from IOHexperimenter import IOH_function
```
It is highly recommended to look at the documentation for the individual classes and functions as follows:

```python
#View docstring of IOH_function
?IOH_function

Based on this, you can then create a an IOH_function object:


```python
#Create a function object, either by giving the function id from within the suite
f = IOH_function(7, 5, 1, suite = 'BBOB')
```


```python
#Or by giving the function name
f2 = IOH_function("Sphere", 5, 1)
```
This function has many standard properties, such as number_of_variables (dimension), bounds, best-so-far value reached,...

```python
#Acces properties of the function
f.number_of_variables
```


```python
f.lowerbound, f.upperbound
```

```python
f.final_target_hit
```
And of course, the function can be evaluated easily:

```python
#Evaluate the function
f([0,0,0,0,0])
```

## Running an algorithm

We can construct a simple random-search example wich accepts an argument of class IOH_function.


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
from IOHexperimenter import IOH_logger
```
Again, plenty of documentation on its arguments is available:

```python
?IOH_logger
```


```python
l = IOH_logger("data", "run", "random_search", "test of IOHexperimenter in python")
```

This can then be attached to the problem


```python
f.add_logger(l)
```

Now, we can run the algorithm. The logger will automatically store the relevant performance data.


```python
random_search(f)
```

To ensure all data is written, we should clear the logger after running our experiments

```python
f.clear_logger()
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
            x = np.random.uniform(func.lowerbound, func.upperbound)
            f = func(x)
            if f < self.f_opt:
                self.f_opt = f
                self.x_opt = x
        return self.f_opt, self.x_opt
    
    @property
    def param_rate(self):
        return np.random.randint(100)
```

We could then use the function track_parameters of the logger to register the parameter to be tracked


```python
?l.track_parameters
```

Alternatively, we can use the IOHexperimenter class to easily run the benchmarking in multiple functions


```python
from IOHexperimenter import IOHexperimenter
```

```python
?IOHexperimenter
```

This can be initialized to a suite (PBO or BBOB are available) by providing lists of function ids (or names), dimensions, instance ids and a number of independent repetitions as follows:


```python
exp = IOHexperimenter()
exp.initialize_BBOB([1,2], [5], [1,2], 2)
```

There is also the option to provide a set of standard python functions, which will be transformed to custom_IOH_functions within the IOHexperimenter


```python
?exp.initialize_custom
```

For now, we stick with the BBOB suite. We can set up the logging as follows:


```python
?exp.set_logger_location
```

```python
exp.set_logger_location("data_test", "run")
```

Parameter tracking can be set up using the following function:


```python
?exp.set_parameter_tracking
```

The algorithm we want to run has property "param_rate", so we can track this


```python
exp.set_parameter_tracking("param_rate")
```

We can customize the parallellization settings or the data storage settings using the following functions:


```python
?exp.set_parallel
```


```python
?exp.set_logger_options
```


Finally, we can run the experiment by simply providing the experimenter object with a list of algorithms to run


```python
exp([opt_alg(1000)])
```


```python
from IOHexperimenter import IOH_function
```
## W-model and custom functions
As well as the PBO and BBOB suites, the IOHexperimenter provides access to the W-model functions ('W_model_function') and even an option to 
convert any python function into an IOH_problem-based object, such that the logging functionality can be used. This can be done using the 'custom_IOH_function' class.