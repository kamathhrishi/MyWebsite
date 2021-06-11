---
layout: post
title:  "Deep Learning in Practice-Be The algorithm"
date:   2020-06-19 16:42:52 +0530
categories: jekyll update
summary:  "Manual reannotation is the process of you going through image/text of your dataset and understanding what features exactly define this given image. Manually reannotating not only helps remove incorrect labels but also helps understand biases your algorithm could pick up on or even helps you pick up on certain data for the algorithm to make training easier."
---


<p style="text-align:justify">Conventional machine learning required the practitioner to manually look at images/text and handcraft appropriate features. Deep Learning models are powerful, powerful enough that they could be trained to memorize random data without any real correlations <a href="#ref1">[1]</a>. This allows us to learn from input/output pairs for tasks end to end without much preprocessing effort giving the flexibility to learn features from raw images/text but at the expense of a lot of data.</p>
<center>
<img height="400px" width="600px" src="{{site.baseurl}}/assets/BEALGO1.png">
<p>The above image depicts how an neural network transforms an image into set of features upon training.</p>
</center>
<p style="text-align:justify">Deep Learning isn't magic, it still needs some correlated patterns to learn required features and generalize to unseen data. This requires the deep learning practitioner to understand the training dataset. As the common machine learning phrase goes <i>"Your algorithm is good as your data"</i>. Deep learning models help extract features relevant to given dataset, provided there are enough examples that expose right features.
 <b>Manual re-annotation</b> is the process of going through every image/text of the dataset and understanding what features exactly define a given class.
 Manually reannotating a dataset not only helps <b>remove incorrect labels</b> but also <b>understand biases</b> the algorithm could pick up on or even <b>understand neural network predictions</b> to an certain extent. While its true neural networks are still blackboxes or less interpretable but on a high level there are still ways to understand what they learned from the dataset. Further manual re-annotation also helps decide <b>data augmentation stratergy</b> that could help the model generalize or provides cues on what additional data could help.</p>
<p><b>Note: </b>Manual re-annotation is not a standard term, just a term I introduced to make my idea clear</p>

<center>
<img height="400px" width="800px" src="{{site.baseurl}}/assets/BEALGO2.jpeg">
</center>

<br />

<p style="text-align:justify">I would like to take an example of manually re-annotating a dataset for a computer vision classification task <b>Indoor Scene Recognition</b>. The dataset consists of around only 5000 images of 67 different classes <a href="#ref2">[2]</a>. In this post I took an example of Computer Vision task, you can similarly analyze text/speech datasets or environments in Reinforcement Learning.</p>

<p style="text-align:justify">Let us annotate the class bar. Think of what features could possibly define bar: high chairs , cups with drink , bottles , low lit eenvironments, bar counters, people and some food. Ofcourse not just these individual attributes but in the context of them occuring toghether. Depending on the label, all or some features could exist for the image to satisfy requirement of the label. Let us look at a few sample images which are labelled as bar in the scene recognition dataset and decide how to eliminate or keep them based on the features we would want to define a bar.</p>
<div>

<br />
<p float="center">
 <img src="{{site.baseurl}}/assets/BEALGO3.jpeg" width="200" height="150"/>
</p>
<br />

<p style="text-align:justify">
The first image meets our definition of a bar : high chairs [check], drinks [check] and bar counter[check]. Although to be able to generalize these features you would need several different variations of the image,such as ones with a bartender, people taking a drink and different shades of furnitures.
<br />
<p float="center">
<img src="{{site.baseurl}}/assets/BEALGO4.jpeg" width="200" height="150"/>
</p>
<br />

The second image could be at a bar , but looks more like a fancy bakery or deli with only signal being the high chairs.  
<br />
<br />
<p float="center">
<img src="{{site.baseurl}}/assets/BEALGO5.jpeg" width="200" height="150"/>
</p>
<br />

The third image also looks more like a Deli or fancy bakery counter.
<br />
<br />
<p float="center">
<img src="{{site.baseurl}}/assets/BEALGO6.jpeg" width="200" height="150"/>
</p>
<br />
<p float="center">
<img src="{{site.baseurl}}/assets/BEALGO7.jpeg" width="200" height="150"/>
</p>
<br />
<p style="text-align:justify">The fourth and fifth image strengthens the features in first image and so does the fifth image making them appropriate training images. They contain high chairs, bottles, bar counters and even people.</p>
<br />
<p float="center">
 <img src="{{site.baseurl}}/assets/BEALGO8.jpeg" width="200" height="150" />
</p>
<br />
<p style="text-align:justify"> The last image could be bar but could possibly be in the category of gameroom or bowling.
The only strong signal being the high chair, the images of bottles are too small to be learnt.</p>
<p style="text-align:justify">While annotating the images you would notice that only if you can clearly identify why the image belongs to the category you could plausibly expect the algorithm to learn the required features. But, there are possible features that the algorithm picks on that are not visually clear. The quality of features learnt depends on the amount of images supporting the features. But with enough variations of images with supporting features it makes them less likely to overfit to other aspects of the image which will be further discussed. Below are some  things I noticed when reannotating the dataset.
</p>


<center>


<h2>Incorrect Labels</h2>
<div>
<center>

<p float="center">
  <img src="{{site.baseurl}}/assets/BEALGO9.jpeg" width="200" height="150" />
  <img src="{{site.baseurl}}/assets/BEALGO10.jpeg" width="200" height="150"/>
  <img src="{{site.baseurl}}/assets/BEALGO11.jpeg" width="200" height="150"/>
</p>


<p style="text-align:justify">The dataset had some incorrect labels which could also lead to learning some incorrect feature or making it difficult to learn the right features. Above images show some incorrect labels present in the dataset. The first image above was labelled as bedroom, second as auditorium and third as bedroom.</p>

<br />
<h2>Multiclass Images</h2>
<p float="center">
 <img src="{{site.baseurl}}/assets/BEALGO12.jpeg" width="200" height="150" />
  <img src="{{site.baseurl}}/assets/BEALGO13.jpeg" width="200" height="150"/>
  <img src="{{site.baseurl}}/assets/BEALGO14.jpeg" width="200" height="150"/>
</p>


<div>
<center>
<p style="text-align:justify">Some images do have certain attributes that are labelled as a single class but could possibly attributed to several classes. In the above images, the first image is labelled as a train station but has features that make it suitable for being a part of mall or church. The second has a roof and clock that make it seem more like a trainstation but belongs to staircase class. The third has the finish, lightning and shops akin to a mall but is labelled as staircase.</p>
<br />
<br />
<h2>Relating Inference Results</h2>

<p style="text-align:justify">Some observations I made after I trained a Alexnet model<a href="#ref3">[3]</a> on the dataset. The inference was stricly performed on the test data.</p>
<br />
<h2>Color and Texture</h2>
<br />
<p float="center">
 <img src="{{site.baseurl}}/assets/BEALGO15.jpeg" width="200" height="150" />
<img src="{{site.baseurl}}/assets/BEALGO16.jpeg" width="200" height="150"/>
</p>
<div>
<center>
<p style="text-align:justify">In the above example the bedroom was incorrectly classified as a gameroom which could be explained by the large of number of images of pool tables in gameroom category. This could be solved by the right augmentation of colored images in the dataset with several different textures or simply getting more varied data. Similarly there were instances where light bluish and green textures were classifed as hospitals rooms. This could be explained by the large number of images of paramedics with bluish green clothes in the training dataset.</p>

<br />
<h3>Brightness</h3>
<p style="text-align:justify">Ensure there are enough bright and dark images of for a given class unless being low lit or bright is a feature of a given category. Most low lit images were classified as a bar or were present in top-3 predictions as almost all bar pictures were low lit.</p>

<br />
<h3>Video and Book Store</h3>
<p float="center">
 <img src="{{site.baseurl}}/assets/BEALGO17.jpeg" width="200" height="150" />
<img src="{{site.baseurl}}/assets/BEALGO18.jpeg" width="200" height="150"/>
</p>
<p style="text-align:justify">There were two different categories called video and book store. Individually observing each of the images gave little or no information on whether racks contained books or video sets. They could instead be combined into a single category.</p>

<br />
<center>
<h3>Scale of objects</h3>
<p style="text-align:justify">The success of scene recognition depends upon recognizing objects associted with a given scene. When objects are of different scales it can make it harder to learnt the required patterns and also making it hard to generalize during inference. Similarly the scale of features in any classification task need to be similar or the network needs to be exposed to enough images of different scales.</p>

<br />
<center>
<h2>The End</h2>

<p style="text-align:justify">I first read about this in Andrej Karpathy's <a href="http://karpathy.github.io/2014/09/02/what-i-learned-from-competing-against-a-convnet-on-imagenet/">blogpost</a> on his analysis on ImageNet and thought it was a simple but powerful technique for putting deep learning into practice. Understanding your data is definitely  important but it is also important to have a powerful model architecture and practices to be able to learn from the data. Ideally learning the right features require several different images with variations, which could be possible by gathering more data or by augmentations. Visualizing your dataset would give an first insight as to what your neural network could have possibly learnt. In order to further understand what your network has learnt, you could use various methods developed for the same such as occulsion experiments or deepdream.</p>  

<p style="text-align:justify">I would like to thank my friend Kavin Kumar for reviewing and helping me improve this article</p>

<br />
<h3>References</h3>
<p id="ref1" style="text-align:justify">
1. Chiyuan Zhang, Samy Bengio, Moritz Hardt, Benjamin Recht, Oriol Vinyals ,<a href="https://www.google.com/search?client=safari&rls=en&q=1.+Chiyuan+Zhang,+Samy+Bengio,+Moritz+Hardt,+Benjamin+Recht,+Oriol+Vinyals+,+%22Understanding+deep+learning+requires+rethinking+generalization%22&ie=UTF-8&oe=UTF-8">"Understanding deep learning requires rethinking generalization"
<p id="ref2" style="text-align:justify">
2. A. Quattoni, and A.Torralba.<a href="https://ieeexplore.ieee.org/document/5206537">Recognizing Indoor Scenes</a>. IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2009
<p id="ref3" style="text-align:justify">
3. Krizhevsky, Alex; Sutskever, Ilya; Hinton, Geoffrey E. (2017-05-24).<a href="https://ieeexplore.ieee.org/document/5206537">"ImageNet classification with deep convolutional neural networks"</a>. Communications of the ACM. 60 (6): 84–90. doi:10.1145/3065386. ISSN 0001-0782.
</p>
