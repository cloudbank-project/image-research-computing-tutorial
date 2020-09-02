# Waterhackweek talk on building and using images

## Introduction
On September 2, 2020, Cloudbank team members presented "images" for research computing to the annual Water Hackweek hosted by Christina Bandarogoda and others. 

* Objective: De-Mystify cloud use in relation to Jupyter notebooks
* Starting with https://github.com/ChristinaB/dhsvm-opt: Build a working environment from `requirements.txt` or `environment.yml`
  * Data is on a public repository (separate from above)
  * Which cloud platform can I use to run another version of this experiment? (any)
  * Interactive example notebook: How to get from a repo to the Amazon cloud? (stay tuned)
  * How do I configure and run this workflow on the public  cloud without a JupyterHub community service? (ditto)  
  * What are the costs and benefits of building a custom kernel on a community jupyterhub vs an individual server in the cloud with access to Jupyter Notebooks and terminals? (horsepower)

## Executive Summary in two parts

The path to your own (supercharged) Jupyter environment is not trivial but it is

* doable
* super flexible in relation to your data/compute needs
* available 24/7
* cost is proportional to use 


Paying for it: Start with free credits (trial account provides $100). See if this works for you. If so: Set up a real account backed by funds to pay for your cloud use. Credits programs sponsored by vendors can defray initial expense; emphasis on stopgap, not ongoing support. 


## Demystify #1

How do I interact with the cloud?
* Usually first is through a browser, authenticating to a console interface. The AWS console is shown below.
* Next is to install and learn the command line language for the particular cloud
* Third is to use a coding interface. For example using Python with AWS: `boto3`

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/whwtalk01.png" alt="screencap" width="700"/>

## Demystify #2 

*Where is the cloud? Is it somewhere? Is it in Cloud City?*


The cloud is actually in a huge computing facility in Oregon. And there is another in Ohio. And in Paris and in Milan... but not Prague. 
These are called *Regions* and each one is actually a collection of facilities. 


*Does this matter?*


Yes! Once you know that there are multiple regions... and you are looking for something that you know is there... but you can’t find it: 
You might be looking in the wrong region even if you are on the right cloud.


Or you might be on the wrong cloud! The Jupyter server building covered in this tutorial works equally well on AWS, Azure or Google Cloud Platform and for about the same cost. While these instructions are specific to AWS they are in essence completely adaptable to other clouds.


Here is a listing of AWS **Regions**.

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/whwtalk02.png" alt="regions" width="300"/>

## Demystify #3


*I've been told that *image* is everything... but that sounds very superficial. Is it really all about image???*


Yes. But only in the context of this talk. On the cloud the term ***image*** means -- for our purposes -- a `tar` file or if you prefer a `zip` file of an entire operating system including a working environment, home directories, sub-directories, code, installed software, and even data. This image you can think of as sitting on a shelf
(costing very little money) just waiting for the opportunity to be converted back into a real computing environment. 


To make this picture complete we need a couple of additional bits of jargon. An ***instance*** is a Virtual Machine on the cloud. That is, it is a functional 
computer. So once you have made an *image* you load it onto an *instance*. Then you work on the instance normally as you would work on your own computer. The
difference is that the instance can be much more powerful than your own computer if you need the compute power. And that instance is physically located deep 
within a regional cloud computing facility. 

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/whwtalk03.png" alt="regions" width="700"/>

## Demystify #4


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/whwtalk04.png" alt="regions" width="700"/>


## Demystify #5

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/whwtalk05.png" alt="regions" width="700"/>

