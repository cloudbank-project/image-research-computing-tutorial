# Bootstrapping a machine image

## Introduction: What is this sub-folder about? 

This is a tutorial for creating the image referenced on the main page of this repository. It is very much a ***prequel***.
Here is the sequence of events. 

1. A Researcher decides to do some work on the public cloud; and gets access
  * Through Cloudbank the mechanism is a *billing account*
  * By public cloud we mean for example the Google Cloud, Azure Cloud, or Amazon Cloud
3. They start up a Virtual Machine (VM) and log in to it (we will use AWS here as the example)
4. They configure the VM to act as a Jupyter Lab notebook server supporting a Python 3 kernel
5. They add other useful tools / libraries / packages / datasets / etcetera to this VM
6. They log out and use cloud management tools to create an ***image*** of this VM
7. They terminate / delete / destroy the VM so they are not paying for it by the hour

That completes the bootstrapping part covered on this page.

8. Some time later they return to the cloud and follow the main page instructions

Step 8 will eventually lead to doing actual research. 


## Tutorial


### Start a Virtual Machine (VM) and log in to it

* Log on to the AWS console and select Services > Compute > EC2
  * This could also be done using the Command Line Interface (CLI) to AWS
  * This could also be done using the AWS API through a package such as `boto3`
* 

### Configure the VM to act as a Jupyter Lab notebook server supporting a Python 3 kernel


### Add other useful tools / libraries / packages / datasets / etcetera to this VM


* Let's install `cmocean`
* Let's add a dataset
* Let's install a Jupyter notebook repository


### Use cloud management tools to create an ***image*** of this VM

* Make sure to record the name and location of the image so as to make it findable in two years
### Terminate / delete / destroy the VM so they are not paying for it by the hour
