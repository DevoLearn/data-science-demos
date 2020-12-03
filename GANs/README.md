
# Generative Adversarial Networks
A GAN is a class of machine learning frameworks designed by Ian Goodfellow and his colleagues. The goal of this framework is to generate realistic synthetic samples similar to existing data. This tutorial explains the intuition and math behind the training of Generative Adversarial Networks. Feel free to check out the implementation in the colab notebook linked below.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Mainakdeb/digit-GAN/blob/main/digit-dcgan.ipynb)
[![Binder](https://camo.githubusercontent.com/bfeb5472ee3df9b7c63ea3b260dc0c679be90b97/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f72656e6465722d6e627669657765722d6f72616e67652e7376673f636f6c6f72423d66333736323626636f6c6f72413d346434643464)](https://nbviewer.jupyter.org/github/Mainakdeb/digit-GAN/blob/main/digit-dcgan.ipynb)

<img src="https://github.com/Mainakdeb/digit-GAN/blob/main/images_and_gifs/digit-demo.gif" width="510">

The gif above showcases some generated samples from the PyTorch based GAN in the notebook.

## How does it work?
Two neural networks, the Generator(G) and the Discriminator(D) are pitted against one another and trained simultaneously. But what are they aiming to accomplish?
  * The **Discriminator** aims to distinguish between real and generated samples. 
  * The **Generator** aims to produce real-looking samples/images to fool the Discriminator.

In simple terms, the Generator is like an art-forger who is trying to sharpen its skills, and the Discriminator is like an art-expert which the generator wants to fool.

## The Math
The 2 networks are trained in a minimax game formulation:

<img src="https://github.com/Mainakdeb/digit-GAN/blob/main/images_and_gifs/minmax.png" width="510">

1. x represents a real sample.
2. z is random vector in the latent space, or lets just say random noise. 
3. D(x) is the discriminator output for a real sample x.
4. G(z) is the generated output from the generator net with noise z given as input.
5. D(G(z)) is the discriminator output for generated fake data G(z).
5. E represents the expected value or the average.


* The **Discriminator** wants to make sure D(x) is close to 1 and D(G(x)) is close to 0. It attempts to maximise the following objective.
<img src="https://github.com/Mainakdeb/digit-GAN/blob/main/images_and_gifs/discriminator_objective.png" width="510">

* The **Generator** wants to make sure D(G(z)) is close to 1 i.e. fool the discriminator into thinking  generated G(z) is real. It attempts to minimise the following objective.
<img src="https://github.com/Mainakdeb/digit-GAN/blob/main/images_and_gifs/generator_objective_weak_grad.png" width="310">

But in practice, optimizing this generator objective does not work well because it has vanishing gradients early on. So instead, people use a different objective function as shown below:

<img src="https://github.com/Mainakdeb/digit-GAN/blob/main/images_and_gifs/generator_objective_practical.png" width="280">

This(green line) has a higher gradient signal for bad samples i.e. towards the left, hence the model learns more in region of bad samples. The purple line is almost flat (weak gradients) towards the left, which is not suitable for training.

<img src="https://github.com/Mainakdeb/digit-GAN/blob/main/images_and_gifs/weak_grad_vs_strong_grad.png" width="490">


## If all goes well:
The Generator generates samples indistinguishable from real samples. And the discriminator is forced to guess (with a probability of ½).

Most of what I've learned are from the following sources:
1. [This lecture](https://www.youtube.com/watch?v=5WoItGTWV54&ab_channel=StanfordUniversitySchoolofEngineering) by Serena Yeung on Generative nets.
2. The [DCGAN paper](https://arxiv.org/abs/1511.06434) by Alec Radford, Luke Metz, Soumith Chintala.
3. [Generative Adversarial Networks](https://arxiv.org/abs/1406.2661) by Ian Goodfellow et al.
