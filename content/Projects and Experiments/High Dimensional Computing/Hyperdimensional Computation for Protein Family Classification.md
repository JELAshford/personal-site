---
title: Hyperdimensional Computing for Protein Family Classification
tags:
  - projects
  - machine-learning
  - software
---
## Protein Families (PFam) Dataset

In this technical test, I was tasked with creating a protein subunit classifier: a model that takes as input a **sequence of amino acids** and emits a **classification of the PFam family** to which most likely belongs. The dataset has been hosted on Kaggle [here](https://www.kaggle.com/datasets/googleai/pfam-seed-random-split), along with a set of previous analyses and modelling approaches. You can find my approach with scripts to reproduce all results at [this GitHub repo](https://github.com/JELAshford/hyperdim-proteins).

Protein sequences are described as a series of amino acids, derived from a sequence of DNA bases. The function of a protein is intimately linked to the 3D structure it takes when "folded" in the cell, and this folding is in-turn determined by the sequence and local environment. Previous research has identified that many important properties of protein function can be predicted from the amino acid sequence [@rao2019], and that there are conserved patterns across many proteins that carry out similar functions, allowing for proteins to be engineered to solve novel cellular tasks [@wang2023f].

PFam was a database of protein sub-sequences grouped into [families](https://en.wikipedia.org/wiki/Protein_family) by their sequence similarities and dynamics [@mistry2021]. These families usually represent sequences with common evolutionary history, and domains or motifs with similar functional behaviour. PFam, along with 12 other protein databases has now been subsumed into [InterPro](https://www.ebi.ac.uk/interpro/) to provide more streamlined access for tooling and future discovery [@paysan-lafosse2023].

There are a range of previous publications dealing with modelling using PFam, including:
- TAPE: PFam used as pre-training corpus, with a next/masked amino-acid prediction task [@rao2019].
- ProtCNN: A learned embedding followed by a Convolutional network to predict PFam class [@bileschi2022a].
- ProtENN: Ensemble of 19 ProtCNN models, achieves error rate as low as 0.162% [@bileschi2022a].
- Protein-Vec: Word embeddings (trained with triplet loss) to create latent space good for downstream predictions, pre-trained on PFam [@hamamsy2023].
- Protein secondary structure prediction with transfer learning due to pre-training on PFam [@rives2021]
- Using ESM as pre-trained embedding before ProtCNN architecture to boost performance on PFam [@bugnon2023].

## Hyperdimensional Computing
Hyperdimensional computing (HDC) employs the sparsity and natural orthogonality of high-dimensional vector (HDV) spaces to carry out vector-symbolic computing (VSC). A recent review of this technique can be found in Dewulf et al. [@dewulf2023a], outlining how encoding and composing samples in these high-dimensional spaces allows for efficient inference using distance measures. 
### Implementation
HDC requires a high dimensional space, usually in the order of ~10,000, and a set of values for each of those dimensions to take - I use ±1 in line with much of the previous literature. In addition, to encode information set of functions is needed that can compare, combine and permute these vectors in a structured way. Here, we define the number of dimensions (`N_DIM`) and all functions as `numpy` operations:

```python
N_DIM = 10_000
hdv = lambda size=N_DIM: np.random.choice([-1, 1], size=size)
bundle = lambda hdv_list: np.sign(np.sum(np.stack(hdv_list), axis=0))
bind = lambda hdv_list: np.multiply.reduce(np.stack(hdv_list))
shift = lambda hdv, k=1: np.roll(hdv, shift=k)
cosine_sim = lambda v1, v2: np.dot(v1, v2) / (np.linalg.norm(v1) * np.linalg.norm(v2))
```

`hdv` generates a new HDV with a given size, and `cosine_sim` measure the similarity of two HDVs using the cosine similarity. The function `bundle` creates a new HDV which is is similar to all those combined, and `bind` creates a new vector that encodes an interaction between HDVs. `shift`-ing a vector performs a cyclic shift, and is used for encoding positional information.
  
### Encoding Proteins
To encode a protein, we use a 3-mer based system: assigning a random HDV to each possible amino acid then binding them together in triplets:

```python
trimer_hdv = bind(shift(first_aa_hdv, 2), shift(second_aa_hdv, 1), third_aa_hdv)
```

using shifting to encode the relative position. To encode a sequence, we `bundle` together all the HDVs of the 3-mer sequences within it. This process is quite slow in `Python`, as we're using a `Python` for-loop to iterate over each string and build the corresponding HDV. Using the `PyO3` crate and `maturin` interface, I rewrote this encoding process in [Rust]() for significantly faster encoding (order of seconds rather than hours!). 

Finally, to associate a set of sequences together that share some semantic meaning (like a protein family) we `bundle` together all the sequences that share that meaning into a class 'prototype' - akin to using the centre of a embedded cluster to represent all the samples within it.

Novel sequences can then be classified by encoding them using the trimer bundling and measuring the `cosine similarity` between them and all the prototypes. The prototype with the highest similarity is the predicted classification, and `top-n` metrics could also be used to measure the performance when many classes are similar.

## Model Performance

Using this method to encode and predict on unseen samples from the most abundant 50 PFam families produces very strong performance, with the confusion matrix for the test 20% of sequences shown below:

![[base_confusion.png|500]]

The very strong diagonal indicates that the vast majority of samples were correctly classified, and the model has a test accuracy of 95.24%, using `scikit-learn`'s `balanced_accuracy` function. Visualising all of the test sequence embeddings in a reduced space generated by `PCA` (from `sklearn.decomposition`) and colouring them by class reveals that similar sequences are clustered together, but the space as a whole is homogenous with lots of overlap between classes. 

![[base_pca 1.png|500]]

Investigating the similarities of the `prototypes` generated for each of the 50 PFam families, we can see that there are clear groupings and similarities. For example, the `DnaJ`  families are grouped together in the bottom right, and a cluster of `transf`-erase related subsequences are clustered together near the middle of the plot. Other clusters are more difficult to interpret without deeper understanding of the names assigned to each family, but it's clear that the HDV space allows clustering of families with similar behaviours. 

![[base_prototypes.png|500]]
## Possible Improvements
### Iterative Re-Training
The original "training" process generating the class `prototypes` involves only a single pass through the dataset, and only uses information from within each class to generate the prototype. The works well due to the natural orthogonality of the HDV space, assigning each class separate regions by default. However, these models still misclassifies samples, and as such previous research has identified ways to use the classification behaviour on the training data to update the class prototypes.

To do this, "inference" is carried out with the `prototypes` on all the training data and mis-classifications are identified. For each of these, the embedding of the misclassified sequence is: added to the true class `prototype`, and subtracted from the class `prototype` it was mistakenly assigned to. The addition/subtraction is scaled by a fractional factor analogous to the "learning rate" of the process, ensuring that a position switch in the prototype vector (between 1 and -1) will only happen if that position is changed by multiple corrective additions/subtractions. Once all misclassified sample have been used, the `prototypes` are run through the sign function to return them to ±1 in each position. 

This whole process of retraining is carried out for a number of "epochs" until the compute/time budget is exhausted or the performance of the model saturates. We trialled this iterative retraining with a range of learning rates, finding the model to be very sensitive to high and low values: high values resulted in extreme model instability as too much of the HDV was modified in each epoch, and too low resulted in no change. We settled on a learning rate of `1e-2`, which resulted in a final test performance of 95.35%, a marginal improvement in performance.

### Positional Embeddings

When working with sequence inputs, there can be some benefits to explicitly including the relative positions of the sequence components into the HDV encoding. In our original model, we did not encode the relative position of each of the trimers, instead bundling them together in a position independent way. Some approaches for encoding position here could include: shifting each trimer by a step relative it's position in the sequence, or binding each trimer to another HDV that is use consistently to represent a relative position along the sequence length. 

In this work, we trialled the second method - generating a list of 100 HDVs who are incrementally modified such that they are most correlated to the HDV immediately adjacent in the list. By assigning each trimer position in the protein sequence to it's nearest 1/100th of the sequence's length, we bind the timer HDV to the matching positional HDV to encode the relative sequence position. Using this scheme, we again saw a model performance increase to 95.35%, suggesting that including the positional embeddings has overcome similar hurdles to the retraining process but not provided a significant boost in performance.  

## Conclusions

HDC provides a fast and robust methodology to carry out classification of complex sequential objects, in a way that requires no padding and very few (if any) training steps. The resulting prototype-based classification process can be very efficient, using similar mathematical operations as most modern deep learning, with the main bottleneck being in encoding new sequences. This can be alleviated by delegating this to custom functions, such as was done here with the `Rust` encoding module. 

The performance of the final "model" in the PFam task was very strong, and saturated in a way that did not significantly improve with additional modelling techniques such as "iterative retraining" or "positional embeddings". It is possible that the performance ceiling was almost immediately met by the baseline model, and these additional techniques would likely have a stronger effect in more complicated datasets where the sequential structure was more useful to the prediction process. 

All the resources and code developed for creating these models can be found [at this GitHub repo](https://github.com/JELAshford/hyperdim-proteins), along with scripts to reproduce the figures included here and more! If you're interested in how this method can be extended, I recommend checking out the `synthetic_aa_with_motif.py` script in which I've been experimenting with interpretability and amino-acid motif detection - I may write this up in the future. Thanks for reading! :) 