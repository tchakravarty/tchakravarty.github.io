---
layout: post
title:  "DCS: A New Package for Dynamic Conditional Score Models (v0.1.1)"
date:   2015-02-24 00:29:38
categories: [dcs, econometrics]
---
This is the first release of the `DCS` package, and you can find it [here](https://github.com/tchakravarty/DCS).
 Currently no binaries or source distributions are available, and you will need to use `devtools` to install from Github. 
  {% highlight R %}
  devtools::install_github("tchakravarty/DCS")
  {% endhighlight %}
  
  At the moment, it only supports the Gaussian GAS copula model from the Creal, Koopman & Lucas (2013) paper, and I anticipate being
  able to provide support for the other estimators in that paper in the next revision. 
  
  The package is a little thin on documentation, and I also anticipate cleaning that up in the next revision. There is 
  really only one function in the package, `GaussianGASCopula` that takes a matrix, and a starting parameter values
  and return the estimated parameters for the Gaussian GAS copula model.