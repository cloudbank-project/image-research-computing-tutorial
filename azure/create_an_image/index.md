# Creating a VM image: Azure cloud

## Introduction

Virtual Machines (VMs) are self-contained computers; also called ***instances***. On the cloud 
an ***instance type*** means a VM with a set of specifications: How much CPU power, memory, storage, and 
networking speed. Based on these specs we pay at some rate for a VM 'per hour' until we **Stop** it.


A VM is distinct from a ***container***: A container makes use of a computer's underlying operating system; 
so it starts much faster. It is more of a (substantial) program running on a computer. 


A VM is of course also distinct from a serverless function, which is a managed service. The VM is 
provided to us provisioned with an operating system where we are the root user; 
so this is far from the notion of 'managed'. 


A single physical computer may host more than one Virtual Machine.  


On the cloud we select a VM by choosing both an ***instance type*** and an ***operating system***. The type
matches the computer's purpose to processing power, memory, network speed and other features. A bigger
VM costs more per hour on the cloud, be it Azure or AWS or GCP or some other platform.


Technical detail: The operating system *actually* selects a pre-built *image* which includes
that operating system. So the term *image* used here is the same term *image* we are
working towards later on. The image we select loads into the VM as a *blank slate*: Just the operating system, 
an empty user directory, no additional content. We log in to this generic VM as a generic user 
and continue from there. (Let's try and log in from the VSCode terminal.)


The VM we use costs $0.12 per hour or $14 per day; so this leads us to a first
rule of cloud VMs: Stop the VM when it is not in use. This is like turning off a laptop: On
a stopped VM everything persists;
but it is no longer incurring hourly charges. **Start** and **Stop** for a VM
are distinct from **Terminate**. When we terminate a VM it evaporates; it is gone. 


#### VM Concept and Plan including Jupyter Notebook servers


A Jupyter notebook server is a research tool in common use at this time. It is a popular component 
of the research ecosystem. We will include one of these in this construction process. 


A researcher installs and runs a Jupyter notebook server on a VM as a step in building
a research workspace. Commonly the programming language in use in these notebooks is 
Python. 


Python also features a level of virtualization in what are called *virtual environments*. 
The Python *base* environment is the Python interpreter and libraries that comprise the
basic Python installation. This base environment is a distinct concept from the Jupyter
notebook server. From this base or default environment a Python (virtual) environment 
is often built to further customize the workspace. A virtual environment is an isolated 
space in which additional libraries are installed.


To connect this together as a narrative we can say that we...

* ...choose a particular cloud (Azure) on which to build a research space
* ...identify and start up a Virtual Machine (VM) with an associated hourly cost
* ...log in to this VM using credentials we received in the start-up process
* ...install Python and a Jupyter Notebook server
* ...create a virtual Python environment and in that context install additional Python libraries
* ...import some test data to the VM file system from object storage (see below)
* ...import some test code to the VM file system from GitHub (see below)
* ...execute Python code found in *cells* of the Jupyter notebooks


#### Object storage versus block storage

A "disk drive" or "storage drive" is a fairly large block of read/write memory associated directly with 
a file system on a computer. On a cloud VM this is known as block storage and it includes two sub-types. 
First there is the root file system that includes the VM operating system, in our case Ubuntu Linux. 
Second there can be additional attached block storage devices often called data drives. These will tend 
to feature larger storage capacity. 

The cloud also features a second type of storage called object storage. On Azure this is called *blob* 
storage. Objects in object storage may be files. However they are not treated as files that can be 
opened and read through (indexed) in search of some particular segment of information. This is in contrast
to block storage where they can. Object storage or blob storage does support reasonably high connectivity speed; 
so object storage is an extension of the computing environment. ***Object Storage...***

- ...is by design virtually infinite in capacity
- ...is cheaper per byte per month than block storage
- ...features fast connection speed but not as fast as block storage attached to a VM
- ...allows us to read an object (say a file) directly into computer memory
- ...allows us to copy an object (say a file) to block storage
- ...does not allow us to "open and access" the contents of an object (say a file)
- ...blobs can be files, collections of files, entire folder trees, entire block storage file systems, or entire VMs


In terms of cloud computing design patterns: Object storage is a cost-effective way of storing data.
*Specifically* data that need not be accessed immediately; but has some future intended use.


Technical note: There are cheaper forms of object storage as well that presume very low data access rates.
These are used for archival data storage and are sometimes called 'cold storage*.


#### GitHub


GitHub is a provider of Internet hosting for software development and version control using **`git`**. 
(source: Wikipedia) **`git`** is in turn a Linux software version control utility. GitHub and similar hosting
sites are in common use as a means of sharing software solutions particular to open and reproducible research.
There are in consequence two important aspects of GitHub use relevant to use of public clouds like Azure. 


1. It is common practice to use the **`git`** command to clone GitHub *repositories*, which are 
thematic collections of files contained in a directory structure. However there are a number of
details in learning to use **`git`** effectively: This means there is a proper and necessary 
**`git`** learning curve.


2. Improper use of GitHub can result in cloud access keys landing in an open repository. There are malevolent 
code bots in operation on GitHub that will use such inadvertently open keys to mine bitcoin on cloud VMs. This
costs actual money that is charged to the cloud User. Typical spend rates for this scenario are USD 15,000 per 
hour; so it is important to avoid committing access keys to GitHub repositories.



#### Plan


"As if we are doing some research" the sequence of events in this walk-through are:


- Start a VM on the Azure cloud
- Install the Anaconda data science platform (Python with Jupyter notebook server support)
- Install some additional Python libraries
- Download some data
- Download a Jupyter notebook repository
- Make sure everything works properly
- Save the results as an Azure VM **image**
- Shut down ("terminate") the VM
- Start a completely new VM from the stored image
- Again test that everything works properly


Note: There is a distinction between a virtualized computer or virtual machine (VM) and an actual physical 
computer. While this is outside the scope of this material it can be relevant within the general topic of 
compute optimization.


## Background perspective


There are three degrees of complexity in building a compute resource on the cloud. Actually there are
more than this but let's start with three: **Functions**, **Containers** and **Images**. Functions are simplest
but they have a limited degree of power and flexibility. Images are the most complicated; they correspond
to Virtual Machines. In fact Images are like a freeze-dried (or if you like 'zip file') version of a VM.
Containers occupy a middle ground between Functions and Images. We are visiting all three with the idea 
of seeing a spectrum of options for doing data science on the cloud. 


## Walk-through

This procedural walks through creating a Virtual Machine, adding a Jupyter notebook server, testing this, 
and storing it as an Azure machine image. That last step is a single click task; the bulk of the effort is 
preparation.


* On a browser sign in to the [Azure portal](portal.azure.com) and verify your Subscription
    * Be sure to work in the Central US Azure region
    * Select or create an appropriate *Resource Group*
        * A Resource Group (abbreviated RG) is a logical/virtual container for associated Azure resources
        * A Resource Group might contain a Virtual Machine (VM), a monitoring service and a Storage Account
* Below is a portal screencapture showing a Resource Group list: Just one Resource Group is present
    * This Resource Group will contain our Virtual Machine and associated resources: That's the goal.

<BR><BR>

Disabled image HTML key:

less than

img src="../../images/azure/Azure_image_01.png" alt="drawing" width="600" style="display: block; margin: auto;"
   
forward slash greater than


<BR><BR>

* From the Resource Group overview click `+Create` (The image below shows `+Add`: Same thing.)
   * Select 'Virtual machine' (directly: click the icon; or use the search bar)
   
Note: We can use the Azure **Marketplace** to browse for VM images by operating 
system and based on other features. As a stretch activity you can spend some time
looking around at what is available.


<BR><BR>


<BR><BR>

* Use the VM wizard to customize the VM; use defaults but note the following:
   * Name the VM something like `YourNetIDvm`
   * Region = (US) Central US
   * Image = Ubuntu Server 20.04 LTS - Gen 2 (or more recent)
       * "LTS" means Long Term Support, i.e. the OS will be supported by Ubuntu for a "long time"
   * Size = Standard_D2as_v5 - 2 vcpus, 8 GB memory
   * Ensure Public inbound ports = Allow selected ports
   * Ensure Select inbound ports = SSH (22)

<BR><BR>


<BR><BR>


<BR><BR>
   
* Skip forward to the Management tab
   * Enable auto-shutdown
       * Keep the shutdown time as 7PM
       * Change the Time zone to Pacific Time
   

<BR><BR>

* Skip forward to the **Tags** tab
    * Include some tags to inform your future self what this VM is for

<BR><BR>


<BR><BR>

* At the **Review and Create** tab: Review the description
    * This VM will cost about $0.10 per hour
    * Click the Create button
        * This will prompt you to download a key file
        * Download this file to a safe location on your computer

* Once the create action is complete: Click 'Go to resource'
    * At the top of the central / main window notice there is a sequence of utility buttons
        * These are Connect, Start, Restart, Stop, Capture and so on
    * From the (default) Overview: Notice that the VM has a tabbed sequence of information pages
        * These are Properties, Monitoring, Capabilities, Recommendations and Tutorials
        * Look through these tabs to get a sense of what is there
    * On the left menu bar under Settings click on Disks
        * Note that the VM has an operating system disk with a 30 GiB capacity
        * Some of this will be used by the operating system
    * On the left menu bar under Automation click on Export template
        * The resources here enable you to build this same VM automatically from code (rather than manually)
    * On the left menu bar under Settings click on Connect
        * Note that this provides you with a four-step recipe for logging in to this VM
        * It is time to log in to the VM


   
* Open Visual Studio and activate the Terminal window (ctrl + ` or on a Mac...?)
* In the terminal verify that `ssh` works by typing it in and hitting return. Should produce a usage message.
* Go to the home directory using `cd ~`
* Move the `.pem` file downloaded during the VM Create to this directory
    * For example `mv /mnt/c/Users/myusername/Downloads/rob5vm_key.pem .`
* Change the permissions of this file to be "user read only" using the somewhat cryptic `chmod` command
    * `chmod 400 rob5vm_key.pem`
* Type in or paste in the `ssh` connection command
    * You can copy this command to your clipboard from the Azure portal **Connect** page identified above
    * Make sure to use the correct path to the `.pem` file
    * Make sure to keep the username `azureuser`
    * Make sure to copy the provided ip address. Below I use `27.173.147.19`
   
```
ssh -i ./rob5vm_key.pem azureuser@21.173.147.19`
```

* During the connection we get a message "The authenticity of host ... can't be established. Are you sure?"
    * Enter 'yes'
    * You should now see a welcome message and a `bash` prompt
        * We used `ssh` to connect but once logged in we are running the `bash` shell
        * To verify this type in `ps -p $$`
    * Do up update / upgrade of your operating system by entering these commands in sequence
        * `sudo apt update`
        * `sudo apt upgrade`
    * Examine the operating system disk 
        * `df`
        * The output tells us we are on a 30GiB root drive, 7% is in use; so 28GiB available

* Now that we are logged in to a VM let's determine whether or not it has Python installed
    * Enter `python3`
        * This should tell you which version of Python you are running... (for me this is Python 3.8.10)
        * ...and it should change the prompt to the Python interpreter `>>> `
        * Stretch activity: If your factoring Azure Function is still running...
            * ...try entering the three lines of Python in the Python interpreter and factor a number!
            * (If your Azure function is no more you can use this one...)
                * `https://rob5azfn02.azurewebsites.net/api/HttpTrigger1`
        * Use ctrl-d to exit the Python interpreter
    * If for some reason you needed to install Python the installation command is something like this:
        * `sudo apt install python3-pip python3-dev`

* We have determined that Python 3 is installed on the VM already, so far so good.
* Is the `jupyter` notebook server installed? Enter `jupyter` to find out (it is not)
    * We must have a Jupyter notebook server so we can do data science!
    * Search 'how to install jupyter on ubuntu' or simply click on
        * [this link](https://www.digitalocean.com/community/tutorials/how-to-set-up-jupyter-notebook-with-python-3-on-ubuntu-18-04)
    * Enter this sequence of nine-or-so commands to install jupyter
    * Test by typing `jupyter`: The VM should now recognize and run this command

We have now reached a point where it would be nice to have some code and data to work with. 
A simple way to get this is to clone some content from the GitHub software management website:
   
```
cd ~
git clone https://github.com/robfatland/ocean
```
   
This should complete in under a minute. You can use `ls` to show there is a new directory called `ocean`. 
It contains data and an IPython notebook called `BioOptics.ipynb`.
   
Normally we run Jupyter notebook servers in a browser window. This will be no exception; but it takes
a little preparation. Issue these two commands: 
   
```





```



* You should now have a tell-tale prompt: `azureuser@machimename:~$ `
    * The primary cause of this step not working is the `0400` permission for the `.pem` file was not set (see above).


## Optional: Mount data drive

In the VM configuration we added a data drive. During class we suggest skipping the next step of mounting this 
drive for use. If you are familiar with the process it should be straightforward. If not we recommend doing this
outside of class time as a stretch activity. 


* Next step is to mount the data disk we added during the VM creation process 
    * This was not done automatically
    * The command to get started is `lsblk -o NAME,HCTL,SIZE,MOUNTPOINT`
    * Notice that the 256 GB disk is listed as `sdc` but it has no mount point as yet


<BR><BR>


<img src="../../images/azure/Azure_image_18.png" alt="drawing" width="600"/>


> Please notice that the 256GB drive (or 32GB drive) in the above is associated with the NAME `sdc`. 
> This may instead by `sdb` so please note which one for use in the following mount commands.


***Completely Optional Aside***: Don't do this if you have limited time available! 
Here is a *green text on a black background customization [link](https://robfatland.github.io/greenandblack/)*. 


<BR><BR>


* Follow directions for mounting a disk on an Azure VM
    * [How-to documentation](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/attach-disk-portal)
    * Based on this the following commands were used to mount the data disk
    * Notice this is for **`NAME=sdc`**. You must modify these commands if **`NAME=sdb`** per above.


```
sudo parted /dev/sdc --script mklabel gpt mkpart xfspart xfs 0% 100%
sudo mkfs.xfs /dev/sc1
sudo partprobe /dev/sdc1
sudo mkdir /datadrive
sudo mount /dev/sdc1 /datadrive
sudo blkid
```

* You can verify the mount by re-running `lsblk`: A mount point should be present now.
* Change directories to the data disk and ensure you can edit, save and modify a test file.
    * You may need to `sudo chmod` the `/datadrive` to make it read/write-able.
* Note the documentation on all of this provides links to additional documentation.

<BR><BR>
   
<img src="../../images/azure/Azure_image_20.png" alt="drawing" width="600"/> <BR><BR>
<img src="../../images/azure/Azure_image_19.png" alt="drawing" width="1000"/>
   

<BR><BR>


# Re-starting a VM

If your earlier session was interrupted and your Virtual Machine was set to auto-halt
every day at 7PM it may currently be Stopped. It can be restarted easily:
in the Azure portal: Find the Resource Group and therein the Virtual Machine.
Select the Virtual Machine and click the **Start** button at the top of the center panel.
Note the new ip address for the VM.


# Customizing the environment

We have a working VM; so let us now import code and data, install some Python libraries,
and run a Jupyter notebook server in our hypothetical quest to become oceanographers.


In what follows, commands are run on the Azure VM bash command line. 
Above: We logged into the Azure VM from a local computer like a laptop using a VSCode terminal.
Once the Jupyter notebook server is running in `--no-display` mode: We return to our local computer 
to set up an ***ssh tunnel*** and use this to connect our local browser to the Azure VM Jupyter 
notebook server. 


* Customizing the Virtual Machine is five steps
    * Install the Anaconda Python platform: Choose the current Linux package from [this page](https://www.anaconda.com/products/individual).
    * Install the `xarray` library
    * Install the `netcdf4` library
    * `git clone` a GitHub repository { Jupyter notebooks, small dataset }
    * Start the Jupyter notebook server in `--no-browser` mode


### Install Anaconda

The following example uses the correct link as of April 7 2021:


```
wget https://repo.anaconda.com/archive/Anaconda3-2020.11-Linux-x86_64.sh
bash Anaconda3-2020.11-Linux-x86_64.sh
```

* This gets the Anaconda installation file using the `wget` command; then installs it using `bash`
    * You can also opt to install a lighter-weight version called Miniconda 
* Accept the license agreement and the defaults, initializer etc; install takes a couple of minutes

<BR><BR>

<img src="../../images/azure/Azure_image_21.png" alt="drawing" width="1000"/>

<BR><BR>

<img src="../../images/azure/Azure_image_22.png" alt="drawing" width="600"/>

<BR><BR>
   
* Definitely log out and then log back in to your VM for "changes to take effect".

<BR><BR>

<img src="../../images/azure/Azure_image_23.png" alt="drawing" width="600"/>

<BR><BR>
   


At this point the Jupyter notebook server should be available for use


### Install two Python libraries

```
conda install xarray 
pip install netcdf4
```


### Clone a GitHub repository

```
cd ~
git clone https://github.com/robfatland/ocean
ls ocean
```

This creates a folder (repository) called `ocean` in your home directory.


If you ran this `git clone` before today (Wednesday 28-APR-2021) you may 
wish to refresh your copy of the repository: 

```
cd ~
pwd
rm -rf ocean
git clone https://github.com/robfatland/ocean
ls ocean
```

Note the use of the `-rf` switches in the `rm` command causes a recursive deletion. If used in the wrong place it can
delete entire file systems; so use this with caution.


### Start the Jupyter notebook server

```
(jupyter notebook --no-browser --port=8889) &
```

The ampersand `&` at the end of this command starts the Jupyter notebook server as a background job.
Notice that the Jupyter notebook server will "listen" on port 8889.
You can log out of the Azure VM but first copy the token string for the Jupyter notebook server.
It looks like this: `token=ae948dc6923848982349fbc48a2938d4958f23409eea427`


## Test the Jupyter notebook server from your local browser


* On your local computer/laptop bash command line run



```
ssh -N -f -i fu.pem -L localhost:7005:localhost:8889 azureuser@111.22.33.44
```

In this command: Make the appropriate substitutions for your `.pem` filename and your VM ip address.
This creates a secure (**`ssh`**) tunnel from port 7005 of your local computer to port 8889 of the Azure VM.

In the browser address bar enter `localhost:7005`
When prompted: Enter the token string you copied above
On success: The Jupyter notebook server will appear in your browser
Navigate to the `ocean` folder/repository and run the first notebook listed.  
   
   
<BR><BR>

<img src="../../images/azure/Azure_image_24.png" alt="drawing" width="800"/>

<BR><BR>

## Create a machine image in the Azure portal

* Select the VM in the Azure Portal and click **Capture**

<BR><BR>

<img src="../../images/azure/Azure_image_25.png" alt="drawing" width="600"/>

<BR><BR>

* The "make image" wizard comes up.
* On the side dialog select No, capture only a managed image

<BR><BR>

<img src="../../images/azure/Azure_image_26.png" alt="drawing" width="600"/>


<BR><BR>
   
* The VM stops and the image creation process starts up. 
* Shortly thereafter (minutes) we have an image of the VM available. 
    * This VM image can be restarted on small low-cost machines or large high-cost machines
    * It can be shared with colleagues or made publicly available
    * The image is static. If you make changes to the VM you must re-Capture the image to keep it up to date if you so desire


<BR><BR>

<img src="../../images/azure/Azure_image_27.png" alt="drawing" width="600"/>


* Try starting the notebook **Ocean 01 A etc** and running the first few cells.

## Concluding remarks


The Virtual Machine configuration took up the bulk of this effort. The VM *image* was a rather
trivial final step. This image is a "safe backup" of the VM. 


* Leaving a VM running when not in use is very common practice, also expensive. 


* We access VMs as shown here over the internet using the `fu.pem` keypair file. This file
should be kept in a secure location away from GitHub respository directories. 

* A backup copy of a keypair file stored in another 
secure location might come in handy. Azure has a security service for managing access keys called 
**Key Vault**; worth knowing about but beyond the scope of this tutorial.


* In this walk-through we created a data disk. These attached drives cost $0.10 / GB / month, approximately.


* A running Virtual Machine has an ip address. These may be fixed or permanent; or they may change 
each time the VM is re-started. The former is more convenient; see Azure Static Public IPs for more on this.


