---
title: "seRNN: Spatially-Embedded Recurrent Neural Networks"
tags:
  - machine-learning
  - software
  - emergence
  - projects
---
Recurrent neural networks (RNNs) with 3D euclidian embedding and distance/connectivity based loss function term.

![[seRNN_embedding.png]]

$Loss = Loss_{Cross Entropy} + (Weights \odot Distances \odot Connectivity)$

Key Q: why isn't my model getting to 100% accuracy? Am I doing something wrong? or is it a Keras/PyTorch difference?

## Notes for Meeting

Spherical Initialisation
Bring up the recent GNN developmental growth paper (Risi et. al.)
