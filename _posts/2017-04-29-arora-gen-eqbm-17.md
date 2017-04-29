---
title: '[Notes]: Generalization and Equilibrium in GANs'
date: 2017-04-29
permalink: /posts/2017/04/arora-gen-eqbm-17/
tags:
  - Deep Learning
  - Theory
  - Adversarial
  - GAN
---

# [Notes]: Generalization and Equilibrium in GANs

paper: https://arxiv.org/abs/1703.00573

A [number of variations of GANs](https://github.com/hindupuravinash/the-gan-zoo)  been proposed in past few years. There are still several challenges associated with them such as training without collapsing, generalization etc. 

## GAN Introduction

A general GAN framework consists of two parts.

Generator $$G$$ which tries to sample from a distribution $$\mathbb{R}^x$$ when a standard gaussian is given as an input. 

Discriminator $$D$$ tries to distinguish if the generated sample from generator is from real data distribution or not. 

Both of these are represented using a neural net.

The overall aim of GAN is to make generator outputs samples as near as real data distribution while discriminator tries to be a strong distinguisher of real or fake samples.   

During training,  Discriminator parameters are updated first using backprop and in next step, assuming a fixed discriminator, Generator parameters are updated. Thus solving a min-max game between 2 agents. 



## GAN Objectives

[Goodfellow et al.](https://arxiv.org/abs/1406.2661) proposed objective for a generic GAN

$$
min_G max_D \mathbb{E}_{x \sim\mathbb{P_{real} }}[\log (D(x))] +   \mathbb{E}_{z \sim\mathbb{P_{z} }}[\log (1 -D(G(z)))]
$$

However, optimally if we have infinite data the discriminator should approach to naive bayes for generic GAN. 

$$
D_{opt}(x) = \frac{P_{real}(x)}{P_{real}(x) + P_{G}(x)}
$$

In case of generic GAN, overall objective convergence is similar to minimizing [Jensen-Shannon(JS) divergence](https://en.wikipedia.org/wiki/Jensen%E2%80%93Shannon_divergence)  which, for 2 distributions($$\mu$$ and $$\nu$$), is defined as 

$$
d_{JS}(\mu, \nu) = \frac{1}{2}(KL(\mu || \frac{\mu+\nu}{2}) + KL(\nu || \frac{\mu + \nu}{2}))
$$

In [Wasserstein Gan by Arjovsky et al. 2017](https://arxiv.org/abs/1701.07875) , Earth-mover distance as objective is proposed , with $\tilde{x}=G(z)$  

$$
min_G max_D \mathbb{E}_{x\sim\mathbb{P}_{real} }[D(x)] -  \mathbb{E}_{\tilde{x}\sim \mathbb{P}_{\theta}}[D(\tilde{x})]
$$

 ## Limitations 

There are severall assumption for the above objectives 

1. Neural Net choosen for $$D$$ is large enough to approximate to optimal discriminator. 
2. Enough data samples to estimate expectations in objective. 

However, in practice, training of a GAN occurs in batches of data samples and hence it is argued that the behavior of $$D$$ is different from what is assumed for the objective. 

Key proposition by [Arora et al.](https://arxiv.org/abs/1703.00573) : " ***a bounded capacity discriminator is unable to force the generator to produce a distribution with very high diversity***"

This key proposition is motivated from previous studies on psuedorandomness. 



## Generalization

A generalized concept over all different distance metrics in GANs is presented in the [paper](https://arxiv.org/abs/1703.00573). 

The generator, during overfiitting, memorizes all of the training examples and if the number of samples are much larger than the dimension of a sample then it is better to memorize all data rather than learning real distribution. 

The generalization argument :  after training GAN, the empirical distance in assumed metric (JS divergence, Earth Mover distance or others)  is well approximated. However, even if the GAN objective is minimized this may not result into generalized distribution. 

Assume distributions for real and fake ( $$\mu$$ and $$\nu$$ respectively) as uniform Gaussian distribution. While training on a sufficiently large dataset, empirical distribution. are sampled from data(batch) given as $$\tilde{\mu}$$ and $$\tilde{\nu}$$ respectively. If the size of batch is much less than over all dimension of data, then JS divergence between empirical distribution is approximately $$\log 2$$ and Earth-Mover(Wasserstein) distance is $$\geq 1.1$$ . While the initial assumptions for each of them to be 0 after training. The proof for this is presented in the paper.  

Hence, interpreting that a GAN objective is minimizing the distance is wrong because even if the generator finds distribution of samples and the discriminator is optimal, distance between the empirical distributions (per batch sampl dist.) is large. This results in generator skipping this distribution and minimizing distance between empirical distributions. 

In summary, a minimized $$G$4 has $$\mathbb{P}_G \ne \mathbb{P}_{real} $$ , but it does not know.   



#### Can there be a correct way to represent the Distance?

In the [paper](https://arxiv.org/abs/1703.00573), a new distance metric is proposed name as Neural Network divergence between NNs $$\mu$$ and $$\nu$$,  given as 

$$
d_{NN}(\mu|| \nu) = \max_{\nu \in \mathcal{V}} \mathbb{E}_{x\sim\mu}[f(D_{\nu}(x))] + \mathbb{E}_{x\sim\nu}[f(D_{\nu}(1-x))]
$$

where $$D_{\nu} : \nu \in \mathcal{V}$$ is a class of discriminators and $$f$$ is measuring function.

This proposed NN divergence is psuedosymmetric because even if $$d_{NN}(\mu, \nu) = 0$$  then the neural nets may not be same, however it satisfies triangular inequality for three NNs ($$\mu_1, \mu_2, \mu_3$$) as $$d_{NN}(\mu_1, \mu_3) \le d_{NN}(\mu_1, \mu_2) + d_{NN}(\mu_2, \mu_3) $$ and hence symmetric. 

The [paper](https://arxiv.org/abs/1703.00573) also shows that for the batch size $m$  and constant $C$, if 

$$
m \ge \frac{C n \Delta^2 \log (L L_f n / \epsilon)}{\epsilon^2}
$$

then it is higly propable that NN distance between empirical distribution and real distribution is bounded by $\epsilon$. 

$$
| d_{NN}(\hat\mu|| \hat\nu) - d_{NN}(\mu|| \nu) | \le \epsilon
$$

This is true for all training steps given correct assumptions. 

#### Limitations of using NN distance

In case of nn distance, even if the distance is small , the distributions  may not be very close. When the samples $$m \ge n/\epsilon^2$$  then with high probability $$d_{NN}(\mu ,\hat\mu) \le \epsilon$$ on the other hand if $$m$$ is small, then the discriminator is incapable of detecting lack of diversity in the sample.

### MIX$$+$$ GANS

[Arora et al.](https://arxiv.org/abs/1703.00573)  provides proof that there exists an equilibrium with a finite mixture of generators and discriminators and using them leads to more stable training. It is impractical to use large mixture, however the best solution should be an infinite mixture of generators and discriminators. This comes from the fact that an infinite mixture of gaussians are able to represent any distribution.  

Using a mixture of $$T$$ generators and $$T$$ discriminators, which in practice  $$T \le 5$$ with same network but different parameters, is proposed in the paper. The weights are obtained by soft-max averaging across the mixture for each layer. This is different from boosting where averaging is done across sample. 

From game theory perspective the pay-off $$F$$ between generator and discriminator is given by 

$$
F(u, v) = \mathbb{E}_{x \sim\mathbb{P_{real} }}[f(D_v(x))] +   \mathbb{E}_{z \sim\mathbb{P_{z} }}[f(1 -D_v(G_u(z)))]
$$

So the objective for MIX$$+$$ GANS is given by 

$$
\min_{\{u_i\}, \{\alpha_{u_i} \}} \max_{\{v_j\}, \{\alpha_{v_j} \}} \Sigma_{i,j \in T} w_{u_i} w_{v_j} F(u_i, v_j)
$$

To regularize, an entropy term is added to training objective. 
