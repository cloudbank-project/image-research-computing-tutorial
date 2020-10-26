# Bootstrapping a machine image

## Introduction: What is this sub-folder about? 

This is a tutorial for creating the image referenced on the main page of this repository. It is a ***prequel*** to the main tutorial.
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


The above five steps are the bootstrapping covered on this page. The image created here does not precisely match the one 
started on the main page. This one includes some additional data disks making it more expensive to operate. To avoid this
(cost of 200 GB of disk space, $20/month) choose a smaller / cheaper instance.


## Summary of Tutorial

This section describes what the subsequent **Tutorial** covers: Before we dig into the many details. 


* You will set aside an available computer on the cloud for your sole use.
    * This computer begins as a blank slate: Just an operating system and a user login.
    * We use Ubuntu Linux in our example. 
* You log in to this computer and begin customizing it
    * Install Jupyter Lab, you import code, you import data, you install additional software, etcetera
* You "save" this customized version of your computer on the cloud as an *image*
* You halt and delete the customized computer: Your goal was just to create the image
    * You don't need the actual computer at the moment
    * In the future you or someone else will reconstitute the computer from the image you created


## Tutorial

### Key concepts

Before beginning let's review some important concepts to have firmly in mind

* Authentication
    * Pardon us belaboring this but in some cases this can save you from weeks of wasted time and aggravation.
    * *Authentication* means "logging in", i.e. you are authenticated to use resources on the cloud
    * On the cloud there are (at least) three authentication paths: Username/passord, login keypairs and IAM Access Keys
    * Authentication is generally **credential** use over a **secure connection**
        * The **secure connection** is built into applications like **ssh** as a protocol: You don't need to worry about this; it just works.
        * The **credential** is sometimes a file containing long text strings akin to passwords: You need to worry about these not falling into unintended hands.
    * The reason to be aware of and vigiliant about your authentication credentials is as follows...
        * Suppose you have AWS credentials in the form of an Access Key file
        * Suppose you place these on a *Virtual Machine* (see below) so that you can use them to access data storage
        * Suppose you then make an *image* of this Virtual Machine (see below)
        * Suppose that this image is shared with other people, perhaps unintentionally with the wrong people
        * What could go wrong? 
            * An inimical person (or their bot agent) finds this Access Key
            * They use it 'as you' to spin up a vast pool of resources on AWS costing hundreds of dollars per minute
            * These resources incur charges that *you* are expected to pay for
    * ***Bottom line: Do not place credentials on AWS Virtual Machines intended to become EC2 images per this tutorial.***


* Virtual Machines (VM / instance / EC2)
    * On AWS a Virtual Machine or VM is also called an *instance*.
    * The instance service is called EC2. Hence 'EC2 instance' is an AWS VM. 
    * There are a wide variety of instance types available for us to choose from.
        * Any given instance type will have associated memory, processing power and storage (see below).
        * The cost of the instance -- per hour -- varies with these characteristics.
        * On the AWS console you select the EC2 service and then Launch Instance
            * This runs a launch wizard where your first step is to choose an operating system.
            * The second step is to choose an instance *type* which gives you a short name for the type of VM you will be launching
            * You should have a good idea of what this <instance-type> will cost per hour of operation
            * It can be challenging to find instance cost on the AWS console
                * Costing shortcut: Enter `cost <instance-type> <region>` in a browser search bar; for example:
                    * `cost m5ad.4xlarge oregon` shows $1.00 per hour as the first result


* Storage
    * Disk drive version 1: Elastic Block Storage (EBS) is persistent storage, acting as a disk drive + file system
    * AWS also supports temporary disk storage through the [*instance store*](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html)
    * It is essential to distinguish between **Elastic Block Strage (EBS) volumes** and **(ephemeral) instance store volumes**
        * Instance store volumes evaporate; so they are only useful for temporary storage. 
        * If we Stop and then Start an image: The instance store file system will still be there but all files are gone. 
    * To summarize
        * Use EC2 instance store volumes for temporary storage where you do not need to keep data
        * Use EC2 EBS block storage as persistent disk space when you need to keep data
        * Use AWS EFS (Elastic File System) as storage shared by multiple instances; analogous to UNIX NFS
        * Use AWS S3 Object Storage to store any (large) amounts of data cheaply, independent of EC2 resources


### Start a Virtual Machine (VM) and configure it


#### Start up a VM from the AWS console


* Log on to the AWS console and select Services > Compute > EC2 > Launch Instance
    * This could also be done using the Command Line Interface (CLI) to AWS
    * This could also be done using the AWS API through a package such as `boto3`
* Run through the launch Wizard; here are some details from our example run
    * The image selected will be 64 but x86 Ubuntu Server (username = `ubuntu`)
        * Notice that this is a "bare machine" with just the Ubuntu operating system
    * The VM selected is an `m5ad.4xlarge` which $1.00 / hour in Oregon at time of writing
        * This instance type includes two 300 GB instance store volumes (temporary storage)
            * Data on these volumes will vanish on stops, terminations, or hardware failures
        * ***We strongly recommend following through this tutorial to the Terminate stage!!!***
            * Failure to do so may result in you being charged for this Virtual Machine at this rate.
            * We refer to this as zombie resource charges: You may forget about it but the cloud provider will not.
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

#### From my own computer log on to the AWS EC2 instance

* Relocate the downloaded keypair file to my `bash` home directory
* `chmod 400` applied to keypair file
* `my computer> ssh -i keypair.pem ubuntu@12.23.34.45`


#### On the EC2 instance (VM) mount any added storage drives


>Intermezzo: If like me you use `vi` or `vim` as a text editor and are puzzled at the profusion of colorized text: You can easily disable this "feature". 
To do so in the editor: In escape mode type `:syntax off`. To do so permanently append the text line `syntax off` to the 
file `~/.vimrc` (which you may be obliged to create). 


* Assumes you are logged on with a bash shell
* This example works with temporary *instance store* volumes; will also work with EBS volumes
* Search engine: 'AWS EC2 EBS mount` gives [this instructive link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)
* `lsblk` produces a device listing
    * For this `m5ad.4xlarge` instance we see three devices `nvme0n1`, `nvme1n1`, `nvme2n1` of type `disk`
        * Full path is in fact `/dev/nvme0n1` and so on
            * Only the third volume `nvme2n1` has an associated partition
            * This is the root device where the operating system and ubuntu home directory resides
            * The remaining two disks `nvme0n1` and `nvme1n1` are the empty instance store volumes; must be mounted
        * Verify that there is no file system on the empty volumes
            * `sudo file -s /dev/nvme1n1` --> `/dev/nvme1n1: data` (The result `data` indicates nothing there.)
        * ***WARNING!!! The following command wipes a file system. Any data will be lost. ***
            * `sudo mkfs -t xfs /dev/nvme0n1` and likewise for `nvme1n1`
                * This should print out some confirmation stats
                * If `mkfs.xfs` is `not found`: Search on installing it using `sudo yum install xfsprogs`
            * `sudo file -s /dev/nvme0n1` should now show an XFS filesystem
            * Create a data directory for each disk
                * `sudo mkdir /data` and `sudo mkdir /data1`
                * `sudo mount /dev/nvme0n1 /data` and `sudo mount /dev/nvme1n1 /data1`
            * You will find that root owns these data directories. By default nobody else including the `ubuntu` user can write to them.
                * It is a security risk to make a data directory world-writable
                * The command to make a data directory world-writable is `sudo chmod a+rwx /data`
                * I use this without any qualms; but please be aware that it is a security-relevant choice
            * Test the data directories by `cd /data` and creating a new file

#### Set up these storage disks to auto-mount on reboot

* Backup copy: `sudo cp /etc/fstab /etc/fstab.orig`
* Get the UUID: `sudo lsblk -o +UUID` 
* Edit the `fstab` file: `sudo vim /etc/fstab`
* Add this entry to the file: `UUID=aebf131c-6957-451e-8d34-ec978d9581ae  /data  xfs  defaults,nofail  0  2`
* Test it out: 
    * `sudo umount /data`
    * `sudo mount -a`
* Refer to the link given above for more detail on this procedure. A broken `fstab` file can prevent the VM from booting.

#### Install the Jupyter Lab notebook serve
* Install Anaconda
    * search `install Anaconda Linux` and follow the instructions
    * Once the download path is determined I used `wget` to download the installer on the VM
        * `wget https://repo.anaconda.com/archive/Anaconda3-2020.07-Linux-x86_64.sh`
    * On Ubuntu I installed in the `/home/ubuntu` directory and needed to add the path for `conda`
        * `export PATH=~/anaconda3/bin:$PATH`
    * Once Anaconda is installed: use the `conda` package manager to install Jupyterlab
    * `conda install -c conda-forge jupyterlab`
* Test this using the `ssh tunnel` described in the main page tutorial of this repository

#### Configure the machine for research

* Install software packages
    * The following are installation commands by way of example (geoscience-oriented)
        * `conda install xarray`
        * `conda install -c conda-forge cmocean`
        * `conda install boto` was apparently already done...
        * `conda install netcdf4`
        * `conda install -c conda-forge ffmpeg`
        * `conda install networkx`
        * `pip install git+https://github.com/cormorack/yodapy.git`
        * `pip install utm`
        * `pip3 install manimlib` is Grant Sanderson's explanatory math visualization library
        * `sudo apt install nodejs` working towards widgets...
    * Import datasets, typically to data volumes
        * Example approach: Use `sftp -r` from the data directory of a source computer
    * Imported code repositories (for example from GitHub) into the ubuntu user home directory
    * Generate `requirements.txt` or `environment.yml`; look into `pip freeze` for example

#### Create an image (AMI) of this Virtual Machine
* On the AWS console: EC2 'running instances table: Locate and select this instance
    * Actions menu > Image > Create Image
    * Be sure to attach extensive metadata (typically add Tags) to the AMI to make it recognizable

## Share the AMI with other AWS accounts
    
* On the AWS console > AMI listing (see left sidebar) > Permissions editor
* Add the AWS 12-digit account of a destination account where you wish this AMI to be available

## Terminate the VM so as not to continue paying for it

* ***WARNING: This will completely delete this EC2 instance. To mitigate concern...***
    * Consider starting a new EC2 instance using the AMI you created above.
    * You can verify everything is preserved as you expect in this new instance
    * At this point the AMI has been demonstrated as correct and you can Terminate both EC2 instances
* On the AWS console > EC2 'Running Instances' table: Locate and select an instance to Terminate
* Actions menu > Instance State > Terminate and confirm


## Updating Anaconda and refreshing the machine image

* Once per month is a common Anaconda update tempo
* Consider updating the operating system as well, for example using `sudo yum update`
* Upon doing updates: Create a new AMI; and manage your AMI catalog to avoid zombie charges


## Remaining open topics

* Linking to M's content
* Do not re-start a running Jupyterlab server. Check for it with `ps -ef | grep jupyter` is helpful.
    * If the local machine disconnects it is usually sufficient to re-issue the ssh tunnel command from your computer
* Enabling widgets is a confusing detail...

```
jupyter nbextension enable --py widgetsnbextension
```

* Making mpeg videos from chart sequences
