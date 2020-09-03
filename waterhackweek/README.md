# Waterhackweek talk on building and using images

## Introduction
On September 2, 2020, Cloudbank team members presented "images" for research computing to the annual Water Hackweek hosted by Christina Bandarogoda and others. 

* Objective 1: De-Mystify cloud use in relation to Jupyter notebooks (largely done)
* Objective 2: Demonstrate "unpacking / building / running" a Python environment with respect to specific code and data (not done)


* The target repository is https://github.com/ChristinaB/dhsvm-opt: Build a working environment from `requirements.txt` or `environment.yml`
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


Yes. But only in the context of this repository. On the cloud the term ***image*** means -- for our purposes -- a `tar` file or if you prefer a `zip` file of an entire operating system including a working environment, home directories, sub-directories, code, installed software, and even data. This image you can think of as sitting on a shelf
(costing very little money) just waiting for the opportunity to be converted back into a real computing environment. 


To make this picture complete we need a couple of additional bits of jargon. An ***instance*** is a Virtual Machine on the cloud. That is, it is a functional 
computer. So once you have made an *image* you load it onto an *instance*. Then you work on the instance normally as you would work on your own computer. The
difference is that the instance can be much more powerful than your own computer if you need the compute power. And that instance is physically located deep 
within a regional cloud computing facility. 


Now for the payoff: Once you have created an **image** of your research computing environment: You can log on to the cloud console and start an 
**instance** (on AWS these are called *EC2* instances) by going through a seven step Wizard as shown below with the red ovals being the seven steps. 
That very first step is "define your operating system" which normally would be a blank slate, like say "give me Ubuntu Linux on my instance"; 
but instead we can choose that research computing image (which includes Ubuntu Linux). Then in step 2 we choose how powerful of a computer we want
and so on through the remaining five steps. At the end of this process there will be a cloud computer or cloud virtual machine or cloud instance
up and running at some ip address, waiting for us to log in and get to work. 


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/whwtalk03.png" alt="regions" width="700"/>


## Demystify #4


*How much does my rented computer on the cloud cost me? And how do I save my work?*


Suppose I look at the dozens of instance type choices and settle on one called **`m5ad.12xlarge`** and I think 'What does that machine cost me
per hour?' In my browser search bar I type `cost m5ad.12xlarge` and the search engine instantly pops up my answer: $2.47 per hour.  

A little bit more investigation shows that this virtual machine (or instance) comes with 24 cores, 192 GB of RAM and 1.8TB of attached
disk volume. So as computers go this one is reasonably powerful. 

Once I am done for the day do I need to create an *image* of this computer? No. It is a good idea to create an image if you want to have a second
version of "everything you have done" to date. But generally speaking you can simply **Stop** the instance. This is like shutting down a computer: 
It will come back when you power it up tomorrow morning, just as you left it. 

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/whwtalk04.png" alt="regions" width="700"/>

## Demystify #5

*Once I start up my Virtual Machine (VM) aka *instance* am I always paying $2.47 per hour for it?*

No, in three sub-answers: 

* You can **Stop** the instance so that you are not paying for it. (You are paying a very small amount now to keep it around 'stopped'.)
* You can ask for your instance to be from the '*pre-emptible*' pool which means it costs much less (say 1/5th) but there is a catch
  * The catch is that it may evaporate on short notice... so you should be ready for that with no serious consequences
  * The pre-emptible resource pool is called the SPOT market on AWS by the way
  * Pre-emptible instances require a bit more learning how to manage but can stretch your computing budget
* You can stop your instance and re-start the image on a smaller, cheaper machine
  * This is a good strategy if all you are going to do today is edit code for example 
  
## Demystify #6

*Now that my machine is up and running... how do I log in so I can actually use it?*


If you are accustomed to using the command line, like say the `bash` shell: You connect to your cloud instance using `ssh`. For this to work
you need three things: 

* First thing: a keypair file for that `ssh` will use to authenticate
  * you can generate this using the wizard as you start up the virtual machine (see above)
  * the file has a `.pem` file extension so it might be called `mycloudinstance.pem`
  * `ssh` requires that this file have user read-only permission so you will probably run the command
    * `mycomputer> chmod 400 mycloudinstance.pem`

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/whwtalk05.png" alt="regions" width="700"/>


* Second thing: a username which in our case will be the default `ubuntu`
* Third thing: an ip address for your computer
  * you note this in the cloud console after your instance starts up
  * let's assume it is 12.23.34.45

With these three things in hand you can construct an `ssh` connect command like this: 

```
ssh -i mycloudinstance.pem ubuntu@12.23.34.45
```

Here is what the connection exchange looks like. Notice the prompt changes when you are logged in to the cloud instance.


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/whwtalk06.png" alt="regions" width="700"/>


## Demystify #7


*What is an ssh tunnel and why do I need one?*


First `ssh` is a secure connection utility. The tunnel allows data to flow in both directions between computers in a secure
manner. We use this to create a browser view on our local computer that corresponds to Jupyter Lab activity on a remote computer. In this
case the remote computer is the instance on the cloud we have set up. As a result of all this fussing about: The browser tab look and acts
just like a Jupyter Lab notebook server that we are running locally, for example on our laptop or desktop computer. However the actual 
Jupyter Lab server is running on the cloud instance; and if that happens to be a powerful instance like the one described above: We are
able to do some pretty heavy computational work.


### An advisory note on preserving your work


Something to bear in mind as you get familiar with working in the cloud environment: If you develop code in a Jupyter notebook and save that
on your virtual machine (instance): You have a nice stable copy of your work. You can stop and then restart the instance and your work
will still be there. You can make an image of your instance and then ***Terminate*** the instance (completely wipe it clean
and return it to the cloud resource pool) and your work will still be preserved in the image you created. Then you can start a new instance 
from that image and your work will be there on it. So far so good; but it is a good idea to check all of this out before taking it for granted.


The point is your work on the cloud is stable... but sometimes human error can intervene. So let's go one step further in keeping your work safe. 


We strongly recommend that you tie your work to a git repository that has nothing to do with your cloud account. For example we
use GitHub for this purpose; which is closely tied to the Linux `git` version control utility. 
The point is: At the end of any given day you can issue four `git` commands to synch your local work on the cloud instance with your 
GitHub repository. Now you have a completely independent copy of your work which means that if human error intervenes to mess up
the cloud situation in some way: You will still have your GitHub copy.


### Completing the `ssh` tunnel

Here are some screen captures from the tunnel process, up to turning on the Jupyter Lab server. To recreate this: Follow the steps in the
tutorial README at the root of this repository. The steps shown are, in descending order:


* On the cloud machine: Starting a Jupyter notebook server (Jupyter Lab) with no browser output and input expected on port 8889
* Part of the token string returned when the previous step is successful: The token is for authentication
* Completing the ssh tunnel from my computer to the cloud computer running Jupyter Lab
* Entering the localhost (port 7005) on my computer, i.e. connecting my browser to the `ssh` tunnel to the cloud Jupyter server
* Jupyter Lab starting in my browser; where I enter the token to authenticate


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/whwtalk07.png" alt="regions" width="700"/>


## Still to complete


We would like to add in here some material on cloning a repo and getting it to run properly. The clone step is easy: On a terminal 
window in the Jupyter Lab environment type in

```
prompt> cd ~
prompt> git clone https://github.com/ChristinaB/dhsvm-opt
```

Now comes the missing part...


## Conclusion

This presentation is an unvarnished summary of ‘what you need to know’ to operate your own Jupyter notebook server (Jupyter Lab) independent of other infrastructure. 
Some key takeaways…

* You choose to dial up / dial down the machine power and cost
* Setting up a pre-built image allows you to treat your work environment like a tar file
* This process applies to any cloud 
* Using an ssh tunnel is one approach; using remote desktop is another
