
# Unearthing Multi-label learning

TL; dr What is multi-label learning, why is it essential and body of work on it.

When looking at machine learning tasks, most of them have been out there since the beginning of the field like multi-class classification, recommender systems, clustering, regression, density estimation, topic modeling or dimensionality reduction not counting the new wave of tasks in vision or language. One problem that has come up not so long ago is multi-label learning.

Now defining multi-label learning is very simple; in a nutshell, it is a multi-class classification problem where the target can have any number of classes opposed to just one class making it, in general, harder than multi-class classification. The roots of multi-label learning can be found in recommendation systems where models have to recommend say K out of N items and topic models where the aim is to predict the topics present in the document. Multi-label learning is a generalization of these.

Coming to why it's important, maybe more so than traditional multi-label classification. Multi-label classification when done on a small number of classes is close in utility to multi-class classification but another variant of it, when the number of classes is LARGE (  like 1 million ), also called extreme multi-label classification (**XML**). It becomes a potent tool that can be deployed in conjunction with databases to efficiently do tasks like querying in a fast and efficient manner. This is tremendously useful in tools like Wikipedia, the secret sauce of search engines, Adsense or e-commerce sites. 

![](http://siddharthasaxena.com/blogImgs/bing.png)  

Now we come to some of the methods that are employed for multi-label learning.

The interesting thing here is that it is not deep learning that is the proverbial king here — neither Bayesian models or SVMs. Instead, carefully drafted embedding and tree-based models are the most widespread here. Although the most surprising thing of all is that the outright state-of-art here is a set of K linear regressors for K labels! [2] albeit run on CPUs with 1000s of cores. 

Here our focus remains on techniques that are feasible with hardware that ordinary people possess, that is a few CPU cores and a couple of GPUs.

## Embedding based models:

Embedding based models are easy to describe. a ) Take high dimensional data, project it to low dimension. b) Take high-dimension labels, project them to low dimensions as well. c) Now solve this classification problem in low dimension. 

In practice, at least for medium-sized datasets, it performs great as well although the performance degrades slightly as the size of the datasets decreases. The great thing about XML is that most of the state-of-art models are available [here](http://manikvarma.org/downloads/XC/XMLRepository.html). Hence running the model is as simple as running a few lines of code. 

### SLEEC
SLEEC or Sparse Local Embeddings for Extreme Multi-label Classification ( Kush Bhatia, Himanshu Jain, Purushottam Kar, Manik Varma, and Prateek Jain ) is the state-of-the-art embedding based model. SLEEC follows the same basic recipe as mentioned above. The difference here being that SLEEC tries to preserve distances between local embeddings, the locality found by clustering the data. 

## Tree-based models:

Tree-based models have also based upon one a basic strategy, that is constructing a tree out of the data points, that clubs several data points together at their leaf nodes. Then to make predictions, a new data point is assigned to one of the leaf nodes and then mean of the labels of that node is the predicted labels by the model. The art here is how these trees are designed themselves, i.e., the way to partition data points at each node.  We now discuss FastXML which is not state-of-the-art but as the name suggests fast even on large datasets and can be run on single CPU core machine as well.

### FastXML

FastXML was developed by people at MSR Bangalore and is a reliable, fast classification model. The key idea of a tree-based model is how to decide which node goes to the left sub-tree and which node goes to the right sub-tree. In fast XML the authors directly optimize the nDCG score. But doing so is not a straight-forward task as nDCG is sharply discontinuous. 
To overcome this hurdle, the authors propose the use of a linear regressor at each node to divide the data points. Their labels (+/-) are decided through alternating maximization i.e. each group's nodes must be such that the items that have highest ranks for each of them are same or similar.

### Deep learning models:

Deep learning models are relatively new here, and this is because scaling them to such large datasets remains a challenge. But they can be leveraged in a complementary fashion to traditional models as opposed to compete with them. Deep learning models are known to be great at feature extraction, and this can be used with methods like FastXML or PfastreXML which otherwise use the TF-IDF features only.

The architecture of one of the most recent works that use deep learning for XML is XML-CNN based upon the CNN for text classification by Kim [3]. The differing aspect of their model is that they use Dynamic Max Pooling, i.e., instead of having only one feature after pooling, have p features as multiple convolution layers are used over the input. The authors also introduce a hidden bottleneck layer and show that it further helps in improving accuracy. Now is a hands-on tutorial of using XML-CNN which can even be run on a CPU.
 
First, download the code-base from my git [repo](https://github.com/siddsax/XML-CNN), while RCV news dataset can be downloaded from [here](http://cse.iitk.ac.in/users/siddsax/rcv.p). Place it inside the codebase within a folder named **datasets** To achieve the full potential of the model; it is desirable to use glove embeddings than cold-starting the model. Download the 6B embeddings from [here](https://nlp.stanford.edu/data/glove.6B.zip) and store them inside a folder named **embedding_weights** inside the codebase.
All these steps can be done with the following set of bash commands
```bash
git clone https://github.com/siddsax/XML-CNN
cd XML-CNN
mkdir datasets
cd datasets
wget http://cse.iitk.ac.in/users/siddsax/rcv.p
cd ..
mkdir embedding_weights
cd embedding_weights
wget https://nlp.stanford.edu/data/glove.6B.zip
unzip glove.6B.zip
cd ..
```
Now we are ready to run the code!
```bash
python main.py --mn=rcv # train a model and save in directory rcv [inside saved_models]
```
The above code will run for some time; simultaneously it will also print the precision scores as below.

![](http://siddharthasaxena.com/blogimgs/p1.png)

So that wraps up the review of multi-label learning. Thanks for reading!

[1] Bing.com
[2] Sparse Local Embeddings for Extreme Multi-label Classification
[3] Convolutional Neural Networks for Sentence Classification
