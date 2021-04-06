# Creating a VM image on the Azure cloud

This procedural walks you through creating a working Virtual Machine with a Jupyter notebook server, testing it, 
and storing it as an Azure machine image. This last step is a single click task; the bulk of the effort is 
the preparation. This walk-through uses the [Azure portal](portal.azure.com), a browser-based interface to
the services available on the Azure cloud. 


## Walk-through

* Sign in to the [Azure portal](portal.azure.com) and find or create a *Resource Group* (RG) in a region nearest to you.

<BR><BR><BR>

<img src="{{site.url}}/images/azure/Azure_image_01.png" alt="drawing" width="600" style="display: block; margin: auto;"/>

<BR>
spacer
<BR><BR>
   
![first image]({{ site.url }}/images/azure/Azure_image_01.png")

<BR><BR><BR>

* Select this Resource Group click on `+ Add`



<BR><BR><BR>


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_02.png" alt="drawing" width="600"/>

<BR><BR><BR>

* This gives us a drop-down that includes Marketplace. This is where we find existing "blank" images.

<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_03.png" alt="drawing" width="200"/>

<BR><BR><BR>

* Select from the Marketplace an Ubuntu Server image. Notice that an *image* is what we are building. 
    * We start out by selecting an "empty" image that includes nothing more than the Ubuntu operating system. 

<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_04.png" alt="drawing" width="200"/>

<BR><BR><BR>

* From here we have the opportunity to `Create`; which means "build a Virtual Machine using this Ubuntu operating system". 

<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_05.png" alt="drawing" width="600"/>

<BR><BR><BR>

* This brings us to a multi-step VM builder wizard. 
* Enter a machine name and a Size from the dropdown. 
* Notice that the monthly cost of a given VM size is shown.
* The remaining entries in the first wizard form should be default values.

<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_06.png" alt="drawing" width="800"/>

<BR><BR><BR>

* Use **Next** to arrive at the disk tab of the VM wizard. Add a 256GB disk in order to create some data capacity on this VM.

<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_07.png" alt="drawing" width="800"/>

<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_08.png" alt="drawing" width="600"/>

<BR><BR><BR>

* Once disk volume is selected you can (**Ok**) confirm you want to **Create a new disk**.

<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_09.png" alt="drawing" width="600"/>

<BR><BR><BR>


* Next are Networking, Management and Advanced tabs: Defaults are fine. 

<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_10.png" alt="drawing" width="800"/>

<BR><BR><BR>


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_11.png" alt="drawing" width="800"/>

<BR><BR><BR>

* **Tags** tab: Include some tags so that an account administrator knows what these resources are for.

<BR><BR><BR>


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_12.png" alt="drawing" width="800"/>

<BR><BR><BR>

* At the **Review and Create** tab we can get a sense of what will be built and what it will cost to operate (here 12 cents per hour). 

<BR><BR><BR>


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_13.png" alt="drawing" width="600"/>

<BR><BR><BR>

* A pop-up dialog gives you the opportunity to download the access key-pair file. 
* We use this rather than using a password to log in to this VM. 
* Place it in a secure location that will not accidentally end up copied to GitHub.

> For Windows users: You can use this `.pem` file as-is from a locally installed bash shell

> For all users: You will need to change the permissions of this `.pem` file to be restrictive.
> This can be done with the command `chmod 400 fu.pem` once it is downloaded.

<BR><BR><BR>


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_14.png" alt="drawing" width="400"/>

<BR><BR><BR>

* Deployment complete message:

<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_15.png" alt="drawing" width="800"/>

<BR><BR><BR>

* The following graphics provide a lot of the details of operation of this newly-created VM:

<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_16.png" alt="drawing" width="800"/>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_17.png" alt="drawing" width="800"/>

<BR><BR><BR>

* Note that the VM is given an ip address. 
    * Let's suppose this happens to be `111.22.33.44`. 
    * The command from the bash shell to login to this virtual machine is then

```
ssh -i fu.pem azureusuer@111.22.33.44
```

* Confirm and login. You should now have a prompt `azureuser@machimename:~$ `
    * The primary cause of this step not working is not setting the permission correctly on the `.pem` file (see above).


* The next task is to mount the data disk. 
    * This was not done automatically.
    * The command here is `lsblk -o NAME,HCTL,SIZE,MOUNTPOINT`
    * The 256GB disk has no mount point as yet

<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_18.png" alt="drawing" width="600"/>

<BR><BR><BR>

* Follow directions for mounting a disk on an Azure VM
    * [Here is some documentation](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/attach-disk-portal)
    * Based on this the following commands were used to mount the data disk

```
sudo parted /dev/sdc --script mklabel gpt mkpart xfspart xfs 0% 100%
sudo mkfis.xfs /dev/sc1
sudo partprobe /dev/sdc1
sudo mkdir /datadrive
sudo mount /dev/sdc1 /datadrive
sudo blkid
```

* You can verify the mount by re-running lsblk
* You might need to `sudo chmod` on the `/datadrive` to make it read/write-able.
* Note the web page provides links to further documentation.

<BR><BR><BR>
   
<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_19.png" alt="drawing" width="800"/>

<BR><BR><BR>
<BR><BR><BR>


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_20.png" alt="drawing" width="800"/>
<BR><BR><BR>
<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_21.png" alt="drawing" width="600"/>
<BR><BR><BR>
<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_22.png" alt="drawing" width="600"/>
<BR><BR><BR>
<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_23.png" alt="drawing" width="600"/>
<BR><BR><BR>
<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_24.png" alt="drawing" width="600"/>

<BR><BR><BR>

* To at last make the VM image: Select the VM in the portal and click **Capture**

<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_25.png" alt="drawing" width="600"/>

<BR><BR><BR>

* The "make image" wizard comes up.
* On the side dialog select No, capture only a managed image

<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_26.png" alt="drawing" width="600"/>


<BR><BR><BR>
   
* The VM stops and the image creation process starts up. Shortly thereafter (minutes) we have an image of the VM available. 


<BR><BR><BR>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_27.png" alt="drawing" width="600"/>



