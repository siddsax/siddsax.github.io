---
layout:     post
title:      "Working on servers"
subtitle:   "From the perspective of a Deep Learning Engineer"
date:       2017-06-17 21:30:00
author:     "Siddhartha Saxena"
header-img: "img/posts/moo.jpg"
comments: true
tags: [ Tech ]
--- 

Working on servers is a part-and-parcel these days with resources like GPUs being essential to Deep learning, it is essential that one can work on servers, academeic or AWS. But starting to work on them can be daunting at first and does involve a learning curve that can slow down your progress at first. So here are some tricks that I have found useful over my experience with them.

* Ssh
* GUI-SFTP
* Nvidia-smi
* Htop
* Install ZSH
* Packages
* Visualizing Plots


First thing first
## How to access a server?
**ssh protocol** to the rescue!
Make sure you have a working internet ( and vpn if required ) and then type in the following command.
```bash
ssh username@server_address # it will first ask if you trust the server and the password for your username
```
An example for my IITK server gpu1.cse.iitk.ac.in it will be 
```bash
ssh siddsax@gpu1.cse.iitk.ac.in
```
The Steps described above are summarized below
![Steps involved as above](http://siddsax.github.io/blogImgs/ssh.png )

A side TIP, install sshpass from [here](https://www.tecmint.com/sshpass-non-interactive-ssh-login-shell-script-ssh-password/). This reduces the need to enter the password for the server again and again which becomes tremendously annoying after some time.  

## GUI on server!
This one is particularly helpful if you have to look at images while training a model or don't want to code on Vim. Using sftp client on **ubuntu** on can also mount the remote drive and access in a similar way to local drives. The steps here are for ubuntu 18.04 but overall its similar for all except location of buttons.

1.  Open nautilus/files on ubuntu and navigate to **Open Locations** on the left pane.
![3](http://siddsax.github.io/blogImgs/sftp.png)

2.  After that write down username and address in the format ```sftp://username@server_address/userHome``` Here the *userHome* is  basically the home directory location of the user. If its not added then the root directory will be opened up instead.
![3](http://siddsax.github.io/blogImgs/addrs.png)

Now you can use any gui based editor of your choice ( psss! MS Code is the best :) )
## Check for resources!

GPUs are a crucial part for training deep learning models, hence the are always in demand on large clusters shared by many people. This makes it important to know if the server is indeed available for use or not. This can simply be done with the ```nvidia-smi``` or small tip ```watch -n .1 nvidia-smi``` to get a live status of resources.  Note: if you don't have nvidia-smi then you have either an old gpu or the GPU drivers are not installed. 


![3](http://siddsax.github.io/blogImgs/smi.png)
Here in this window there are few things of importance. The lower half shows the processes that are currently running with the first column being the GPU numbers. Often times as in this case a single process can span multiple GPUs. The last column in this part shows the amount of memory that they are using.

The first half shows the global status of the GPUs on this server. The memory usage row is the key column which shows how much memory is being used up on that gpu. So in this case if your process is small it can still be run on GPU #2 (indexing from 0). But another important parameter is the GPU usage which fluctuates but if is higher than 50 at any point of time will mean that running another process on that GPU will slow down both of them as is in the case of GPU#2 here. 

Another important aspect is the amount of recourses on the **CPU** which comes into play when you are running models other than those that can run on GPUs such as SVMs. Here the equavalent tool is **htop** which might or might not be installed by default. In case of the latter, one can also use **top** which is slightly less cool. This also has a similar format so I'll not go into the details.

## Install ZSH

If it needs to mentioned from [here](https://stackoverflow.com/a/15293565)

## Packages
Packages can be one of the most hassle free things or the most annoying things on a server depeneding on who is the admin or what are your needs. Most of the packages can be installed using ```pip install --user``` in a non-sudo environment but the more preferable way is to use [Anaconda](https://conda.io/docs/user-guide/install/linux.html). This is because anaconda makes it possible to have different versions of libraries on the same computer using environments. Hence its possilble to run one code that depends on PyTorch 0.3.0 while developing on PyTorch 0.4.1 

## Visualizations
Visualizations such as loss vs iterations, or generated data are crucial metrics while training a model. The easiest way to achieve this is via mathplotlib (maybe not the best though) directly in code but several times servers have a restriction on Xserver preventing the generation of GUIs. Another alternative similar to this is to save it to a figure but that can be problematic at times. 

Here I'll go over how to use Facebook's Visdom or Google's Tensorboard if on server. Both of these are exceptional tools for visualizing without any time lags that might be there in previous ones due to the time needed to write files on server.

First [here](https://github.com/siddsax/graphers) you can find some code to run Facebook's Visdom module. The steps are as follows

```bash
pip  --user install visdom # on server
python -m visdom.server # on server, it will first download some scripts
ssh -L 8097:localhost:8097 username@server_address # on your machine
```

This is called ssh tunnelling, as it creates a tunnel between your machine and the server. Now http://localhost:8097 on the server is mapped to your computer's localhost:8097. To access it simply open http://localhost:8097 in your browser. You should be seeing something like this
 
 ![3](http://siddsax.github.io/blogImgs/viz.png)
 
Now finally to test it, run the code on the server 
```bash
cd $LocationGitRepo
python vizPlot.py
```
Now there should be a graph plotting as below 
![](http://siddsax.github.io/blogImgs/vizPlot.gif)

With this, I end this post here. Hope you got know something or solved a problem!

