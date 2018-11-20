---
layout:     post
title:      "Visualizing MSD-Net with Grad-CAM Part-1"
subtitle:   "Visualizing any model with Grad-CAM"
date:       2018-08-18 21:30:00
author:     "Siddhartha Saxena"
header-img: "img/posts/moo.jpg"
comments: true
tags: [ Technical, AI ]
--- 

# Visualizing MSD-Net with Grad-CAM Part-1


**tl;dr : This part explains how to visualize models with Grad-CAM for *any* model**

This year at ICLR 2018, among several interesting papers, there was one paper named **Multi-Scale Dense Networks for Resource Efficient Image Classification**, albeit published on archive in early 2017. It for the first time discussed new perspectives to resource efficient classification. Overall their aim in-short was ***use more prediction time/computation for tough samples, less for easier ones***.  

Crudely, to achieve this they added multiple classifiers after layers of a deep neural network such that easier samples can be predicted with classifiers after less number of layers and vice versa. This raises an important question which this article is about. **What do the tough sample classifiers look at opposed to easy sample classifiers making them better?**

Now in this blog I'll not go into the details of MSD-Net ( you can find it [here](https://github.com/siddsax/PaperSum/blob/master/Summaries/MSDNet.md) in my summaries!) This question is important as it gives an insight into what makes an image classifier better. The MSD-Net consists of 10 classifiers, each classifier after one block. So the classifier after 10 blocks is the most powerful and after 1st one is weakest. 

Before we delve into the above question, we'll first side-track into the second aspect of this blog which will fill up the part-1. How to effectively visualize where the network is paying attention. Enter Grad-CAM ([link](https://arxiv.org/abs/1610.02391)). Grad-CAM is an effective method that extends the previously proposed Class Activation Map-layer ([link](http://cnnlocalization.csail.mit.edu/)) which can localize the model attention **without the need to alter or even retrain!** the network.  

In short Grad-CAM finds the weighted summation of the last layer's activation maps. The weights being in this case the average of the gradients as follows.   
	
![](http://siddharthasaxena.com/blogImgs/gc.png)

These are the same weights that in the CAM paper connected the penultimate layer (CAM-layer) with the outputs. Hence this in effect is a generalization of that work. This weighted sum is then resized to the size of the image and can show where the model is paying attention.

A more detailed explaination of Grad-CAM can be found from the author himself [here](https://ramprs.github.io/2017/01/21/Grad-CAM-Making-Off-the-Shelf-Deep-Models-Transparent-through-Visual-Explanations.html)  

Below through the Grad-CAM code I used for MSD-Net, I'll illustrate how to use Grad-CAM with any network. the code for MSD-Net can be found at my [github](https://github.com/siddsax/VisualizeMSDNet) repo. 

1) Alter the model, to output the features of the layer just before the classifier. In the following example,  the network ```self.subnets[block_num+self.num_blocks]``` is the classifier after ```block_num``` numbered block, hence we return its input along with model prediction. 

```
class_output = self.subnets[block_num+self.num_blocks](block_output[-1])
feat = block_output[-1]
outputs[block_num] = class_output
probab[block_num] = torch.nn.functional.softmax(class_output, dim=-1)
feats[block_num] = feat
return outputs, feats
```
2) Second alteration needed is to add gradient hooks to save the gradients of the features wrt output. This can be done with the ```register_backward_hook``` attribute of torch's nn.sequential layers. An example is as follows.

```python
def __init__()
	self.subnets[0].MSD_layer_4.subnets[-1].register_backward_hook(self.hookFunc)
def  hookFunc(self, module, gradInput, gradOutput):
	self.gradients.append(gradOutput[0])
```

Here ```self.subnets[0].MSD_layer_4.subnets[-1]``` is the final feature layer of the model ( as this is from MSD-Net, there are multiple classifiers. This is one of them). One can check this by printing the model itself. Remember in the last case we got away simpl with the features though here we need to work with the layer so some digging in the model is required. The hookFunc is basically a helper that appends the output gradient, i.e. the gradient of the layer's output with the next layer, which is the classifier layer here in this case. 

3. Integrate changes to ```__call__``` function of GradCam according to how the model predicts data. Finally the code below will generate the heat map in a file ending with name '_cat' in folder named diags 

```python
grad_cam = GradCam(model  = model)
mask = grad_cam(target_var.cpu().data.numpy()[0], input_var, 0, args.maxC-1)

img =  input[0].cpu().data.numpy().transpose(1,2,0)
img = cv2.resize(img, (512, 512))
show_cam_on_image(img, mask, name)
```
This is how any model can be visualized wit Grad-CAM. I end this part of the article here, in the second part I'll summarize the results on MSD-Net in the next part!
