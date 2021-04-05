# Creating a VM image on the Azure cloud

This procedural walks you through creating a working Virtual Machine with a Jupyter notebook server, testing it, 
and storing it as an Azure machine image. This last step is a single click task; the bulk of the effort is 
the preparation. This walk-through uses the [Azure portal](portal.azure.com), a browser-based interface to
the services available on the Azure cloud. 


## Walk-through

* Sign in to the [Azure portal](portal.azure.com) and find or create a *Resource Group* (RG) in a region nearest to you.

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_01.png" alt="drawing" width="600"/>


* Select this Resource Group click on `+ Add`





<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_02.png" alt="drawing" width="600"/>

This gives us a drop-down that includes Marketplace. This is where we find existing "blank" images.

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_03.png" alt="drawing" width="200"/>


Select from the Marketplace an Ubuntu Server image. Notice that an *image* is what we are building. 
We start out by selecting an "empty" image that includes nothing more than the Ubuntu operating system. 


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_04.png" alt="drawing" width="200"/>


From here we have the opportunity to `Create`; which means "build a Virtual Machine using this Ubuntu operating system". 

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_05.png" alt="drawing" width="200"/>


This brings us to a multi-step VM builder wizard. Enter a machine name and a Size from the dropdown. 
Notice that the monthly cost of a given VM size is shown.
The remaining entries in the first wizard form should be default values.


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_06.png" alt="drawing" width="800"/>


Use **Next** to arrive at the disk tab of the VM wizard. Add a 256GB disk in order to create some data capacity on this VM.


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_07.png" alt="drawing" width="800"/>



<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_08.png" alt="drawing" width="600"/>


Once disk volume is selected you can (**Ok**) confirm you want to **Create a new disk**.


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_09.png" alt="drawing" width="600"/>


Next is the networking tab; defaults are fine. 


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_10.png" alt="drawing" width="800"/>


Networking: Likewise


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_11.png" alt="drawing" width="800"/>


**Advanced** tab: Nothing to do. **Tags** tab: Include some tags so that an account administrator knows what these resources are for.


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_12.png" alt="drawing" width="800"/>


At the **Review and Create** tab we can get a sense of what will be built and what it will cost to operate (here 12 cents per hour). 



<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_13.png" alt="drawing" width="600"/>


A pop-up dialog gives you the opportunity to download the access key-pair file. We use this rather than using a password to log in to this VM. 



<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_14.png" alt="drawing" width="400"/>


Deployment complete message:


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_15.png" alt="drawing" width="800"/>


The following graphics provide a lot of the details of operation of this newly-created VM:


<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_16.png" alt="drawing" width="800"/>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_17.png" alt="drawing" width="800"/>



<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_18.png" alt="drawing" width="600"/>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_19.png" alt="drawing" width="600"/>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_20.png" alt="drawing" width="600"/>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_21.png" alt="drawing" width="600"/>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_22.png" alt="drawing" width="600"/>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_23.png" alt="drawing" width="600"/>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_24.png" alt="drawing" width="600"/>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_25.png" alt="drawing" width="600"/>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_26.png" alt="drawing" width="600"/>

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/azure/Azure_image_27.png" alt="drawing" width="600"/>



