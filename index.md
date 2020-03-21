# Google Summer of Code

## Overview of the project

In constraint-based metabolic modelling, physical and biochemical constraints define a polyhedral convex set of feasible flux vectors. Uniform sampling of this set provides an unbiased characterization of the metabolic capabilities of a biochemical network. The corresponding polyhedra typically lie in hundreds or thousands of dimensions. Fast convergence to the stationary uniform distribution is crucial from a computational point of view, to enable reliable and tractable sampling of genome-scale biochemical networks.

## Tests
The mentors asks for the students to do one or more of the following tests before contacting them.

Here are the aforementioned [tests](https://github.com/GeomScale/gsoc2020/wiki/High-dimensional-sampling-with-applications-to-structural-biology#tests).  

## Solutions of tests

Click on each task to see the relevant answers.

### [Easy](test_easy.html): 
Compile and run VolEsti. Use the R extension to visualize sampling in a polytope.

### [Medium](medium_again.html): 
Import the [e.coli dataset](http://bigg.ucsd.edu/models/e_coli_core) from bigg and create a matrix in R

### [Hard](test_hard.html): 
Support lower dimensional polytopes in volesti and use existing methods to sample from them
