---
layout: post
title:  "Analyze Private datasets using Pandas"
date:   2021-02-22 16:42:52 +0530
categories: jekyll update
---

<p style="text-align:justify">Conventionally pandas allows you to analyze datasets that are present locally on your PC, that is when you are given access to a given dataset.
But, there are valuable and informative datasets that consists of Personally Identifiable Information (PII) that you wont be able access to directly query it. There are privacy compliances that prevent data from being moved in some industries such as Banking and Healthcare.
With the use of framework GreyNSights you can leverage pandas to analyze and transform datasets that are sensitive. GreyNSights is stricly based on
client-server implementation. The data owner hosts the dataset which begins a server that listens in to requests. The data analyst connects to the dataowner
using client scripts of GreyNSights and can interactively query the dataset remotely. With GreyNSights the data owner doesn't have to move the dataset out of their premises and analyst can still analyze the dataset. So its a win-win situation for both. Analyst can now get access to datasets that are otherwise
difficult to get and data owners can now maintain privacy of individuals in dataset.</p>


<b>Repository: </b><a href="https://github.com/kamathhrishi/GreyNSights">Link</a>


<h1> Design Principles </h1>
<p style="text-align:justify">Some other approaches for analyzing and querying privacy sensitive datasets uses anonymization techniques to obfusticate or remove PII's and then share the dataset. While, this seems like a promising approach. It could lead to linkage attacks. Analysts could reidentify anonymized data rows by linking it with other data sources. One such popular attack is the <a href="https://arxiv.org/abs/cs/0610105">Netflix attack</a>[1]. The netflix dataset was anonymized and posted publicly, yet identity of the users could be recovered by linking with publicly available IMDB dataset.</p>
<p style="text-align:justify">Using anonymization techniques are not the best method of attaining privacy. This motivates the idea of being able to use PII's but such that the rows of PII's are not directly seen by the analyst. Further, when the rows are restricted for viewing analysts could also recover values of the individual rows using differencing attacks where they perform two queries one for the whole dataset and another excluding a given datapoint. Say one query where we calculate a sum with a given datapoint and another query without a given datapoint. Subtracting the values of both gives the value of a sensitive row. This attack is known as differencing attack. It could be mitigated by instead returning differentially private answers to queries rather than exact answers to queries. Differentially private results are query results to which well calibrated noise is added such that the result of a query is almost the same even if a given datapoint is not present. I have explained Differential Privacy in detail in my blogposts <a href="https://kamathhrishi.github.io/DPIntro/">DP Intro</a> and <a href="https://kamathhrishi.github.io/DPIntro/">DP Mechanisms</a>.</p>
<p style="text-align:justify">The fundamental design principles of the framework are:</p>

* <b>No raw data is exposed only aggregates</b>

  <p style="text-align:justify">The analyst can query and transform the dataset however they would want to, but can only get the aggregate results back.</p>

* <b>The aggregates or analysis does not leak any information about individual rows</b>

   <p style="text-align:justify">The aggregate results are differentially private securing data rows from differencing attacks.</p>

* <b>Pandas capabilities to transform and process datasets is still preserved</b>

  <p style="text-align:justify">The analyst might have to add a few lines of code for initializing the setup with dataowner, but they would essentially use the same pandas syntax ensuring   
  anybody who already knows pandas could use without having to learn anything more. </p.

<h2>Pointers</h2>
<p style="text-align:justify">Every query executed remotely gives a pointer object to the analyst. This pointer object is a reference to the actual object that lives on the dataowner's server.These pointers could be transformed and used the same way, the underlying object could be used. When a operation is performed on the pointer, the transformation occurs remotely on the object the pointer points to. The analyst could fetch the actual result of the pointer by calling get() on the pointer object. But, the actual results are retrieved only if the result is an aggregate result. The analyst is restricted from looking at rows or columns of dataset. </p>


<h1> Simple Example</h1>
<p style="text-align:justify">Below is an simple example where an data owner hosts a dataset on animals and carrots and the analyst queries it remotely. The example reproduces the official example of base Differential Privacy package PyDP. You can view the <a href="https://github.com/kamathhrishi/GreyNSights/tree/main/examples/carrots_demo">example</a> in the official repository.</p>

<h2> Data Owner</h2>

``` python
import pandas
from GreyNsights.analyst import Pointer
from GreyNsights.host import Dataset, DataOwner
from GreyNsights.config import Config

dataset = pandas.read_csv(
    "animals_and_carrots.csv", sep=",", names=["animal", "carrots_eaten"]
)


owner = DataOwner("Bob", port=6544, host="127.0.0.1")

config = Config(owner)
config.load("test_config.yaml")

dataset = Dataset(owner, "Sample Data", dataset, config, whitelist={"Alice": None})
dataset.listen()
```

<h2>Analyst</h2>

The below script allows the analyst to connect to the remote datasource.

``` python
import GreyNsights
from GreyNsights.analyst import DataWorker, DataSource, Pointer, Command, Analyst
from GreyNsights.frameworks import framework

"""The analyst identity. Currently just a placeholder and non-functional. But , in future could allow analyst to identify
   with a x.509 certificate"""

identity = Analyst("Alice", port=65441, host="127.0.0.1")

#The details of remote datasource
worker = DataWorker(port=6544, host="127.0.0.1")

#The dataset hosted by remote datasource. Currently supports only 1 datasource.
dataset = DataSource(identity,worker, "Sample Data")

#Initialization pointer
dataset_pt = dataset.init_pointer()

frameworks = framework()

#Initialize GreyNSights version of Pandas
pandas = frameworks.pandas

```

<p style="text-align:justify">Once the dataowner has hosted the dataset server for listening to requests , data analyst can request for the config from data owner
                              to understand the level of privacy the dataowner has enforced.</p>
``` python
config = dataset.get_config()
print(config)
```

<p style="text-align:justify">In order to get a initialization pointer the dataowner has to approve the config</p>

```python
dataset_pt = config.approve().init_pointer()
```

Just to depict that the GreyNSights version of Pandas could be used like ordinary Pandas. The dataset is already a dataframe.
```
df = pandas.DataFrame(dataset_pt)
```

The command displays the columns of dataset
``` python
df.columns
```

<p style="text-align:justify">Executes describe remotely, which returns a pointer and gets underlying value of it when get() is called. Describe is an aggregate query so the analyst is permitted to see the result. </p>
``` python
df.describe().get()
```

Calculates mean, standard deviation and sum.
``` python
df['carrots_eaten'].mean().get()
```

``` python
df['carrots_eaten'].sum().get()
```

``` python
(df['carrots_eaten']>70).sum().get()
```

<br/>
<br/>

<h1>Analyze Multiple Parties Datasets (Federated Analytics)</h1>
<p style="text-align:justify">We can query multiple datasets as a single dataset, ensuring that the analyst is exposed only to aggregate values. But, the individual parties values are not exposed to the analyst. The method known as Federated Analyticals. Currently the framework supports only counts , approximate standard deviation, mean and sum. </p>

<h2> Data Owner</h2>
The code for launching data owner server. Similarly, host several dataowners with different ports and dataset names.

``` python
import pandas
from GreyNsights.host import Dataset, DataOwner
from GreyNsights.config import Config

dataset = pandas.read_csv("week_data.csv")

owner = DataOwner("Bob", port=65444, host="127.0.0.1")

config = Config(owner)
config.load("test_config.yaml")

dataset = Dataset(owner, "Sample Data1", dataset, config, whitelist={"Alice": None})
dataset.listen()
```

<h2>Analysis</h2>

```python
identity = Analyst("Alice", port=65441, host="127.0.0.1")

# Initialize DataOwner1
worker1 = DataWorker(port=65444, host="127.0.0.1")
dataset1 = DataSource(identity, worker1, "Sample Data1")
config1 = dataset1.get_config()
dataset1 = config1.approve().init_pointer()

# Initialize DataOwner2
worker2 = DataWorker(port=65446, host="127.0.0.1")
dataset2 = DataSource(identity, worker2, "Sample Data2")
config2 = dataset2.get_config()
dataset2 = config2.approve().init_pointer()

# Initialize DataOwner3
worker3 = DataWorker(port=65442, host="127.0.0.1")
dataset3 = DataSource(identity, worker3, "Sample Data3")
config3 = dataset3.get_config()
dataset3 = config3.approve().init_pointer()

# Create a workergroup to which commands to all dataowners are executed together
group = WorkerGroup(identity)
group.add(dataset1, worker1, config1)
group.add(dataset2, worker2, config2)
group.add(dataset3, worker3, config3)

pt = group.init_pointer()

# Perform queries on all three workers together
er = pt["Money spent (euros)"].sum() + pt["Money spent (euros)"].sum()

er = pt["Money spent (euros)"].sum().get()

print(er)
```
<p style="text-align:justify">When get() is called on the workergroup pointer, dataowner communicate with each other and create shares.
These exchanged shares are added by the analyst to obtain the summation of shares. This ensures that the
analyst does not look at the individual values of dataowners but gets only the aggregate values of all dataowners.
The underlying protocol used is called <b>Secure Aggregation</b> [2]. Secure aggregation works only for linear queries
such as mean, sum & standard deviation.</p>

## References

1. A. Narayanan and V. Shmatikov Robust de-anonymization of large sparse datasets (how to break anonymity of the netflix prize dataset). In Proceedings of IEEE Symposium on Security and Privacy. 2008
2. K. A. Bonawitz Vladimir Ivanov Ben Kreuter Antonio Marcedone H. Brendan McMahan Sarvar Patel Daniel Ramage Aaron Segal Karn Seth Practical Secure Aggregation for Federated Learning on User-Held Data NIPS Workshop on Private Multi-Party Machine Learning, 2016
