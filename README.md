# AutoEncoders

Topics:
- What are autoencoders?:  Autoencoders are neural networks that have the ability to discover low-dimensional representations of high-dimensional data and are able to reconstruct the input from the output. Autoencoders are made up of two pieces of the neural network, an encoder and a decoder. The encoder reduces the dimensionality of a high dimensional dataset to a low dimensional one whereas a decoder essentially expands the low-dimensional data to high-dimensional data. The goal of such a process is to try to reconstruct the original input. If the neural network is good, then there is a good chance of reconstructing the original input from the encoded data. 

# How autoencoding works
![12](https://github.com/andysingal/AutoEncoders/blob/main/images/Screenshot%202023-06-06%20at%2011.22.33%20AM.png)

# Designing autoencoding neural networks
PCA is a very popular method for dimensionality reduction and visualization, and any situation where you would use PCA is one where you may want to instead use an autoencoding network. An autoencoding network is the same idea, but we will make the encoder and decoder larger networks with more layers so they can learn more powerful and complex encoders and decoders. Since we have seen that PCA is an autoencoder, an autoencoding network may be a more accurate choice by being able to learn more complex functions.

Autoencoding networks are also useful for outlier detection. You want to detect outliers so that you can have them manually reviewed, because an outlier is unlikely to be handled well by a model. An autoencoder can detect outliers by looking at how well it reconstructed the input. If you can reconstruct the input well, the data probably looks like normal data you’ve seen. If you cannot successfully reconstruct the input, it is probably unusual and thus an outlier.

# Robustness to noise
To help us answer the question about which autoencoder is better, D′ = 2 or D′ = 2 ⋅ D, we will add some noise to our data. Why? One intuition we can use is that if a representation is good, it should be robust. Imagine if the clean data we have been using was like a road, and our model was the car. If the road is pristine and smooth, the car will handle well. But what if there are potholes and cracks (read, noise) in the road? A good car should still be able to drive successfully. Similarly, if we have noisy data, an ideal model will still perform well.

There are many different ways we could make our data noisy. One of the easiest is to add noise from a normal distribution. We denote the normal distribution as N(μ,σ), where μ is the mean value returned and σ is the standard deviation. If s is a value sampled from the normal distribution, we denote that as s ∼ N(μ,σ).

##Denoising autoencoders
It is not easy to balance having D′ small enough to be robust but large enough to do well at reconstruction. But there is a trick we can play that will allow us to have large D' > D and learn a robust model. The trick is to create what is called a denoising autoencoder. A denoising autoencoder adds noise to the encoder’s input while still expecting the decoder to produce a clean image. So our math goes from ℓ(f(x),x) to ℓ(f(),x. If we do this, there is no naive solution of just copying the input, because we perturb it before giving it to the network. The network must learn how to remove the noise, or denoise, the input and thus allows us to use D′ > D while still obtaining robust representations.

# Understanding convolutional autoencoders
In the previous section, we learned about autoencoders and implemented them in PyTorch. While we have implemented them, one convenience that we had through the dataset was that each image has only 1 channel (each image was represented as a black and white image) and the images are relatively small (28 x 28). Hence the network flattened the input and was able to train on 784 (28*28) input values to predict 784 output values. However, in reality, we will encounter images that have 3 channels and are much bigger than a 28 x 28 image.

In this section, we will learn about implementing a convolutional autoencoder that is able to work on multi-dimensional input images. However, for the purpose of comparison with vanilla autoencoders, we will work on the same MNIST dataset that we worked on in the previous section, but modify the network in such a way that we now build a convolutional autoencoder and not a vanilla autoencoder.
![img](https://github.com/andysingal/AutoEncoders/blob/main/images/Screenshot%202023-06-07%20at%209.00.48%20AM.png)
From the preceding image, we can see that the input image is represented as a block in the bottleneck layer that is used to reconstruct the image. The image goes through multiple convolutions to fetch the bottleneck representation (which is the Bottleneck layer that is obtained by passing through Encoder) and the bottleneck representation is up-scaled to fetch the original image (the original image is reconstructed by passing through the decoder).

# Understanding variational autoencoders
So far, we have seen a scenario where we can group similar images into clusters. Furthermore, we have learned that when we take embeddings of images that fall in a given cluster, we can re-construct (decode) them. However, what if an embedding (a latent vector) falls in between two clusters? There is no guarantee that we would generate realistic images. Variational autoencoders come in handy in such a scenario.

Before we dive into building a variational autoencoder, let's explore the limitations of generating images from embeddings that do not fall into a cluster (or in the middle of different clusters).

We can see from the preceding output that when we plot images that were generated from the mean and the noise-added standard deviation of columns of known vectors, we got images that are less clear than before. This is a realistic scenario, as we would not know beforehand about the range of embedding vectors that would generate realistic pictures.

Variational Autoencoders (VAE) help us resolve this problem by generating vectors that have a mean of 0 and a standard deviation of 1, thereby ensuring that we generate images that have a mean of 0 and a standard deviation of 1.

In essence, in VAE, we are specifying that the bottleneck layer should follow a certain distribution. In the next sections, we will learn about the strategy we adopt with VAE, and we will also learn about KL divergence loss, which helps us fetch bottleneck features that follow a certain distribution.

##Working of VAE
 We enable that with a VAE by adopting the following strategy:

- The output of the encoder is two vectors for each image:
- One vector represents the mean.
- The other represents the standard deviation.
- From these two vectors, we fetch a modified vector that is the sum of the mean and standard deviation (which is multiplied by a random small number). The modified vector will be of the same number of dimensions as each vector.
- The modified vector obtained in the previous step is passed as input to the decoder to fetch the image.
- The loss value that we optimize for is a combination of the mean squared error and the KL divergence loss:
- KL divergence loss measures the deviation of the distribution of the mean vector and the standard deviation vector from 0 and 1, respectively.
- Mean squared loss is the optimization we use to re-construct (decode) an image.

