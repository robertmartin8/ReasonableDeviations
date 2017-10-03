---
layout: default
title: Deep Sparse Rectifier NNs
---

## Deep Sparse Rectifier Neural Networks

**Glorot, Bordes, Bengio**

- The **rectifier function** $\max(0,x)$ is both a useful model for neuron activation in neuroscience and an efficient activation function in neural networks. 
- Biological neurons can have activations that are **asymmetric** ($1 \rightarrow -1$), **symmetric** ($1 \rightarrow 1$), or **one-sided** ($1 \rightarrow 0$)
- Neurons encode information in a **sparse** manner: only 1-4% are active at once. This is a tradeoff between expressiveness and low energy use. Neural nets without $\ell_1$ regularisation do not have this property. 
- Biological neurons use very different activations to NNs:
    - their firing rate is zero for all current less than zero, then increases sub-linearly. 
    - artificial neurons use sigmoid or tanh activations, which are both asymmetrical about zero (not present in biology)
    - tanh is preferred to sigmoid because its steady state is zero. 

### Consequences of sparsity

- **Information disentangling** – the networks will be robust to small input changes
- Efficient variable-size representation – inputs can be in a variable-sized data structure. 
- More likely to be linearly separable
- Excess sparsity may reduce predictive capability. 

### Rectifier neurons

- Because real neurons rarely reach their saturation (where increases in current no longer affect firing rate), they can be well approximated with the rectifier function.
- A rectifier automatically produces sparsity, 50% of activations will be initialised to zero. 
- Much easier to compute than tanh or sigmoid.
- If one is worried about differentiability, the **softplus function** $f(x) = \ln(1+e^x)$ can be used. However, experiments suggest that the hard zeroes are good for NN performance.
- Rectifier nets need more hidden units to represent any antisymmetry in data. 


### Experimental results

- For image classification, sparsity does not hurt performance until 85% of neurons are zeroes.
- Rectifiers outperform softplus activations
- No improvement using pretraining autoencoders – easier to use. 
- Rectifiers work very well with supervised and semi-supervised problems, but in the latter case pretraining *is* needed.
- Very strong performance on text sentiment analysis: lower RMSE than tanh at 50% sparsity. 