---
layout:     post
title:      "Graphical models with neural networks for structured representations and fast inference"
subtitle:   "Paper Summary"
date:       2017-12-07 21:30:00
author:     "Siddhartha Saxena"
header-img: "img/posts/summ.jpg"
comments: true
tags: [ Summaries, ML ]
--- 

# Composing graphical models with neural networks for structured representations and fast inference

## Paper Details
* Authors MJ Johnson, David Duvenaud, A Wiltschko, Sandeep Datta, Ryan P. Adams
* Link: [Arxiv](https://arxiv.org/pdf/1603.06277.pdf)
* Tags: Graphical Models, VAE, probabilistic models
* Year: 2016
* Conference: NIPS 2017
* Implementation: [Official in TensorFlow](https://github.com/mattjj/svae)

## Summary
* In the first half of the paper, the authors argue that neural networks and graphical models have complementary strengths. The latter makes rigid assumptions about the data opposed to the flexibility of neural networks while giving structured representations which is generally challenging to obtain for neural networks. They provide examples where this can be seen as bellow.
    * Gaussian Mixture Models + Neural Nets: (Fig 1) Here they show a simple neural network can not encode the clustered structure of the data, while GMM by itself doesn't leed to very good partitions
    *  VAE + Linear Dynamic System: Here, by making the output at time step n, depend on output at time step n-1. It can encode time dependence in the predictions that is important to model videos.
    * VAE + Switching LDS: Going a step further. Latent LDS can be combined with GMM to create a model that has another latent discrete layer. Hence making it possible for the model to evolve.

* The problem that the authors address is that it is possible to leverage the flexibility of neural networks with PGMs by outputting PGM potentials instead of defining the whole variational distribution ( like the mean and variance in vanilla VAE) using a recognition network ( i.e., the encoder in VAE ).
* They derive this fact by writing down the variational lower bound for a VAE in Eq 3 and then parametrize the potential with a recognition network in the next step ( end of page 5). 
* Then they introduce the SVAE lower bound and state that it lower bounds the above objective, proved in the appendix (  stated in proposition 4.1 )
* The authors note that L_{SVAE} is similar to the VAE lower bound. Hence it can be maximized similar to a VAE to get the estimate of its parameters.     
