---
title: Random Matrix Theory
subtitle: For the months of November 2024 to February 2025 I will be focusing on developing a deep undestanding of linear models. 
          When analysing such models, the results tend to be produced quite easily with a solid theoretical understanding.
layout: default
date: 2024-11-20
keywords: statistics, linear models
published: true
---

Normally we think of matrices and vectors as a bunch of numbers. However, they can also be a bunch of random variables.
We can extend the traditional concepts of expectation, variance ect. to random variables. 



## Expectation & Variance
To begin with some definitions, we define the expectation of a random vector {% katexmm %} $\mathbf{y} = [y_1, \dots, y_p]$ {% endkatexmm %} as 
{% katexmm %}
$$\mathbb{E}[\mathbf{y}] = [\mathbb{E}[y_1], \dots, \mathbb{E}[y_p]]$$
{% endkatexmm %} 
The familiar properties of exepectation in extend to {% katexmm %} $\mathbb{R}^p$ {% endkatexmm %}. Namely, constant vectors and 
matrices can be *pulled out* such as {% katexmm %} $\mathbb{E}[A\mathbf{y}] = A\mathbb{E}[\mathbf{y}]$ {% endkatexmm %}. These properties
and definitions extend analogously for when dealing with a random matrix *A*. 

Defining the variance of a random vector is slightly trickier. We want to not jsut include the variance of the variables 
themselves, but also how the variables affect each other. Recall that the Variance of a random variable *Y* is the average 
squared deviation between *Y* and its mean which we will denote by  {% katexmm %} $\mu$ {% endkatexmm %}. That is, 
{% katexmm %}
$$\text{Var}(Y) \stackrel{\Delta}{=} \mathbb{E}[(Y-\mu)^2]$$
{% endkatexmm %} 
The analog to *p*-dimensional space theis so-called *covariance matrix* given by:
{% katexmm %}
$$\text{Var}(\textbf{y}) \stackrel{\Delta}{=} \mathbb{E}[(\textbf{y} - \mathbf{\mu})(\textbf{y} - \mathbf{\mu})^T]$$
{% endkatexmm %} 

From the definition, one can recognise the diagonal elements are the variance of each component 
{% katexmm %} $\text{Var}(\textbf{y})_{ii}  = \text{Var}(y_i)$ {% endkatexmm %}. The off-diagonal elements are the covariances of 
the elements: {% katexmm %} $\text{Var}(\textbf{y})_{ij}  = \text{Cov}(y_i, y_j) = \mathbb{E}[(y_i - \mu_i)(y_j - \mu_j)]$ {% endkatexmm %}. Similiar to the expectation, matrix-analogs exist for the 
