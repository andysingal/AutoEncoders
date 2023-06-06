# AutoEncoders

Topics:
- What are autoencoders?:  Autoencoders are neural networks that have the ability to discover low-dimensional representations of high-dimensional data and are able to reconstruct the input from the output. Autoencoders are made up of two pieces of the neural network, an encoder and a decoder. The encoder reduces the dimensionality of a high dimensional dataset to a low dimensional one whereas a decoder essentially expands the low-dimensional data to high-dimensional data. The goal of such a process is to try to reconstruct the original input. If the neural network is good, then there is a good chance of reconstructing the original input from the encoded data. 

# How autoencoding works
![12](https://github.com/andysingal/AutoEncoders/blob/main/images/Screenshot%202023-06-06%20at%2011.22.33%20AM.png)

# Designing autoencoding neural networks
PCA is a very popular method for dimensionality reduction and visualization, and any situation where you would use PCA is one where you may want to instead use an autoencoding network. An autoencoding network is the same idea, but we will make the encoder and decoder larger networks with more layers so they can learn more powerful and complex encoders and decoders. Since we have seen that PCA is an autoencoder, an autoencoding network may be a more accurate choice by being able to learn more complex functions.

Autoencoding networks are also useful for outlier detection. You want to detect outliers so that you can have them manually reviewed, because an outlier is unlikely to be handled well by a model. An autoencoder can detect outliers by looking at how well it reconstructed the input. If you can reconstruct the input well, the data probably looks like normal data you’ve seen. If you cannot successfully reconstruct the input, it is probably unusual and thus an outlier.
