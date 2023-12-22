---
layout: page
title: Star Discrepancy Competition
permalink: /competitions/mabbob24
has_children: true
---


# GECCO 2024 Competition: Anytime Algorithms for Many-affine BBOB Functions

## Motivation


### Main Reference:


## Competition Setup

For this competition, we make use of the IOHprofiler environment. A python notebook with explicit examples can be found [here](https://github.com/IOHprofiler/IOHexperimenter/blob/master/example/example_star_discr.ipynb). Other examples and tutorials on IOHprofiler can be found on the links in the sidebar of this page.


## Evaluation

We will evaluate all submissions on a number of different instances in dimensions 2 and 5. The algorithms will be evaluated with respect to the anytime performance criterion (area over the convergence curve), with a fixed budget of $2000 \cdot d$.
For testing, we make available 1000 instances on which several baselines have been run. The settings to generate these exact instances can be found [here](https://zenodo.org/records/8208572), in addition the corresponding performance files for the baselines (which have also been processed and will uploaded to [IOHAnalyzer](https://iohanalyzer.liacs.nl) soon). 

For testing, we will generate a set of new instances (from the same distribution as the train instances) and evaluate the algorithms with respect to the normalized AOCC measure. Whichever algorithm reaches the highest average AOCC will be considered the winner. 
As a default, we assume that each submission should be considered for the 2 and 5 dimensional categories. If a submission should only be considered in one of the categories, i.e., either 2 or 5D, we ask the contributors to clearly state this in the submission email. 

Further examples and detailed requirements will be added in January. 

Submission Deadlines and Modalities:


## Submission

* All submissions made on or before *June 30, 2024 (AoE)* participate in the competition.
* To submit to the competition, we recommend creating a publically visible repository (e.g. on [Zenodo](zenodo.org)) where you upload the performance data of your algorithm as a single zip-file (named according to your algorithm name and the category you are submitting to) as well as the algorithm code used to collect this data. A short readme to allow for easier reproducibility checking is highly recommended. Finally, you should email the link to your repository to the competition organizers. 
* Competition participants may also consider submitting a short (2-page, including references) description of their submission for consideration for publication in the **Companion proceedings of GECCO 2024**. Note that the deadline for these submissions is considerably earlier than the competition entry deadline, on *April 14, 2024*. Submissions to the GECCO companion are handled via their submission system, instructions and relevant dates are similar to those of the workshop papers (see this GECCO website for details). Note, however, that competition papers are limited to 2 pages, including references.

## Hosting Events

The Competition on Anytime Algorithms for Many-affine BBOB Functions  is co-located with the [ACM/SIGEVO Genetic and Evolutionary Computation Conference, GECCO 2024](https://gecco-2024.sigevo.org/HomePage), July 14-18 (hybrid: Melbourne, Australia, and online)

## Questions

Please send all inquiries to [Diederick](mailto:d.l.vermetten@liacs.leidenuniv.nl). He will coordinate your request.

## Organizers
* Diederick Vermetten, LIACS, Leiden University, The Netherlands
* Konstantin Dietrich, TU Dresden, Germany
* Pascal Kerschke, TU Dresden, Germany
* Carola Doerr, CNRS and LIP6, Sorbonne University, Paris, France