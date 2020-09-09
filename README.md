# Research Computing on a Cloud Image

This tutorial introduces a virtual machine ***image*** as a basis for research computing.


You may be familiar with a *zip* or *tar* file containing an entire directory. 
A *machine image* is analogous; think of it as a zip file of the entire computer's contents
from operating system to home directory to code to data files. The idea is that once 
a cloud Virtual Machine (VM) is *configured* for use: It can be stored as a machine 
image. This image is then un-packed back onto a Virtual Machine. The image is used
to creating a working research computing environment for a scientist to use. 


Our specific example here takes advantage of the Jupyterlab notebook server. This page
is a tutorial for going from a pre-built Linux environment with Jupyterlab installed 
as an *image* and reconstituting it on the AWS cloud as a Virtual Machine. We will use
a secure tunnel called an `ssh` tunnel to enable you the researcher to connect to the
Jupyterlab server through your web browser. 


In the sub-folder called `bootstrap` we go through the process of building this machine image. 

I the sub-folder called `waterhackweek` we go from the rebuilt Jupyterlab VM to cloning a 
repository of notebooks on the VM for a particular research topic. 


## Important remark on cost management


In what follows on this page: The VM has a single disk drive (filesystem) with a fairly small 
capacity of 32 Gigabytes. However it is common practice to create images that bundle large
datasets. The sub-folder `bootstrap` tutorial spins up a Virtual Machine, for example, that has
600 GB of attached SSD data directory capacity. This allows the image *builder* to include 
a moderately large dataset with the image and provide some working space as well. 

Our point of emphasis here -- as with all cloud resource allocation actions -- is that one 
should understand and track the costs associated with cloud resources. 'Block' or 'disk' 
storage -- for example -- runs about $60 per month for 600 GB.

> Again our main admonition: The cloud is very powerful but it is
important to understand and manage the cost of using it. 


## Outline

- In what follows please customize your resources by substituting your own name for `hedylamarr`.
- Obtain credentials to log in to the cloud console: Contact cloudbank for details
- Identify the proper *image* and use it to jumpstart a Virtual Machine *instance*
- Log on to this *instance* and start up a Jupyter Lab server
- Create an **ssh** tunnel from your computer to this server
- Use your browser to connect to the Jupyter Lab and use this to explore some data


## For Windows Users

A brief interruption anticipating a possible issue: Further down we are going to build 
something called an "ssh tunnel" to use our Virtual Machine as a Jupyter notebook server. 
If you are doing this on a PC running Windows: No problem, that's perfectly feasible but
Windows does not natively make this Linux-y step trivial.  So be ready: We will introduce
the idea of installing a small Linux bash shell on the Windows PC. It is a bit of a 
"yet another step, really??" situation but on balance it can help save time and avoid frustration. 
[Instructions are here](https://ubuntu.com/tutorials/tutorial-ubuntu-on-windows#1-overview).


## Procedure

Moving on. 


We would like to visually explore some (ocean) data. This data took years to collect and 
months to bring together in one location. Hopefully it will take you less than an hour to
open up a Jupyter Lab notebook in your browser to explore this data. 


> Prerequisites: Cloudbank credentials to connect to the cloud and an available bash shell.


We are using in this case the AWS (Amazon Web Services) cloud. You will log on to the AWS 
console, start a Virtual Machine (called an EC2 *instance*) and on that machine start a 
Jupyter Lab server. If you were starting from nothing you would be installing Python 
packages and importing datasets for quite some time; but our objective here is to avoid 
all of that by using a pre-built environment stored as an *image*. Once you have identified
this image you can start it on virtually any size machine; from a small cheap one say
costing $0.04 per hour to a very powerful computer that might cost $1.00 / hour or more. 
Cloud users choose a computer based on computational needs.


Once the computer is running (with everything pre-installed) you will create 
an *ssh tunnel*. This is a secure connection that associates a local address with a Jupyter Lab 
server running on the cloud instance. By connecting through this tunnel the cloud instance 
becomes the backing engine for exploring the data. 


The procedure is presented in 25 steps with interspersed comments.
Upon completion you will have your own full-blown data science research environment. 
Don't forget to turn the lights out when you are done exploring. 


### Procedure


1. Log on to the AWS console using your credentials; and be sure to set your Region (upper right drop-down) to Oregon
    o Sidebar: You can choose Services (upper left) to see a listing of services, i.e. what you can do on the AWS cloud


2. Navigate to Amazon Machine Image choices (AMIs): Services > Compute > EC2. Then choose AMIs from the left sidebar. 


3. In the upper left drop down menu of the AMI pane select 'Private Images' (it may say 'Owned by Me' or 'Public Images' by default). When 'Private Images' is selected, you should see an AMI listed called `jupyter1-cb`. Select this AMI by clicking on it so a blue dot appears at the left edge of the table.


4. Choose Actions > Launch. Choose a VM type `c5.large` (you may have to scroll down a bit to find this). Choose Review and Launch at the bottom right.


> WARNING: If this tutorial is a class-sized activity there is a potential collision scenario. Let's 
take a moment to outline this and how to resolve it.


> The AWS EC2 Launch Wizard goes through seven steps. Step 6 involves choosing a 
Security Group. This SG is given a name by default; and a classroom full of people will get
the same default as they proceed. So the way to avoid this (which will obstruct the next 
couple of steps) is to click on the Security Group step (step 6) and give a name for the 
Security Group that is unique. As below with keypair and instance names the best choice 
for a Security Group name is simply `yourname`. In our instructions we use the name
`hedylamarr` as an example of your name. Now you can proceed to step 7 of the wizard;
which is step 10 in this procedural. 


5. Choose **Launch** at the bottom right. In the ensuing keypair dialog choose **Create new keypair** 
and name it `hedylamarr`. This will be the key to identifying your instance and logging in to
it in what follows. Download the keypair file you generated; then click **Launch Instance**
in the dialog box.
 

6. Continue in the AWS console: Click **View Instances** at the lower right. This takes 
you to a table of EC2 instances (Virtual Machines), where one instance is listed per row. 
Therefore one of the rows in this table should be your instance.


> Class strategy remark: You now have a Virtual Machine ("EC2 instance") on the public cloud. If you are in
a class with many people doing the same thing at the same time it can be difficult to identify
which instance is *your* VM. Once you identify your instance: Name it using `yourname`.


7. Locate your instance by the key name: In the table of instances scroll right to find the 
`Key name` column. Scan down this column for the keypair you used to identify the row for your EC2 instance.
Return to the left-most **Name** column of this row -- the row for your instance -- and hover your cursor to bring up 
a pencil icon in the "Name" column. Click on that and name your instance appropriately.


8. One the instance status dot changes to green (running): Note its `ip address` in the instance table.
Here we take this to be `12.23.34.45` as an example. 


> Now we are ready to log in to this VM or EC2 instance from a bash shell. Our main goal on the
instance is to start a 'quiet' Jupyterlab session. There will not initially be a browser interface; 
that comes a little further down. Once the Jupyterlab server starts it will print a *token*, a long 
string of letters and numbers used to authenticate; so be prepared to copy that to a text editor
for a few minutes. 
 

9. On your computer: Open a `bash` shell and ensure the keypair file `hedylamarr.pem` is present in your working directory.
Issue a `chmod` command to give the keypair file limited `rwx` permissions: `chmod 400 hedylamarr.pem`.


> If running **Windows** on your local computer: You may need to install or enable the native `bash` shell. 
As an alternative you can install an **Ubuntu** `bash` shell. In either case it is useful to realize that
the *home directoy* of this shell is not the same location as the Windows User home directory. 
If `hedylamarr.pem` was downloaded to `C:\\Users\hedylamarr\Downloads`:  Move it to your `bash` home 
directory, for example using this sequence of commands: 


```
bash> mv /mnt/c/Users/hedylamarr/Downloads/hedylamarr.pem ~
bash> cd ~
bash> chmod 400 hedylamarr.pem
```


> The `ssh` command insists the authentication keypair file `hedylamarr.pem` have User read-only permission,
Files in the Windows User area are not amenable to `chmod`; so we relocate this file and proceed in `bash` from `~`.
These details can very frustrating so we go to the trouble to elaborate this here.

 

10. In bash run `ssh -i hedylamarr.pem ubuntu@12.23.34.45` to connect to your EC2 instance. (Use the correct ip address.)
Respond `yes` at the prompt to complete the login. Note that your username is `ubuntu` and you have the ability to run
root commands using `sudo`. 


> If you have configured the image using (for example) the tutorial in the `bootstrap` sub-folder: You may wish 
to spend some time looking around to ensure the instance is configured as expected. For example if there are 
additional data disks you can check to see these have been mounted properly. 


11. On the command line of your EC2 instance issue `(jupyter lab --no-browser --port=8889) &`
After about one minute this command should produce multiple lines of output including a 
`token string`.

 
```
...token=ae948dc6923848982349fbc48a2938d4958f23409eea427
...token=ae948dc6923848982349fbc48a2938d4958f23409eea427
```


Copy this string to a text editor for later use. Issue `exit` to log out of your cloud instance.


> Note: The Linux structure `(command) &` causes `command` to run as a background job. 
This allows you to log out of your instance, leaving it to function as a Jupyterlab server.


12. In the `bash` shell issue `ssh -N -f -i hedylamarr.pem -L localhost:7005:localhost:8889 ubuntu@12.23.34.45`. 
(Make appropriate substitutions.)


> This `ssh` command creates an ssh tunnel to the EC2 instance running the Jupyterlab server. 
You associate a location on your own computer (localhost:7005) with the connection point on the EC2 instance 
(12.23.34.45:8889). The number trailing the colon, called a port, acts as a qualifier for the connection.


13. In your browser address bar type `localhost:7005`. This should change to `localhost:7005/lab?`.
When prompted for the token paste in the token string you copied earlier. You should now see a 
Jupyterlab environment. When using the instructional image for this tutorial there should be 
a notebook listed on the left called `chlorophyll.ipynb`. You may click on this notebook to open it
and explore the contents. 


## Using Jupyterlab


There are comprehensive guides to using Jupyterlab. Here we include some minimal notes.


* A Jupyter Lab environment has a left, narrow panel and a primary, larged notebook panel
    * Notebooks are listed in the left panel
    * Double-click them to open them in the main panel
    * In our example the primary notebook of interest is called `chlorophyll.ipynb`
* A Jupyter Lab *notebook* such as `chlorophyll.ipynb` consists of a sequence of cells.
    * Each cell contains some text
    * For our purposes each cell is either a *Python code* cell or a *markdown* cell
        * Python code executes (or tries to execute) when the cell runs
        * Markdown renders as formatted readable text -- like a narrative -- when the cell is run
* How to run Jupyter Lab notebook cells
    * Notice that a cell is selected when it has a vertical blue bar at its left border
    * Select a cell by clicking on it with the cursor; or by using up/down arrows 
    * Run a cell using Shift-Enter (this advances the selected cell to the next cell down)
    * Run a cell using Ctrl-Enter (this does not advance the selected cell)
    * Run a cell using Alt-Enter (this runs the cell and opens a new empty cell below it)
* Modifying cells
    * Double-click on a cell to open it as a text editor
    * Edit the text to change what the cell does; and then run it to see the results
* The *kernel* is the program that executes the Python code
    * The kernel status is indicated by a circle at the upper right of the notebook
    * Hover over the circle with your cursor for elaboration
