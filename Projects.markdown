---
layout: page
title: Projects
permalink: /projects/
---

## Open Source Contributions

#### PySyft
<em>"A library for answering questions using data you cannot see"</em>
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


#### Deepgaze
Computer Vision library for human-computer interaction in **Python**
<br/>
<p style='text-align: justify;'>
I have refactored certain parts of the library for improved usability and wrote test cases to ensure accurate functionality. Ported library to Python 3.0.
</p>

<br/>

## Some Personal Projects I am proud of

For complete list of projects feel free to check my [Github Profile](https://github.com/kamathhrishi).

**GreyNSights**
<br/>
Privacy Preserving Pandas

**Indoor Scene Recognition with Visual Attributes**
<br/>
- Trained a Residual Network to classify 67 different Indoor Scenes on MITIndoor 67, obtained 60% accuracy on the test dataset.
- MITIndoor67 is a small dataset consisting of 5400 images, training a network directly on the dataset led to substantial overfitting.
- In order to learn finer features, a separate attribute prediction network was trained on SUN attribute database.
- The predicted attributes are used to augment image features in the linear layers of the network scene recognition network.

**Weakly Supervised Street View Text Detection**
<br/>
- Used Pytorch, OpenCV and Pillow in Python
- Trained a character agnostic text detector on Chars74K dataset along with images consisting of indoor/outdoor scenes without text. The character agnostic model is   a alexnet network pretrained on imagenet.
- Used the classifier and sliding windows to annotate images in UCSD SVT and NEOCR dataset to derive bounding boxes
- Trained a street text localisation and detection Fully Convolutional Network(FCN) on the weakly supervised labelled dataset by using text detector’s starting CNN layers
- Currently working on character level segmentation and recognition

**Animals with Attributes**
<br/>
*Supervised Learning*
<br/>

- Trained a 30 layer Residual CNN from scratch which could predict 50 different species of animals with 72% accuracy. Trained it with probabilistic augmentations using Alubmentations library.
- Finetuned an ImageNet pre-trained 18 layer network to attain 89% accuracy
- Spent time understanding and analyzing top Imagenet CNN architectures  

*Zero Shot Learning*
<br/>

- Currently exploring Zero shot learning on the dataset. A naive implementation with Direct Attribute prediction attains 35% accuracy prediction unseen classes.

<br/>