---
layout: page
title: Projects
permalink: /projects/
---

## Open Source Contributions

<br/>
**Core Contributor, SyMPC [2021-]**
<br/>
<em>"A library for training Neural Networks using Multi Party Computations using Pytorch"</em>
<br/>
<br/>
Been working on something awesome. Will come back to you on this :). Currently, feel free to stalk my Pull requests and commits on Github to understand better.

<br/>

**PySyft [2018-19]**
<br/>
<em>"A library for answering questions using data you cannot see"</em>
<br/>
<br/>
A framework for privacy-preserving deep learning using Pytorch and Tensorflow.
<br/>
I worked on **Pytorch** development.
Part of the Core team
Contributed to Core PySyft Team
<br/>
<br/>
The experience helped me learn a lot of software engineering practices such as unit tests, code reviews, Git, writing clean and documented code.
<br/>
- Wrote use cases of Federated Learning for developing Word Embeddings from Private Data and demonstration on CIFAR10
- Wrote an implementation of Differential Privacy method Private Aggregation of Teacher Ensembles (PATE)
- Tried working on Polynomial Tensor for non-linear computation in multiparty computation (MPC) setting. The method approximated non-linear computation using interpolation/Taylor series methods. Could not integrate it as part of Syft chain of tensors.
- Refactored data loaders and federated dataset and wrote a tutorial on developing custom federated datasets
- Type annotated the codebase and wrote documentation for parts of it

<br/>

**Deepgaze**
<br/>
<em>Computer Vision library for human-computer interaction in **Python**</em>
<br/>
<p style='text-align: justify;'>
I have refactored certain parts of the library for improved usability and wrote test cases to ensure accurate functionality. Ported library to Python 3.0.
</p>

<br/>

## Some Personal Projects I am proud of

For complete list of projects feel free to check my [Github Profile](https://github.com/kamathhrishi).

**GreyNSights**
<br/>
<p style="text-align:justify">GreyNSights is a Framework for Privacy-Preserving Data Analysis.</p>
<p style="text-align:justify">Currently with support only for Pandas. The framework allows analysts to remotely query a dataset such that the dataset remains at source and private to data analyst. The query results returned are differentially private. The framework offers flexibility to the analyst by ensuring that they can use the same pandas syntax for analyzing and transforming datasets, but cannot view the individual rows. GreyNSights also offers flexibility to query several parties together and get aggregate statistics without revealing individual counts of parties.</p>

<br/>

**Indoor Scene Recognition with Visual Attributes**
<br/>
- Trained a Residual Network to classify 67 different Indoor Scenes on MITIndoor 67, obtained 60% accuracy on the test dataset.
- MITIndoor67 is a small dataset consisting of 5400 images, training a network directly on the dataset led to substantial overfitting.
- In order to learn finer features, a separate attribute prediction network was trained on SUN attribute database.
- The predicted attributes are used to augment image features in the linear layers of the network scene recognition network.

<br/>

**Weakly Supervised Street View Text Detection**
<br/>
- Used Pytorch, OpenCV and Pillow in Python
- Trained a character agnostic text detector on Chars74K dataset along with images consisting of indoor/outdoor scenes without text. The character agnostic model is a alexnet network pretrained on imagenet.
- Trained a street text localisation and detection Fully Convolutional Network(FCN) on the weakly supervised labelled dataset by using text detector’s starting CNN layers
- Reduced time to label a single image by 34% by training a smaller network using Knowledge Distillation, making the model capable of labelling thousands of images in a few hours.
- Also explored and analyzed other methods for Neural Network compression.
- Used the distilled network and sliding windows to annotate images in UCSD SVT and NEOCR dataset to derive bounding boxes

<br/>
