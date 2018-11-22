
# Visualizing MSD-Net with Grad-CAM Part-2

**TL; dr: This part explains what makes one classifier better than other in MSD-Net**

Having laid the foundations in Part 1 ([link](http://siddharthasaxena.com/blog/visualizing-MSD-Net-with-Grad-CAM-Part-1)), let's get straight into the meat of this here. 

We use the Cifar-10 dataset for experimentation which is an image classification dataset made up of 60K color images with a resolution of 32x32. Note that the low resolution can even make it tough for us, humans to classify these images. First, for reference, Fig 1 plots the global accuracies of each classifier on the CIFAR-10 dataset.

![](http://siddharthasaxena.com/blogImgs/accMSDnet.png)   
-------------------------------------------------- **Fig 1** ----------------------------------------------------

The performance of classifiers generally improves as the number of blocks increase. This makes sense as an increase in the number of blocks, also means more parameters. Although, the best classifier on average is **B6** (after 6 blocks). This is possibly due to the fact that as the number of blocks increase, overfitting becomes more prominent.  

Now let's see some images of dogs ---- visualized with Grad-CAM!

![](http://siddharthasaxena.com/blogImgs/dogs.jpg)
 -------------------------------------------------- **Fig 2** ----------------------------------------------------
In Fig 2, we have several images of dogs that are correctly/incorrectly classified by different MSD-Net classifiers. Some of these samples are more difficult than the others. This can be estimated by the number of classifiers that correctly classify the given image, mentioned in the brackets in the first column. In the paper, the authors instead learn to predict a confidence value corresponding to each prediction which is a more principled way to do it.

The key points to note here as follows:

* Higher the number of parameters, higher is the attention span of the model. This can be seen from Fig 2 where the heatmaps spread over a larger region in case of blocks 5 and 10 opposed to 1 and 3. Clearly, this is one of the contributing factors to misclassification by B1 and B3
* The attention spans of B10 and B5 are more similar to each other as compared to those of B1 and B3 in spite of the former having more blocks between them. This convergence of visual spans corroborates with the convergence of accuracies in Fig 1. Giving us further proof that model performance is linked to the Grad-CAM visualizations.
* Each classifier has its own set of characteristics as it classifies objects. For example, B10 attends over the whole while B5 mostly tries to find legs. This also hints at which classifier can be a better judge for a given class. If a classifier attends to more discriminative features with respect to that object, then it is likely that it will outperform a model that does not.
* To explain it further, consider the class 'truck'. The attention span of B10 over the back of the truck ( albeit a large portion ) while the classifier B5 looks at small parts of the front and back of the truck. The later set of features are much more discriminative, leading to the superior performance of B5 over B10, summarized in Table 1.

| B1 | B2 | B3 | B4 | B5 | B6 | B7 | B8 | B9 | B10 |
| ----|----|----|----|----|----|----|----|----|----|
|0.939| 0.941| 0.960| 0.956| 0.966| 0.966| 0.966| 0.962| 0.958| 0.958|

-------------------------------------- ** Table 1**----------------------------------------------------------

![](http://siddharthasaxena.com/blogImgs/truck.png)
-------------------------------------------------- **Fig 3** ----------------------------------------------------

Although it is easy to judge the performance of these models otherwise using their prediction scores, for unsupervised models such as GANs and VAEs, these visualization techniques can potentially lead to automated evaluation techniques which remain the holy grail.

With this note, I'll conclude the 2-part series. Hope it would have helped you in getting an idea of qualitatively understanding the performance of models, and through the previous post, how to localize model attention.
