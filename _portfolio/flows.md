---
anchor: flows
title: Normalizing Flows
internal_title: Variation Inference via Transformations on Distributions  
image: img/portfolio/VAENF.png
image_rec: img/portfolio/yodlee_rec.jpg
report: "https://arxiv.org/abs/1707.02510"
code: "https://github.com/jaivardhankapoor/vaenf"

intro: This project was a part, of course, CS698S, Bayesian Machine Learning in the spring semester of 2017. The motivation for this was that Variational Inference is one of the most central things in Bayesian Machine Learning with extensive mathematics. Even after its advantage of being fast, it takes a lot of assumptions on the posterior distribution. We talked this problem in this work to get more diverse and complex distributions. 

problem: Variational inference methods often focus on the problem of efficient model optimization, with little emphasis on the choice of the approximating posterior. In this paper, we review and implement the various methods that enable us to develop a rich family of approximating posteriors. We show that one particular method employing transformations on distributions results in developing very rich and complex posterior approximation. We analyze its performance on the MNIST dataset by implementing with a Variational Autoencoder and demonstrate its effectiveness in learning better posterior distributions.

results: We achieved a multimodal latent distribution for a variational auto-encoder as opposed to the unimodal normal distribution. We used the MNIST handwriting dataset which is a multi-class dataset with 10 classes and observed distinct modes for different archetypes of digits, proving the hypothesis as well as an increase in the diversity of the distribution with the increase in the number of transformation layers i.e. increasing its complexity.  

period: September 2014
category: Project
---
