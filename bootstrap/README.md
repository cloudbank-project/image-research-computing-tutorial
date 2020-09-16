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
(cost of 600 GB of disk space) choose a smaller / cheaper instance. 


## Tutorial

Before beginning let's review some important concepts to have firmly in mind

* Virtual Machines (VM / instance / EC2)
    * On AWS a Virtual Machine or VM is also called an *instance*.
    * The instance service is called EC2. Hence 'EC2 instance' is an AWS virtual machine. 
    * There are a wide variety of instance types available for us to choose from.
        * Any given instance type will have associated memory, processing power and storage (see below).
        * The cost of the instance -- per hour -- varies with these characteristics.
        * It can be challenging to find instance cost on the AWS console.
            * Costing shortcut: Enter `cost <instance-type> <region>` in a browser search bar; for example:
                * `cost m5ad.4xlarge oregon` shows $1.00 per hour as the first result
* Storage
    * Disk drive version 1: Elastic Block Storage (EBS) is persistent storage, acting as a disk drive + file system
    * AWS also supports temporary disk storage through the [*instance store*](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html)
    * It is essential to distinguish between **Elastic Block Strage (EBS) volumes** and **(ephemeral) instance store volumes**
        * Instance store volumes evaporate; so they are only useful for temporary storage. 
        * If we Stop and then Start an image: The instance store file system will still be there but all data/files are gone. 
    * Use AWS S3 Object Storage to store any amount of data completely independent from EC2 instances
    * Use AWS EFS as storage shared by multiple instances

### Start a Virtual Machine (VM) and log in to it

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
* On my own computer
    * Relocate the downloaded keypair file to my `bash` home directory
    * `chmod 400` applied to keypair file
    * `my computer> ssh -i keypair.pem ubuntu@12.23.34.45`
* On the VM 
    * (This follows from the `ssh` command issued from a terminal window on my computer, last command above)
    * ***Mount the two 300 GB SSD 'EBS' drives for use***
        * Search engine: 'AWS EC2 EBS mount` turns up [this instructive link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)
        * `lsblk` produces a device listing
            * In this case (again with an `m5ad.4xlarge` we see three devices `nvme0n1`, `nvme1n1`, `nvme2n1` of type `disk`
                * These would have a full path `/dev/nvme0n1` etcetera
                * Of these three only the third one `nvme2n1` has an associated partition
                    * This is the root device where the operating system resides
                    * The remaining two disks `nvme0n1` and `nvme1n1` are the empty volumes; these must be mounted for use
                * Verify that there is no file system on the empty volumes: `sudo file -s /dev/nvme1n1` --> `/dev/nvme1n1: data`
                * ***WARNING!!! If you use the following command on a file system that has data on it: You will wipe that data out.***
                    * `sudo mkfs -t xfs /dev/nvme0n1` and likewise for `nvme1n1`
                        * This should print out some confirmational stats
                        * If `mkfs.xfs` is *not found*: Do a search on installing it using `sudo yum install xfsprogs`
                    * `sudo file -s /dev/nvme0n1` should now show an XFS filesystem
                    * Create a data directory for each disk
                        * `sudo mkdir /data` and `sudo mkdir /data1`
                        * `sudo mount /dev/nvme0n1 /data` and `sudo mount /dev/nvme1n1 /data1`
                    * You will find that root owns these data directories. By default nobody else including the `ubuntu` user can write to them.
                        * It is a security risk to make a data directory world-writable
                        * The command to make a data directory world-writable is `sudo chmod a+rwx /data`
                        * I use this without any qualms; but please be aware that it is a security-relevant choice
                    * Test the data directories by `cd /data` and creating a new file
    * Set up these data directories to mount automatically when the instance reboots
        * Backup copy: `sudo cp /etc/fstab /etc/fstab.orig`
        * Get the UUID: `sudo lsblk -o +UUID` 
        * Edit the `fstab` file: `sudo vim /etc/fstab`
        * Add this entry to the file: `UUID=aebf131c-6957-451e-8d34-ec978d9581ae  /data  xfs  defaults,nofail  0  2`
        * Test it out: 
            * `sudo umount /data`
            * `sudo mount -a`
        * Refer to the link given above for more detail on this procedure. A broken `fstab` file can prevent the VM from booting.

     * Install the Jupyter Lab notebook server
         * Install Anaconda
             * search `install Anaconda Linux` and follow the instructions
             * Once the download path is determined I used `wget` to download the installer on the VM
                 * `wget https://repo.anaconda.com/archive/Anaconda3-2020.07-Linux-x86_64.sh`
             * On Ubuntu I installed in the `/home/ubuntu` directory and needed to add the path for `conda`
                 * `export PATH=~/anaconda3/bin:$PATH`
         * Once Anaconda is installed: use the `conda` package manager to install Jupyterlab
             * `conda install -c conda-forge jupyterlab`
         * Test this using the `ssh tunnel` described in the main page tutorial of this repository



### What else did I do / want to do


* `conda install xarray`
* `conda install -c conda-forge cmocean`
* `conda install boto` was apparently already done...
* `conda install netcdf4`
* `conda install -c conda-forge ffmpeg`
* `conda install networkx`
* `pip install git+https://github.com/cormorack/yodapy.git`
* `pip install utm`

* Imported datasets to the `/data` file system (300 GB capacity)
    * The quick-and-easy approach is to use `sftp -r` from the data directory in the Pangeo JupyterHub pod
        * Three glodap files from an S3 bucket to /data/glodap
        * hydrophone data in /data/hydrophone
        * argo data in /data/argo
        * rca data in /data/rca
* Imported repositories into ~
    * chlorophyll
    * golive
* NEED: generated requirements.txt files inside repos: Look into `pip freeze`


### Use cloud management tools to create an ***image*** of this VM


* Make sure to record the name and location of the image so as to make it findable in two years


### Terminate / delete / destroy the VM so they are not paying for it by the hour


## Updating Anaconda and the machine image

* Once per month is a common Anaconda update tempo...
* Operating system: `sudo yum update` and so on


## When Does What Happen?

* Do not re-start a running Jupyterlab server: It can't possibly turn out well... `ps -ef | grep jupyter` is helpful.

* Enabling widgets is a confusing detail...

```
jupyter nbextension enable --py widgetsnbextension
```
