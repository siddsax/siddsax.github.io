---
anchor: docclustering
title: Interactive Clustering
internal_title: User feedback Interactive Clustering   
image: img/portfolio/interClust.png
image_rec: img/portfolio/yodlee_rec.jpg
report: "https://cse.iitk.ac.in/users/siddsax/Reports/MLT_report.pdf"
code: "https://github.com/siddsax/Interactive_Bayesian_Clustering"  

intro: This project was part of course CS771, Machine Learning Techniquues which I did in the fall semester of 2016. A critical problem with clustering in the unsupervised domain is that one can have varied types of clustering based on input parameters and learning rates ( in case of a deterministic model ). Hence it leads to the idea of using user feedback, so that one can get the clustering of their choice. 

problem: In order to use the user feedback we used a fully probabilistic Gaussian Mixture Model with a prior over the parameters of the model. The prior was formulated such that it downweightes the probability of having model parameters that give rise to rejected clusters and upweight the probabilities for accepted clusters. We also implemented fast coordiate descent in place of standard gradient descent for optimizing in the M step as the challenge was to work in an interractive setting, as the user has to wait in between rejection cycles.

results: We were able show that the technique worked for 2D data points visually. After rejection of a given cluster and acceptance of another one, the model was able to still consider the accepted cluster but recluster the data points of the rejected ones, which showed the utility of this model on the problem.  

period: September 2014
category: Project
---
