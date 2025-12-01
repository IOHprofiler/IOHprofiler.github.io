---
layout: page
title: Benchmark Design Challenge
permalink: /competitions/benchdesign 
has_children: false
---


# 2026 Competition: The Benchmark Design Challenge for Continuous Optimization

## Motivation

This competition challenges participants to design new and interesting benchmark functions for continuous optimization. Unlike traditional competitions that focus on solving predefined problems, this challenge inverts the paradigm: participants submit problem sets that best differentiate the performance of a predefined set of optimization algorithms. The objective is to advance our understanding of what makes optimization problems difficult, diverse, or distinctive with respect to algorithm performance. 

## Competition Setup

Participation is simple: you design a set of 25 benchmark problems of the form $[0,1]^N\rightarrow\mathbb{R}$.
The quality of your problem set is then judged in terms of performance diversity of a set of 5 commonly-used algorithms (CMA-ES, DE, PSO, BFGS, COBYLA). The exact evaluation procedure is presented below. Your problem set should meet the following criteria:

### Technical requirements
* The problems should be submitted in Python.
* If you use any non-default libraries in your problem code, make sure to include a requirements file
* Problem signature should match the examples provided in the getting started guide (only one input x and one output f(x), any other parameters should have a default value set)
* Your problem suite should be available as a dictionary of exactly 25 (name, problem) pairs
* To test whether your problem suite is eligible for submission, you can run the 'test_suite' function in the getting-started materials. 

### Problem properties
* Problem dimensionality: your problems should be scalable with dimensionality, but it is up to you to define the settings you use in your suite
* Your problems should use a bounded search space of [-5,5]. If you wish to use other bounds, you can scale the input
* Your problems should be deterministic
* 


## Evaluation
The evaluation procedure matches the setup provided in the getting-started materials, with the exception being the parameterizations of the used optimizers (to prevent overfitting). 

All submitted problem sets will be evaluated using a fixed set of five reference solvers:
* CMA-ES
* Differential Evolution
* Particle Swarm Optimization
* BFGS with restarts
* COBYLA


Each solver will be executed on every submitted function under controlled experimental settings.
The runs will use a budget of 5000 function evaluations.

The quality of each submitted benchmark suite will be measured by its ability to differentiate solver performance. The exact setup can be found in our getting-started materials. 

## Materials

Our getting-started materials will be made available January 7th, 2026. This will include the following:
* An example problem suite to showcase the required interface
* A function to validate whether a problem suite is eligible for submission
* The implementation of the used solvers, plus the code to run them on a problem suite
* The code used to judge the problem suite quality based on the collected performance data

## Submission

* All submissions made on or before *June 29, 2026 (AoE)* participate in the competition.
* Files to submit to the competition: simply make a copy of the problem_suite.py file and rename it to your team name. 
* 
* Note that each team can submit only **one** problem set.

## Hosting Events

To be announced

## Questions

Please send all inquiries to [Diederick](mailto:diederick.vermetten@lip6.fr). He will coordinate your request.

## Organizers
* Diederick Vermetten, CNRS and LIP6, Sorbonne University, Paris, France
* Niki van Stein, LIACS, Leiden University, The Netherlands