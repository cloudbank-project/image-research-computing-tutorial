# Waterhackweek talk on building and using images

## Introduction
On September 2, 2020, Cloudbank team members presented "images" for research computing to the annual Water Hackweek hosted by Christina Bandarogoda and others. 

* Objective: De-Mystify cloud use in relation to Jupyter notebooks
* Starting with https://github.com/ChristinaB/dhsvm-opt: Build a working environment from `requirements.txt` or `environment.yml`
  * Data is on a public repository (separate from above)
  * Which cloud platform can I use to run another version of this experiment? (any)
  * Interactive example notebook: How to get from a repo to the Amazon cloud? (stay tuned)
  * How do I configure and run this workflow on the public  cloud without a JupyterHub community service? (ditto)  
  * What are the costs and benefits of building a custom kernel on a community jupyterhub vs an individual server in the cloud with access to Jupyter Notebooks and terminals? (horsepower)

## Executive Summary in two parts

The path to your own (supercharged) Jupyter environment is not trivial but it is

* doable
* super flexible in relation to your data/compute needs
* available 24/7
* cost is proportional to use 


Paying for it: Start with free credits (trial account provides $100). See if this works for you. If so: Set up a real account backed by funds to pay for your cloud use. Credits programs sponsored by vendors can defray initial expense; emphasis on stopgap, not ongoing support. 


## Demystify #1

How do I interact with the cloud?
* Usually first is through a browser, authenticating to a console interface. The AWS console is shown below.
* Next is to install and learn the command line language for the particular cloud
* Third is to use a coding interface. For example using Python with AWS: `boto3`

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/whwtalk01.png" alt="screencap" width="700"/>

## Demystify #2 

<img src="https://github.com/cloudbank-project/image-research-computing-tutorial/blob/master/images/whwtalk02.png" alt="regions" width="500"/>

