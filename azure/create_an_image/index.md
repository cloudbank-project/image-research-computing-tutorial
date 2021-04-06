# Creating a VM image on the Azure cloud

This procedural walks you through creating a working Virtual Machine with a Jupyter notebook server, testing it, 
and storing it as an Azure machine image. This last step is a single click task; the bulk of the effort is 
the preparation. This walk-through uses the [Azure portal](portal.azure.com), a browser-based interface to
the services available on the Azure cloud. 


## Walk-through

* Sign in to the [Azure portal](portal.azure.com) and find or create a *Resource Group* (RG) in a region nearest to you.
* Here is a portal screencapture showing a Resource Group listing with just one Resource Group.
* This RG will act as a container for the Virtual Machine and associated resources we put together in this walk-through.

<BR><BR>

<img src="../../images/azure/Azure_image_01.png" alt="drawing" width="600" style="display: block; margin: auto;"/>


<BR><BR>

* Select the RG and click `+Add v`: We proceed to add a Virtual Machine.



<BR><BR>


<img src="../../images/azure/Azure_image_02.png" alt="drawing" width="600"/>

<BR><BR>

* The `+Ad v` provides a drop-down. Select `Marketplace`. This is where we can get Ubuntu Linux VM images at not cost.

<BR><BR>

<img src="../../images/azure/Azure_image_03.png" alt="drawing" width="200"/>

<BR><BR>

* Select from the Marketplace an Ubuntu Server image. Notice that an *image* is what we are building. 
    * We start out by selecting an "empty" image that includes nothing more than the Ubuntu operating system.
    * Once the Virtual Machine running Ubuntu is functional we will customize it.

<BR><BR>

<img src="../../images/azure/Azure_image_04.png" alt="drawing" width="200"/>

<BR><BR>

* Click on `Create`; which means "Build a Virtual Machine using this Ubuntu operating system". 

<BR><BR>

<img src="../../images/azure/Azure_image_05.png" alt="drawing" width="600"/>

<BR><BR>

* This brings us to a multi-step VM builder wizard. 
    * We will work through the multiple tabs fairly quickly, mostly using default values
    * On the first tab: Enter a machine name and a Size from the dropdown. 
        * Notice that the monthly cost of a given VM size is shown.
        * The remaining entries in the first wizard tab should be default values.

<BR><BR>

<img src="../../images/azure/Azure_image_06.png" alt="drawing" width="800"/>

<BR><BR>

* Click **Next** to arrive at the **Disks** tab of the VM wizard. 
* Add a 256GB disk to create some data capacity on this VM.

<BR><BR>

<img src="../../images/azure/Azure_image_07.png" alt="drawing" width="800"/>

<BR><BR>

<img src="../../images/azure/Azure_image_08.png" alt="drawing" width="600"/>

<BR><BR>

* Once disk volume is selected you can (**Ok**) confirm you want to **Create a new disk**.

<BR><BR>

<img src="../../images/azure/Azure_image_09.png" alt="drawing" width="600"/>

<BR><BR>


* Next are Networking, Management and Advanced tabs: Defaults are fine. 

<BR><BR>

<img src="../../images/azure/Azure_image_10.png" alt="drawing" width="800"/>

<BR><BR>


<img src="../../images/azure/Azure_image_11.png" alt="drawing" width="800"/>

<BR><BR>

* **Tags** tab: Include some tags so that an account administrator knows what these resources are for.

<BR><BR>


<img src="../../images/azure/Azure_image_12.png" alt="drawing" width="800"/>

<BR><BR>

* At the **Review and Create** tab we can get a sense of what will be built and what it will cost to operate (here 12 cents per hour). 

<BR><BR>


<img src="../../images/azure/Azure_image_13.png" alt="drawing" width="600"/>

<BR><BR>

* A pop-up dialog gives you the opportunity to download the access key-pair file. 
* We use this rather than using a password to log in to this VM. 
* Place it in a secure location that will not accidentally end up copied to GitHub.
* You will need to change its read-write-execute permissions to `r--------` or `0400` in octal.


> For Windows users: You can use this `.pem` file as-is from a locally installed bash shell.
> If instead you would like to use the popular Windows PuTTY free ssh client: Be prepared 
> to do a bit of learning as there is necessary change in file format applied to that `.pem`
> file. We suggest that installing an Ubuntu bash shell is more in the spirit of the 
> target Ubuntu operating system; which in turn works well with Anaconda and Jupyter notebook servers.
  

> For all users: As noted above you must change the permissions of the `.pem` file to `r--------`.
> That is: Only the user can read the file. This is a restrictive step that increases the
> security of your access to the Virtual Machine you are building. 
> Changing the permissions in this manner is done with the command `chmod 400 fu.pem`.
> Once the file is downloaded and its permissions changed: It is used by `ssh` to authenticate 
> logging in to the VM.

<BR><BR>


<img src="../../images/azure/Azure_image_14.png" alt="drawing" width="400"/>

<BR><BR>

* Here is the "Deployment complete" message you should see once the VM has been launched:

<BR><BR>

<img src="../../images/azure/Azure_image_15.png" alt="drawing" width="800"/>

<BR><BR>

* We then see a lot of the details of operation of this newly-created VM:

<BR><BR>

<img src="../../images/azure/Azure_image_16.png" alt="drawing" width="800"/>

<img src="../../images/azure/Azure_image_17.png" alt="drawing" width="800"/>

<BR><BR>

* Note that this information includes an ip address for this VM. 
    * In the example above the ip address is 138.91.145.112
    * For simplicity let's suppose your ip address comes up as `111.22.33.44`
    * Let's also suppose you have named your `.pem` keypair file to be `fu.pem`  
    * Your ssh login command is then:

```
ssh -i fu.pem azureuser@111.22.33.44
```

* Confirm this to login. You should now have a prompt `azureuser@machimename:~$ `
    * The primary cause of this step not working is not setting the `0400` permission correctly on the `.pem` file (see above).


* The next step is to mount the 256GB data disk we added during the VM creation process 
    * This was not done automatically
    * The command to get started is `lsblk -o NAME,HCTL,SIZE,MOUNTPOINT`
    * Notice that the 256 GB disk is listed but has no mount point as yet

<BR><BR>

<img src="../../images/azure/Azure_image_18.png" alt="drawing" width="600"/>

<BR><BR>

* Follow directions for mounting a disk on an Azure VM
    * [How-to documentation](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/attach-disk-portal)
    * Based on this the following commands were used to mount the data disk

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
   
<img src="../../images/azure/Azure_image_20.png" alt="drawing" width="600"/>
<img src="../../images/azure/Azure_image_19.png" alt="drawing" width="1000"/>
<img src="../../images/azure/Azure_image_21.png" alt="drawing" width="1000"/>

<BR><BR>

* At this point we begin customizing the Virtual Machine
* First we install the Anaconda Python package

<BR><BR>

<img src="../../images/azure/Azure_image_22.png" alt="drawing" width="600"/>

<BR><BR>
   
* As noted below you may need to log out of and then log back in to your VM.

<BR><BR>

<img src="../../images/azure/Azure_image_23.png" alt="drawing" width="600"/>

<BR><BR>

* At this point the Jupyter notebook server should be available for use. 
    * We proceed in two steps here
        * First setting up some interesting content
        * Second configuring the Jupyter notebook server to work remotely


* Insert a link here to 'using an image'
   
   
<BR><BR>

<img src="../../images/azure/Azure_image_24.png" alt="drawing" width="800"/>

<BR><BR>

* Finally create an image of this customized VM image
    * Select the VM in the portal and click **Capture**

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


## Concluding remarks


There is a connection between a running Virtual Machine (which costs a certain amount per hour of operation) and the VM image
created here. The VM image acts as a safe backup copy of the VM. 


There are a couple of important things to emphasize about the Virtual Machine. First a Virtual Machine can be "forgotten about";
i.e. it can left running without being in use. This costs perhaps $0.20 per hour; or if it is a powerful machine maybe $1.20 
per hour, arguably not good for the project budget. However this VM can be stopped, for example through the portal interface. 
When a VM is stopped it effectively costs nothing; and it can be re-started in a matter of a few minutes. The VM can also be 
connected to a cloud service that stops it every day at say 7 PM. This is a failsafe cost management technique. Stopping an
already-stopped machine has no effect.


Another important point to keep in mind is that we access VMs through the internet using a `.pem` keypair file. This file
should be kept in a secure location away from GitHub respository directories. It is sometimes possible to accidentally
delete files so it might be wise to keep a backup copy of a keypair file in another secure location. Azure has a security
service for managing access keys called **Key Vault**; worth knowing about but beyond the scope of this tutorial.


In this walk-through we created a 256GB data disk that we mounted as a file system on the VM. There is a cost associated 
with drives like this when they are left running, typically about $0.10 per GB per month. Like VMs an attached storage 
device can be frozen as a stored snapshot, again reducing cost. 


Finally a running Virtual Machine has an ip address when configured as here for remote access. This ip address will change
each time the VM is restarted, either from a stopped state or from a VM image. It is possible to get a "permanent" ip address
associated with a VM to avoid having to keep looking up the latest ip address. On Azure these are called Static Public IPs
whereas on AWS they are called Elastic IPs. 

