# JupyterLab

Jupyter Lab is a notebook server that is "next-gen" beyond the original (now 'classic'). These notes are *in addition to* 
the bootstrap page on some possible challenges. At the moment they are a bit disorganized so please take this as *in development* 
content. 


## widgets

I found that my nbextension `widgets` did not work properly; and as I tried to update I was blocked by a dependency 
(`nodejs`) where an old install was blocking a new install. This led to an exchange on the Jupyter discourse forum
that resolved partially with this: 

[quote="guiwitz, post:5, topic:6554"]
`jupyter labextension install @jupyter-widgets/jupyterlab-manager@2.0`
[/quote]

Thanks to you both. Some progress: I had to throw it in reverse so my necessary install of nodejs v15 was not masked; so that I can run that `jupyter labextension` command. Here is a precis of the command sequence:

* `curl -sL https://deb.nodesource.com/setup_15.x | sudo -E bash` to get nodejs updated
* `sudo apt-get install -y nodejs` to install it; and this worked fine
* `node --version` shows the *wrong* (ancient) version of *nodejs* shadowing what i just installed
* `which node` shows the old nodejs installed within `anaconda`... let's uninstall it
* `conda uninstall nodejs` ... worked... does this reveal the new install?
* `which node` produces `/usr/bin/node` so that looks encouraging
* `node` shows voila v15, great
* `jupyter labextension install @jupyter-widgets/jupyterlab-manager@2.0` per Guillaume; this runs fine

Now the widget appears in Jupyter Lab as desired, no errors, Classic Notebook not necessary. 
`turtle` is still MIA but I'll work on this. Let's call this closed. 
