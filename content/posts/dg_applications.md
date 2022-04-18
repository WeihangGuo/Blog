+++ 
draft = false
date = 2022-04-19T02:33:47+08:00
title = "Applications of Differential Geometry in Artificial Intelligence"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

# Applications of Differential Geometry in Artificial Intelligence

(REPRINT FROM https://math.stackexchange.com/questions/584551/applications-of-differential-geometry-in-artificial-intelligence/2929482#2929482)

There are certainly several areas in which differential geometry (DG) affects AI, particularly within machine learning and computer vision.

## Data as Manifolds 

The basic idea of "manifold learning", which is a technique largely used for non-linear dimensionality reduction (NLDR), is to view the data as lying on a low-dimensional (Riemannian) manifold in a high-dimensional raw data space. Now, there are many methods for NLDR that are not really differential geometric in nature but are still called manifold learning methods. However, there are a few very "DG" methods.

One is Laplacian Eigenmaps. Every Riemannian manifold has an associated Laplace-Beltrami operator, with an accompanying operator spectrum. This spectrum has very good properties for representing functions on the manifold (see: Belkin & Niyogi's 2003 paper, and also "On the optimality of shape and data representation in the spectral domain" by Aflalo, Brezis, and Kimmel, 2015), and are thus a good choice for use as an embedding representation function. The Laplace-Beltrami operator is estimated via a discrete nearest neighbours graph Laplacian that converges to the true continuous operator of the underlying manifold, from which the data is sampled, in the limit.

Another one is Diffusion Maps. Here, one looks at the distance between points using an approximation of the "diffusion distance" on the underlying manifold. This is of course closely linked to the diffusion (heat) PDE on the manifold.

But, does data actually lie on low dimensional manifolds? There is some work on testing this, such as Fefferman et al, 2016, Testing the Manifold Hypothesis.

## Information Geometry of Statistical Models

If you consider a family of parameterized probability distributions, then an obvious way to measure distance between them is to look at e.g. the distance between the parameter values. However, this is actually a terrible way to measure this distance. Instead, one should look at how, say, the KL divergence between the distributions change, as one varies the parameters away from each other. This is (roughly) the idea of *[information geometry](https://en.wikipedia.org/wiki/Information_geometry)*. See also [here](https://stats.stackexchange.com/questions/51185/connection-between-fisher-metric-and-the-relative-entropy).

One essentially considers the space of probability distributions formed by some parametrized family, and then defines a Riemannian metric on the space (causing it to become a Riemannian manifold).

Of course, many statistical models are essentially probability distributions. In machine learning and pattern recognition, the goal is to compute the parameters. BUT, now that the parameter space is actually a Riemannian manifold, instead of following the "classic" gradient (which only considers Euclidean distance between parameters), you can follow the "natural gradient", which has vastly superior properties. See Amari, *Natural gradient works efficiently in learning*. It's main disadvantage is computation time, but people are looking at ways to fix that (e.g. see the work by Martens and Grosse). See also the work by Ollivier on Riemannian Metrics for Neural Networks.

The founder of the field, Amari, also discusses applications to ML in his book *Information Geometry and Its Applications*.

## Shape Understanding

Particularly in 3D computer vision and in efforts to apply machine learning to computer graphics, differential geometry plays a key role. The entire field of Geometric Deep Learning hinges on it. The issue arises because the data itself is manifold-valued, i.e. standard convolutional networks don't really apply to non-Euclidean data. A great reference is *Geometric deep learning: going beyond Euclidean data* by Bronstein et al.

## Curvature of Generative Models 

Recent deep generative models learn mappings between a data space and a latent space. In both cases, the surfaces formed within each space can be viewed as Riemannian manifolds. This suggests geometric ways to analyze or improve the algorithms. For example:

- Shao et al, *The Riemannian Geometry of Deep Generative Models*
- Arvanitidis et al, *Latent Space Oddity: On the Curvature of Deep Generative Models*