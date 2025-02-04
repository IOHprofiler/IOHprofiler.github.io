---
layout: page
title: Star Discrepancy Competition
permalink: /competitions/mabbob25
has_children: true
---


# GECCO 2025 Competition: Anytime Algorithms for Many-affine BBOB Functions

## Motivation

The MA-BBOB generator has recently been proposed as a way to create large sets of benchmark problems based on the popular BBOB suite. To explore this suite, we invite the community to submit their algorithms for evaluation on a large set of problems created using this generator. 

### Main Reference:

Vermetten, D., Ye, F., Bäck, T., & Doerr, C. (2023). MA-BBOB: A Problem Generator for Black-Box Optimization Using Affine Combinations and Shifts. [arXiv preprint arXiv:2312.11083](https://arxiv.org/abs/2312.11083).

## Competition Setup

For this competition, we make use of the IOHprofiler environment. A full getting-started document + baseline data can be found [here](https://github.com/IOHprofiler/IOHdata/blob/master/MABBOB_GettingStarted.zip). Other examples and tutorials on IOHprofiler can be found on the links in the sidebar of this page.


## Evaluation

We will evaluate all submissions on a number of different instances in dimensions 2 and 5. The algorithms will be evaluated with respect to the anytime performance criterion (area over the convergence curve), with a fixed budget of $2000 \cdot d$.
For testing, we make available 1000 instances on which several baselines have been run. The settings to generate these exact instances can be found [here](https://github.com/IOHprofiler/IOHdata/blob/master/MABBOB_GettingStarted.zip), in addition the corresponding performance files for the baselines. 

A notebook which includes a script for evaluating and visualizing your performance data will be made available soon. This will use our new IOHinspector package, and replace the scripts used last year. This change allows you to run the exact same analysis methods we will use during the judging process. 

For testing, we will generate a set of new instances (from the same distribution as the train instances) and evaluate the algorithms with respect to the normalized area over the convergence cuver measure. Whichever algorithm reaches the highest average AOCC will be considered the winner (note: maximizing AOCC is eqiuvalent to minimizing area under the ECDF). 
As a default, we assume that each submission should be considered for the 2 and 5 dimensional categories. If a submission should only be considered in one of the categories, i.e., either 2 or 5D, we ask the contributors to clearly state this in the submission email. 

For more technical details and examples, please look at [this notebook](https://github.com/IOHprofiler/IOHexperimenter/blob/master/example/Competitions/MA-BBOB/Example_MABBOB.ipynb).

Submission Deadlines and Modalities:


## Submission

* All submissions made on or before *June 30, 2025 (AoE)* participate in the competition.
* To submit to the competition, we recommend creating a publically visible repository (e.g. on [Zenodo](zenodo.org)) where you upload the performance data of your algorithm as a single zip-file (named according to your algorithm name and the category you are submitting to) as well as the algorithm code used to collect this data. A short readme to allow for easier reproducibility checking is highly recommended. Finally, you should email the link to your repository to the competition organizers. 
* Competition participants may also consider submitting a short (2-page, including references) description of their submission for consideration for publication in the **Companion proceedings of GECCO 2025**. Note that the deadline for these submissions is considerably earlier than the competition entry deadline, on *April 14, 2025*. Submissions to the GECCO companion are handled via their submission system, instructions and relevant dates are similar to those of the workshop papers (see this GECCO website for details). Note, however, that competition papers are limited to 2 pages, including references.
* Note that each team can submit a total of **two** algorithms. For ranking, we will consider the single best of these two algorithms (not the joined virtual best).

## Hosting Events

The Competition on Anytime Algorithms for Many-affine BBOB Functions  is co-located with the [ACM/SIGEVO Genetic and Evolutionary Computation Conference, GECCO 2025](https://gecco-2025.sigevo.org/HomePage), July 14-18 (hybrid: Malaga, Spain, and online)

## Questions

Please send all inquiries to [Diederick](mailto:d.l.vermetten@liacs.leidenuniv.nl). He will coordinate your request.

## Organizers
* Diederick Vermetten, LIACS, Leiden University, The Netherlands
* Carola Doerr, CNRS and LIP6, Sorbonne University, Paris, France