# PosDefManifoldML.jl

| **Documentation**  | 
|:---------------------------------------:|
| [![](https://img.shields.io/badge/docs-dev-blue.svg)](https://Marco-Congedo.github.io/PosDefManifoldML.jl/dev) |

**PosDefManifoldML** is a [**Julia**](https://julialang.org/) package for classifying data in the [**Riemannian manifolds**](https://en.wikipedia.org/wiki/Riemannian_manifold) **P** of real or complex [**positive definite matrices**](https://en.wikipedia.org/wiki/Definiteness_of_a_matrix). It is based on the [PosDefManifold.jl](https://github.com/Marco-Congedo/PosDefManifold.jl), [GLMNet.jl](https://github.com/JuliaStats/GLMNet.jl) and [LIBSVM.jl](https://github.com/mpastell/LIBSVM.jl) packages. 

[Machine learning](https://en.wikipedia.org/wiki/Machine_learning) (ML) in **P** can either operate directly on the manifold, which requires dedicated Riemannian methods, or the data can be projected onto the **tangent space**, where standard (Euclidean) machine learning methods apply (e.g., linear discriminant analysis, support-vector machine, logistic regression, random forest, deep neuronal networks, etc). 

![](/docs/src/assets/Fig1.jpg)

For the moment being, **PosDefManifoldML** implements the Riemannian **Minimum Distance to Mean (MDM)** classifier, which operates directly in **P**, the **elastic net logistic regression** (including the pure **Ridge** and pure **Lasso** logistic regresison model) and several **support-vector machine** classifiers in the tangent space. The models operating in the tangent space can be used also for traditional (Euclidean) feature vectors, making of this package also a nice interface to the binomial family of generalized linear models implemented in *GLMNet.jl* and all SVM models implemented in *LIBSVM.jl*

## Installation

Execute the following commands in Julia's REPL:

    ]add PosDefManifoldML
    add PosDefManifold

## Reviewers & Contributors

Independent reviewers are welcome.
To contribute, please check the section [how to contribute](https://marco-congedo.github.io/PosDefManifoldML.jl/dev/contribute/).

## Examples

```
using PosDefManifoldML

# simulate symmetric positive definite (SDP) matrices data for a 2-class problem.
# P is a vector of SPD matrices, y a vector of labels. Tr=training, Te=testing.
# SDP matrices will be all of size 10x10.
# The training set will have 30 matrices for class 1 and 40 for class 2.
# The testing set will have 60 matrices for class 1 and 80 for class 2.
PTr, PTe, yTr, yTe=gen2ClassData(10, 30, 40, 60, 80)

# # # MACHINE LEARNING IN THE PD MANIFOLD # # #

# (1)
# craete and fit (train) a Riemannian Minimum Distance to Mean (MDM) model:
model=fit(MDM(), PTr, yTr)
#
# predict labels (classify the testing set):
yPred=predict(model, PTe, :l)
#
# prediction error in percent
predictErr(yTe, yPred)
#
# predict probabilities for the matrices in `PTe` of belonging to each class:
predict(model, PTe, :p)

# (2)
# average accuracy obtained by 10-fold cross-validation:
cv = cvAcc(MDM(), PTr, yTr)

# # # MACHINE LEARNING IN THE TANGENT SPACE # # #

# (1)
# craete and fit (train) LASSO Logistic Regression models
# finding the best model by cross-validation:
model=fit(ENLR(), PTr, yTr)
#
# predict labels (classify the testing set) using the 'best' model:
yPred=predict(model, PTe, :l)
#
# prediction error in percent
predictErr(yTe, yPred)
#
# ...
#
# create and fit a RIDGE logistic regression model
model=fit(ENLR(), PTr, yTr; alpha=0)
#
#...
#
# create and fit an ELASTIC NET logistic regression model with alpha = 0.5
model=fit(ENLR(), PTr, yTr; alpha=0.5)

# (2)
# average accuracy obtained by 10-fold cross-validation:
cv = cvAcc(ENLR(), PTr, yTr; alpha=0.5)

# (1)
# craete and fit (train) an SVM with Radial Basis kernel
# finding the best model by cross-validation:
model=fit(SVM(), PTr, yTr)
#
# predict labels (classify the testing set) using the 'best' model:
yPred=predict(model, PTe, :l)
#
# prediction error in percent
predictErr(yTe, yPred)

# (2)
# average accuracy obtained by 8-fold cross-validation:
cv = cvAcc(SVM(), PTr, yTr; nFolds=8)
```

## About the Authors

[Marco Congedo](https://sites.google.com/site/marcocongedo), corresponding
author, is a research scientist of [CNRS](http://www.cnrs.fr/en) (Centre National de la Recherche Scientifique), working at [UGA](https://www.univ-grenoble-alpes.fr/english/) (University of Grenoble Alpes). **contact**: marco *dot* congedo *at* gmail *dot* com

Anton Adreev is a research engineer working at the same institution.

Saloni Jain is a student at the
[Indian Institute of Technology, Kharagpur](http://www.iitkgp.ac.in/), India.

| **Documentation**  | 
|:---------------------------------------:|
| [![](https://img.shields.io/badge/docs-dev-blue.svg)](https://Marco-Congedo.github.io/PosDefManifoldML.jl/dev) |



