# Virtual Machine *Images*
## A CloudBank Solution


- [Walkthrough home](https://cloudbank-project.github.io/image-research-computing-tutorial/)
- [Azure-specific walkthrough for MSE544](https://cloudbank-project.github.io/image-research-computing-tutorial/azure/create_an_image/)



This walkthrough introduces *virtual machines* and corresponding *machine images*. 
On the cloud these are a powerful approach to on-demand research computing. 
A **machine image** is analogous to a **container image** but with a complete 
operating system bundled up.


## Overview of virtual machine *images*


Just as a file folder and its contents can be bundled up 
as a single *zip* or *tar* file, so for an entire *virtual machine* (**VM**):
The zip file corresponds to a machine *image*. This includes the entire 
computer's contents from operating system to home directories to code to data files. 


Once a cloud Virtual Machine (henceforth 'VM') is *configured* for use, 
it can be stored as a machine image, i.e. as a collection of bytes. This 
stored snapshot does not execute code; but at some time in the future
it can be loaded back into a new VM.



***How much does it cost to run a Virtual Machine (VM) for a day?***


\$10 to \$30 per day for a moderately powerful range of VMs.


***How much does it cost to store a Virtual Machine for a day?***


Typically less than \$1 per day, an order of magnitude less than a running VM.


***What are other features of machine images?***


- A machine image can be built with both tools and data pre-installed. As such 
it can be started in a matter of minutes as a fully capable virtual machine. 
Virtual machines can be very cheap (a few cents per hour) or quite expensive
(20 dollars per hour for a powerful VM) so it is important to realize that
a machine image can be started on any VM across the power/cost spectrum. 


- Once a machine image is started it can be started again; and again. One can
make multiple copies of the running VM with no limit. 


The main admonition: The cloud is powerful but it is
important to understand and manage the cost of using it. 


## Outline of tutorials

There are two tutorials per cloud: Creating an image and using an image. Our 
specific use case is a VM running a Jupyter notebook server.


To create an image


- Obtain authentication credentials to log in to the cloud console
- Log in to the console and start a VM, being sure to obtain login credentials (different than above)
- Log directly in to the VM 
- Configure the working environment by installing Anaconda and other packages
- Start a notebook server in headless mode, i.e. without a browser, but associated with a particular port
- Use an ssh tunnel from your computer (say a laptop) to connect to the Jupyter notebook server to test it
- From the console convert the virtual machine to a saved image


To use an image as a Jupyter notebook server


- Obtain credentials to log in to the cloud console
- Identify the proper *image* and use it to start up a Virtual Machine *instance*
- Log on to this *instance* and start up a Jupyter Lab server in no-browser mode; and associate a port
- Create an **ssh** tunnel from your computer to this server
- Use your browser to connect to the Jupyter notebook server and carry on with your research
- Save your work and when you are done be sure to Stop (not Terminate) your VM


## For Windows Users


A brief remark: We build 
something called an "ssh tunnel" when using our Virtual Machine as a Jupyter notebook server. 
If you are doing this on a PC running Windows: No problem, that's perfectly feasible. However
Windows does not natively make the Linux-esque tunnel trivial.  We will introduce
the idea of installing a Linux bash shell on the Windows PC. This is a bit of a 
"yet another step, really??" but it saves time and avoids certain frustrations. 
[Instructions are here](https://ubuntu.com/tutorials/tutorial-ubuntu-on-windows#1-overview).




