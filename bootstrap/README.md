# Bootstrapping a machine image

## Introduction: What is this sub-folder about? 

This is a tutorial for creating the image referenced on the main page of this repository. It is very much a ***prequel***.
Here is the sequence of events. 

1. A Researcher **R** decides to do some work on the public cloud; and gets access
    * Through Cloudbank the mechanism is a *billing account*
    * By public cloud we mean for example the Google Cloud, Azure Cloud, or Amazon Cloud
2. R starts a Virtual Machine (VM) and logs in
    * We will work on AWS for the example below
3. R configures the VM to act as a Jupyter Lab notebook server supporting a Python 3 kernel
    * R adds other useful tools / libraries / packages / datasets as well...
4. R logs out and uses cloud management tools to create an ***image*** of this VM on the cloud
    * This image is specific to the cloud used; it is not transferrable
    * For trainsferrable computing environments: Look into *containers*
5. R terminates / deletes / destroys the VM so as not to continue paying for it the hour
    * Notice though that the ***image*** persists and will be used in the future
    * This is where the main tutorial begins, assuming the existence of such an ***image***

The above five steps are the bootstrapping covered on this page.


## Tutorial


### Start a Virtual Machine (VM) and log in to it

* Log on to the AWS console and select Services > Compute > EC2 > Launch Instance
  * This could also be done using the Command Line Interface (CLI) to AWS
  * This could also be done using the AWS API through a package such as `boto3`
* Run through the launch Wizard; here are some details from our example run
  * The image selected will be 64 but x86 Ubuntu Server (username = `ubuntu`)
      * Notice that this is a "bare machine" with just the Ubuntu operating system
  * The VM selected is an `m5ad.4xlarge` which is quite expensive: About $5 per day or $2000 per year
      * ***We strongly recommend following through this tutorial to the Terminate stage!!!***
          * Failure to do so may result in you being charged for this Virtual Machine at this exhorbitant rate.
          * We refer to this as zombie resource charges: You may forget about it but the cloud provider will not!
  * Configure instance: Default values
  * Add storage: Default values (note 2 x 300GB SSD drives are included)
  * Add Tags: Added key values `Name`, `Project`, `Date`, `Owner`, `URL`
  * 

### Configure the VM to act as a Jupyter Lab notebook server supporting a Python 3 kernel


### Add other useful tools / libraries / packages / datasets / etcetera to this VM


* Let's install `cmocean`
* Let's add a dataset
* Let's install a Jupyter notebook repository


### Use cloud management tools to create an ***image*** of this VM

* Make sure to record the name and location of the image so as to make it findable in two years
### Terminate / delete / destroy the VM so they are not paying for it by the hour
