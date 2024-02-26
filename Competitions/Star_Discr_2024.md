---
layout: page
title: Star Discrepancy Competition
permalink: /competitions/stardiscr24
has_children: true
---


# GECCO 2024 Competition on Star Discrepancy Computation

## Motivation

Discrepancy measures are designed to quantify how well a set of points is distributed in a given domain. One of the most widely studied discrepancy notions is the $L_\infty$ star discrepancy, which measures the largest difference between the volume of boxes of type $[0,x)$ and the fraction $$\lvert  P \cap [0,x)\rvert/\lvert  P\rvert$$ of points in $$P\subseteq [0,1]^d$$ that lie inside this box.

Point sets of small $L_\infty$ star discrepancy have **important applications** in numerical approximation, in computer vision, but also in surrogate-based optimization, where they are commonly used as initial designs (a.k.a. DoEs). Designing point sets of low $L_\infty$ star discrepancy value is therefore an important task. Among the best-known constructions are the sequences by Sobol, by Halton, by Hammersley, etc. See [this page](https://en.wikipedia.org/wiki/Low-discrepancy_sequence) for more information about star discrepancy notions.

An important bottleneck in the design of low-discrepancy point sets is the **hardness of computing the star discrepancy value for a given point set.** The best problem-specific algorithm has a runtime that scales as $\lvert P\lvert^{d/2+1}$, which infeasible already for moderate dimensions (around 8). 
However, despite the difficulty of finding the global maximum that defines the $L_\infty$ star discrepancy of the set, local evaluations of $f(x) = \lvert P \cap [0,x)\rvert/\lvert P\lvert$ at selected points are inexpensive. This makes the problem tractable by black-box optimization approaches. 

The objective of this GECCO 2024 competition is to identify solvers that extend the settings for which we obtain accurate estimates for the $L_\infty$ star discrepancy of a given point set, where **setting** refers to the dimension d and the number of points in P.

Default numerical black-box optimization algorithms do not seem to be competitive for the star discrepancy problem. However, we expect that their performance can be considerably improved with moderate effort. 

What makes this competition particularly interesting for evolutionary computation research is that the problem can be tackled both as a purely numerical problem $\max\{f(x)\lvert x \in [0,1]^d\}$ and as a purely discrete problem $\max\{f(x)\lvert x \in [1..n]^d\}$. 

### Main Reference:
François Clement, Diederick Vermetten, Jacob de Nobel, Alexandre Jesus, Luí­s Paquete, Carola Doerr 
Computing Star Discrepancies with Numerical Black-Box Optimization Algorithms. Technical report, available [here](https://webia.lip6.fr/~doerr/Star-Discrepancy-BBO.pdf)

## Competition Setup

For this competition, we make use of the IOHprofiler environment. A python notebook with explicit examples can be found [here](https://github.com/IOHprofiler/IOHexperimenter/blob/competion_examples/example/Competitions/StarDiscrepancy/example_star_discr.ipynb). Other examples and tutorials on IOHprofiler can be found on the links in the sidebar of this page.
 
For this years competition, we focus on *Discrete* black-box optimization approaches, operating on $$\{1,2,...,n+1\}^d$$


## Evaluation

We will evaluate all submissions on a number of different instances in dimensions 2 to 20. The algorithms will be evaluated with respect to the fixed budget perspective. We will consider two categories and will determine a winner for each of the categories. We distinguish between two budget values:
* In the **low budget** category, each algorithm will be run for $500*d$ fitness evaluations and the best solution obtained during the run will be used as the result.
* In the **high budget** category, each algorithm will be run for $2,500*d$ fitness evaluations and the best solution obtained during the run will be used as the result.
Each algorithm will be run on each test evaluation instance 3 times in each category. Algorithms will be ranked for each considered instance. The winner is the approach that has the smallest average rank in each category, i.e., low budget or high budget.
As a default, we assume that each submission should be considered for the low and the high budget categories. If a submission should only be considered in one of the categories, i.e., either low or high budget, we ask the contributors to clearly state this in the submission email. Submission Deadlines and Modalities:


## Submission

* All submissions made on or before *June 30, 2024 (AoE)* participate in the competition.
* To submit to the competition, we recommend creating a publically visible repository (e.g. on [Zenodo](zenodo.org)) where you upload the performance data of your algorithm as a single zip-file (named according to your algorithm name and the category you are submitting to) as well as the algorithm code used to collect this data. A short readme to allow for easier reproducibility checking is highly recommended. Finally, you should email the link to your repository to the competition organizers. 
* Competition participants may also consider submitting a short (2-page, including references) description of their submission for consideration for publication in the **Companion proceedings of GECCO 2024**. Note that the deadline for these submissions is considerably earlier than the competition entry deadline, on *April 14, 2024*. Submissions to the GECCO companion are handled via their submission system, instructions and relevant dates are similar to those of the workshop papers (see this GECCO website for details). Note, however, that competition papers are limited to 2 pages, including references.

## Hosting Events

The Competition on Star Discrepancy Computation is co-located with the [ACM/SIGEVO Genetic and Evolutionary Computation Conference, GECCO 2024](https://gecco-2024.sigevo.org/HomePage), July 14-18 (hybrid: Melbourne, Australia, and online)

## Questions

Please send all inquiries to [Carola](mailto:carola.doerr@lip6.fr). She will coordinate your request.

## Organizers
* Carola Doerr, CNRS and LIP6, Sorbonne University, Paris, France
* Francois Clement, LIP6, Sorbonne University, Paris, France
* Diederick Vermetten, LIACS, Leiden University, The Netherlands
* Jacob de Nobel, LIACS, Leiden University, The Netherlands
* Alexandre Jesus, CISUC, DEI, University of Coimbra, Portugal
* Thomas Bäck, LIACS, Leiden University, The Netherlands
* Luí­s Paquete, CISUC, DEI, University of Coimbra, Portugal