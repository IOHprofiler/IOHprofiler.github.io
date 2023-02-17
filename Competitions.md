---
layout: page
title: Competitions and Workshops
permalink: /competitions
---


# Ongoing Competitions
## The Submodular Optization Competition
 The [Competition - Evolutionary Submodular Optimisation GECCO 2022](https://cs.adelaide.edu.au/~optlog/CompetitionESO2022.php), supporting Aneta Neumann, Frank Neumann (University of Adelaide, Australia), and Chao Qian (Nanjing University, China). For this competition, a python notebook with explicit examples can be found [here](https://github.com/IOHprofiler/IOHexperimenter/blob/competition_notebooks/example/Example_Submodular.ipynb).

## The Star Discrepancy competion

The $L_{\infty}$ star discrepancy is a measure for the regularity of a finite set of points taken from $[0,1)^d$. Low discrepancy point sets are highly relevant for Quasi-Monte Carlo methods in numerical integration and several other applications. Unfortunately, computing the $L_{\infty}$ star discrepancy of a given point set is known to be a hard problem, with the best exact algorithms falling short for even moderate dimensions around 8. However, despite the difficulty of finding the global maximum that defines the $L_{\infty}$ star discrepancy of the set, local evaluations at selected points are inexpensive. This makes the problem tractable by black-box optimization approaches.

For this competition, a python notebook with explicit examples can be found [here](https://github.com/IOHprofiler/IOHexperimenter/blob/competition_notebooks/example/Example_StarDiscr.ipynb).

# Ongoing Workshops

The [SBOX-COST workshop](https://sbox-cost.github.io/) at GECCO 2023 focusses on bound-constrained single-objective, noiseless, continuous optimization. For this workshop, a python notebook with explicit examples can be found [here](https://github.com/IOHprofiler/IOHexperimenter/blob/competition_notebooks/example/Example_SBOX.ipynb).


Benchmarking plays a critical role in the design and development of optimization algorithms. The way in which benchmark suites are set up thus influences the set of algorithms recommended to practitioners and biases the goals of algorithm designers. The focus of this workshop is on benchmarking algorithms on problems with box constraints (i.e. upper and lower limits on the input variables defining the domain of the search space). In practical applications, evaluating points outside of the domain is often impossible, or not sensible, and as such, should be avoided. However, in benchmarking as practiced today, problems are well-defined even outside of the stated parameter ranges, which translates to the algorithms being able to safely ignore these ranges and operate on infeasible solutions.

Setting up box constraints represents the simplest type of constraints and gives rise to the so-called box-constrained problems. In this workshop, we will discuss in more detail how the presence of box-constraints impacts the performance of optimization algorithms and provide participants with a new variant of the BBOB suite called SBOX-COST for continuous, single-objective, noiseless optimization.

