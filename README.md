# Research Computing on a Cloud Image

This tutorial introduces a virtual machine *image* as a basis for research computing. 
You may be familiar with a *zip* file that contains an entire directory's contents. A
*machine image* is analogous; think of it as a zip file of the entire computer's contents
from operating system to home directory to code to data files. In the example we use
here the computer will have a single drive with a capacity of 32 Gigabytes. 

## Outline

- In what follows please customize your resources by substituting your own name for `hedylamarr`.
- Obtain credentials to log in to the cloud console: Contact cloudbank for details
- Identify the proper *image* and use it to jumpstart a Virtual Machine *instance*
- Log on to this *instance* and start up a Jupyter Lab server
- Create an **ssh** tunnel from your computer to this server
- Use your browser to connect to the Jupyter Lab and use this to explore some data


## Procedure


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


1. Log on to the AWS console using your credentials.


2. Set your Region (upper right drop-down) to Oregon and choose Services (upper left) to see an 
array of what you can do on the AWS cloud.


3. Choose EC2 in the upper left region of the Services listing. 


4. Choose AMIs from the left sidebar.


5. Ensure that the upper drop-down menu reads 'Private Images'. You should see an AMI listed called `jupyter1-cb`. 


6. Select this AMI by clicking on it so a blue dot appears at the left edge of the table.


7. Choose Actions > Launch 


8. choose a VM type `c5.large` (you may have to scroll down a bit to find this)


9. choose Review and Launch at the bottom right


> WARNING: If this is done as a class activity there is a potential collision scenario. Let's 
take a moment to outline this and how to resolve it.

> The AWS EC2 Launch Wizard goes through seven steps, of which step 6 involves choosing a 
Security Group. This SG is given a name by default; and a classroom full of people will get
the same default as they proceed. So the way to avoid this (which will obstruct the next 
couple of steps) is to click on the Security Group step (step 6) and give a name for the 
Security Group that is unique. As below with keypair and instance names the best choice 
for a Security Group name is simply **yourname**. In our instructions we use the name
`hedylamarr` as an example of your name. Now you can proceed to step 7 of the wizard;
which is step 10 in this procedural. 


10. Choose Launch at the bottom right


11. In the keypair dialog choose Create new keypair and name it `hedylamarr.pem`. 
This will be the key to finding your instance in what follows.


12. Download the keypair file that is generated; then click Launch Instance in the dialog box

 
You now have a Virtual Machine ("EC2 instance") starting up on the public cloud. If you are in
a class with many people doing the same thing at the same time it can be difficult to identify
which machine is *yours*. Once you identify your computer you should name it using **yourname** 
just as you did for the keypair name in steps 11 and 12.

 

13. Continuing in the AWS console: Click View Instances at the lower right. This takes 
you to a table of EC2 instances (Virtual Machines), where one instance is listed per row. 
Therefore one of the rows in this table should be your instance.


14. Locate your instance by the key name: Scroll across to the right in the instance table to find the 
key name column; 
and scan down until you find your keypair name. That row will be the row for your EC2 instance.


15. Hover in the left-most box of this row--the row for your instance--to bring up a little pencil in the "Name" column


16. Click the pencil so you can enter the name of this instance. Call it **yourname**.


17. Wait for the instance state to change to running (a green dot in the Status column). 


18. While you wait for your instance to finish starting up: Write down its `ip address`. 
Find this address by scrolling a bit over to the right. Let's suppose for example that 
your ip address is `12.23.34.45`. We use this in what follows.

 

> Now you are ready to log in to this virtual machine (EC2 instance). 
Once you log in you will start a Jupyter Lab session. This session start
will give you a *token* -- a long string of letters and numbers -- that 
you use to authenticate when you first log in to the Jupyter Lab.

 

19.Open a `bash` shell and ensure the keypair file `hedylamarr.pem` is present in your working directory.


20. Issue this command in bash: `chmod 400 hedylamarr.pem`

 

For **Windows** Users: If you are using Windows you may need to install or enable the native `bash` shell. 
As an alternative you can install an **Ubuntu** `bash` shell. In either case it is useful to realize that
the *home directoy* of this shell is not the same thing as the Windows User home directory. So for example
if the keypair file `hedylamarr.pem` was downloaded to `C:\\Users\hedylamarr\Downloads` you should move this 
file to your `bash` home directory, for example using a command like this: 


```
bash> cd ~
bash> mv /mnt/c/Users/hedylamarr/Downloads/hedylamarr.pem .
bash> chmod 400 hedylamarr.pem
```

There is some method to this madness based in security. The `ssh` command insists that the 
authentication keypair file `hedylamarr.pem` has read-only permission; and files in your Windows User file system
are not amenable to the `chmod` command issued in the `bash` shell. 
These details can very frustrating so we go to the trouble to elaborate this here.

 

21. In bash run `ssh -i hedylamarr.pem ubuntu@12.23.34.45` to log in to your EC2 instance. (Use the correct ip address.)

 

After you respond `yes` at the prompt you are logged on to your EC2 instance on the AWS cloud. The prompt should reflect this.
Note that the default username is `ubuntu`. 

 

22. On the command line of your EC2 instance run `(jupyter lab --no-browser --port=8889) &`


Wait ~1 min for this command to produce multiple lines of output including a couple occurrences of a 
**token string**, as in...

 
```
...token=ae948dc6923848982349fbc48a2938d4958f23409eea427
...token=ae948dc6923848982349fbc48a2938d4958f23409eea427
```


This token string `ae948...` is used in step 25. 


Note: The Linux structure `(command) &` causes `command` to run as a background job. 
This means you can log off your EC2 instance and return to the bash prompt on your own computer. 


23. Copy the token string and type `exit` to return to your local machine bash prompt.


24. In bash run `ssh -N -f -i hedylamarr.pem -L localhost:7005:localhost:8889 ubuntu@12.23.34.45`. Make the appropriate substitutions.

 

Remember that you must substitute the correct name of the `hedylamarr.pem` keypair file; 
and you must use the proper ip address in place of `12.23.34.45`. 
This is 'creating an ssh tunnel': You associating a location on your own computer 
with the connection point on the EC2 instance via port numbers. Your machines port 7005 is actually a tunnel to the 
EC2 instance port 8889.

 
You are almost there. 


25. In your browser address bar type `localhost:7005`

 
This should change to `localhost:7005/lab?` and a brief pause will ensue. You should be prompted for the token,  
the long string you copied from the output in step 23. After this login / pause you should see a Jupyter lab 
environment with a notebook listed at the left called `chlorophyll.ipynb`. Click on this notebook to open it.
In the notebook you can run cells -- blocks of Python code together with explanatory sections -- 
and explore the ocean science data provided.


## Using Jupyter Lab

There are very comprehensive guides to using Jupyter Lab. Here we provide a very minimal description.

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
  * Hover over the circle with the cursor for elaboration
