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
    * Add storage: Default values 
        * Note
            * By selecting the `m5ad.4xlarge` we already have 2 300 GB SSD EBS drives on this instance
            * EBS = Elastic Block Storage (AWS acronym), meaning "big empty disk drives"
            * See the note below on mounting these drives to make them useful
    * Add Tags: Added key values `Name`, `Project`, `Date`, `Owner`, `URL`
    * Configure Security Group: Default values
    * Review and Launch: Created a temporary keypair file, downloaded
        * See the main page of this repo for details
    * Launch: Note resulting ip address of the VM; let's say it is `12.23.34.45`
* On my own computer
    * Relocate the downloaded keypair file to my `bash` home directory
    * `chmod 400` applied to keypair file
    * `my computer> ssh -i keypair.pem ubuntu@12.23.34.45`
* On the VM 
    * (This follows from the `ssh` command issued from a terminal window on my computer, last command above)
    * Mount the two 300 GB SSD 'EBS' drives for use
        * Search engine 'AWS EC2 EBS mount` turns up [this instructive link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)
        * `VM> lsblk` produces a device listing
            * In this case (again with an `m5ad.4xlarge` we see three devices `nvme0n1`, `nvme1n1`, `nvme2n1` of type `disk`
                * These would have a full path `/dev/nvme0n1` etcetera
                * Of these three only the third one `nvme2n1` has an associated partition
                    * This is the root device where the operating system resides
                    * The remaining two disks `nvme0n1` and `nvme1n1` are the empty volumes; these must be mounted for use
                * Verify that there is no file system on the 
                
        * 


### Configure the VM to act as a Jupyter Lab notebook server supporting a Python 3 kernel


### Add other useful tools / libraries / packages / datasets / etcetera to this VM


* Let's install `cmocean`
* Let's add a dataset
* Let's install a Jupyter notebook repository


### Use cloud management tools to create an ***image*** of this VM

* Make sure to record the name and location of the image so as to make it findable in two years
### Terminate / delete / destroy the VM so they are not paying for it by the hour
