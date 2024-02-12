---
title: "GUM: Gaussian-Uniform Mixtures"
tags:
  - software
  - projects
---
## Motivation

In biostatistics, we are often provided with noisy experimental data which under a set of basic transformations can be usefully imagined to have been drawn from a set of simple underlying distributions. Often in nature, due to behaviours such as the [law of large numbers](https://en.wikipedia.org/wiki/Law_of_large_numbers) and the [central-limit theorem](https://en.wikipedia.org/wiki/Central_limit_theorem), we can approximate the behaviour of datasets with normal (or gaussian) distributions.

As an extension of this: in systems where there may be multiple possible states, such as  activation of a certain cell behaviour, we can expect to see multiple distributions in data. Thus the distribution of all the data (when not separated by state) can be expressed as a `mixture` of the underlying distributions. In biological data, these are often normal and thus the [Gaussian Mixture Model](https://scikit-learn.org/stable/modules/mixture.html) (GMM) has become a powerful tool for modellers and statisticians.

Finally,  often despite the best intentions of the modeller, the underlying distributions may be more complicated than the estimated distributions or contaminated with noisy values from sources of error. In gene expression microarray and protein expression datasets, prior work has identified errant "tails" in the distributions, which interferes with the GMM fitting process and thus returns unsatisfactory summaries of the data.

In these cases, a GMM might be fitted to data that shows clear bi-modality to identify an threshold of expression between the two expression 'states'. This threshold can then be interpreted as a way to categorise the data into "Low" and "High" expression, or similar, which allows for different downstream interpretations and analysis.

[@distefano2021]

%% An example figure of some expression data showing tailed behaviour %%
## Existing solutions

### GMM

Fitting a GMM to a set of data can be carried out with the `sklearn` package, and comes with automatics features for resetting bad initial configurations and identifying when singularities occur in the covariance matrix. To fit a GMM, the only required parameter is the number of gaussian components, but there are also options to constrain the covariance matrix to produce only

### GMMChi

GMM chi

## Our Proposition

GUM
Integrating a uniform term into the GMM optimisation

## Benchmarks

Simulated Dataset Design
Performance
Time
