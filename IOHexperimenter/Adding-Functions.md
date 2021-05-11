---
layout: page
title: Extending IOHexperimenter
parent: IOHexperimenter
permalink: /IOHexp/extension/
--- 


## <a name="adding-new-problems"></a>Add new problems

We offer a very simple and convenient interface for integrating new benchmark problems/functions. First, you could define a new `test_problem` as you like. Note that the `<vector>` header is already imported in "ioh.hpp".

```C++
#include "ioh.hpp"

std::vector<double> test_problem(const std::vector<double> &)
{
    // the actual function body start here
    // ...
}
```

Then, you only need to "wrap" this new function as follows:

```c++
auto new_problem = ioh::problem::wrap_function<double>(
  &test_problem,
  "test_problem" // name for the new function
);
std::cout << new_problem.meta_data() << std::endl;
```

After wrapping, we could also create this `test_problem` from the problem factory. Note that,
the instance id is ineffective in this approach since we haven't implemented it for the wrapped problem.

```c++
auto &factory = ioh::problem::ProblemRegistry<ioh::problem::Real>::instance();
auto new_problem_f = factory.create(
  "test_problem",  // create by name
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

Please check [this example](https://github.com/IOHprofiler/IOHexperimenter/blob/759750759331fff1243ef9e121209cde450b9726/example/problem_example.h#L51) for adding continuous problems in this manner.