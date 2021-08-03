---
layout: page
title: Projects
permalink: /projects/
---
<center><h3>Open Source Contributions</h3></center>
<br/>
**Core Contributor, SyMPC [2021-]**
<br/>
<em>"A library for training and evaluation Neural Networks using Multi Party Computations in Pytorch"</em>
<br/>
<br/>
I review pull requests, fix bugs, and develop features for the repository.
<br/>
Worked on some functionality of Falcon MPC protocol and improved automatic differentiation module.
<br/>
<br/>

**Core Contributor, PySyft [2018-19]**
<br/>
<em>A framework for privacy-preserving deep learning using Pytorch and Tensorflow.</em><br/>
<br/>
I worked on **Pytorch** development.The experience helped me learn a lot of software engineering practices such as unit tests, code reviews, Git, writing clean and documented code. Some tasks I worked on are:
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
Refactored certain parts of the library for improved usability and wrote test cases to ensure accurate functionality. Ported library to Python 3.0.
</p>

<br/>

<center><h3>Some Personal Projects I am proud of</h3></center>
For complete list of projects feel free to check my [Github Profile](https://github.com/kamathhrishi).
<br/>
<br/>

<h3>GreyNSights</h3>
<b><a target="_blank" href="https://github.com/kamathhrishi/GreyNSights">[Repository]</a></b>
<b><a target="_blank" href="https://kamathhrishi.github.io/MyWebsite/jekyll/update/2021/02/22/Privatepandas.html">[Introductory Blogpost]</a></b>
<center><p style="text-align:justify"><i>GreyNSights is a Framework for Privacy-Preserving Data Analysis.</i></p></center>
<p style="text-align:justify">Currently with support only for Pandas. The framework allows analysts to remotely query a dataset such that the dataset remains at source and private to data analyst. The query results returned are differentially private. The framework offers flexibility to the analyst by ensuring that they can use the same pandas syntax for analyzing and transforming datasets, but cannot view the individual rows. GreyNSights also offers flexibility to query several parties together and get aggregate statistics without revealing individual counts of parties.</p>

**Example Usage**

<p>Owner of a sensitive datasets hosts the dataset</p>
```python
import pandas
from GreyNsights.analyst import Pointer
from GreyNsights.host import Dataset, DataOwner
from GreyNsights.config import Config

dataset = pandas.read_csv("animals_and_carrots.csv", sep=",", names=["animal", "carrots_eaten"])

owner = DataOwner("Bob", port=6544, host="127.0.0.1")
config = Config(owner)
config.load("test_config.yaml")
dataset = Dataset(owner, "Sample Data", dataset, config, whitelist={"Alice": None})

dataset.listen()
```

<p>A data analyst interested in the dataset can now analyze the dataset while maintaining
privacy of dataset and keeping it at source</p>

```python
#Initilization code of GreyNSights
import GreyNsights
from GreyNsights.analyst import DataWorker, DataSource, Pointer, Command, Analyst
from GreyNsights.frameworks import framework

identity = Analyst("Alice", port=65441, host="127.0.0.1")
worker = DataWorker(port=6544, host="127.0.0.1")
dataset = DataSource(identity,worker, "Sample Data")
config = dataset.get_config()

#Initialization Pointer
dataset_pt = config.approve().init_pointer()

#Analysis of dataset
df = pandas.DataFrame(dataset_pt)
df.columns
df.describe().get()
df['carrots_eaten'].mean().get()
df['carrots_eaten'].sum().get()
(df['carrots_eaten']>70).sum().get()
df['carrots_eaten'].max().get()
```

<br/>

<h3>Weakly Supervised Street View Text Detection</h3>
<b><a target="_blank" href="https://github.com/kamathhrishi/Weakly-Supervised-Street-Text-Detection">[Repository]</a></b><br/>
<center>
<img height="200px" width="200px" src="https://github.com/kamathhrishi/Weakly-Supervised-Street-Text-Detection/raw/main/art/1.jpg">
<img height="200px" width="200px" src="https://github.com/kamathhrishi/Weakly-Supervised-Street-Text-Detection/raw/main/art/4.jpg">
<img height="200px" width="200px" src="https://github.com/kamathhrishi/Weakly-Supervised-Street-Text-Detection/raw/main/art/3.jpg">
</center>
<br/>

<p>Trained a Convolutional Neural Network for segmentation and localisation using dataset labelled by charecter detection network</p>

- Used Pytorch, OpenCV and Pillow in Python
- Trained a character agnostic text detector on Chars74K dataset along with images consisting of indoor/outdoor scenes without text. The character agnostic model is a alexnet network pretrained on imagenet.
- Trained a street text localisation and detection Fully Convolutional Network(FCN) on the weakly supervised labelled dataset. The dataset was labelled by the character agnostic text detector
- Reduced time to label a single image by 34% by training a smaller network using Knowledge Distillation, making the model capable of labelling thousands of images in a few hours.
- Also explored and analyzed other methods for Neural Network compression.
- Used the distilled network and sliding windows to annotate images in UCSD SVT and NEOCR dataset to derive bounding boxes
