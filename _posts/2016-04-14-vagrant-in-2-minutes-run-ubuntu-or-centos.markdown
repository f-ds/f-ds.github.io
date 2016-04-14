---
published: false
title: Vagrant in 2 minutes - run Ubuntu or CentOS 
layout: post
---
Hi this post is about Vagrant. 

Do you ever need for a fresh installation of your favourite Operating System for testing? Here we are going to talk about a tool, called **Vagrant**, which help us doing this on the fly.

###The basics: what and why Vagrant

Do you know what Vagrant is? Awesome. 
Don't you know it? It's just a ruby file to store some settings to start locally a fresh virtual machine. It works by default with VirtualBox but it can be easilly configured with many other virtualizzation system.

I usually prefer VMware Player instead of VirtualBox but vagrant works great with the latter so in my opinion worth the installation of VirtualBox if you don't have already installed it.

At first glance Vagrant seems offer not much more than simply run a live CD of the OS but it's not true: other than a great integration with the most common virtualizzation softwares it's works very well also with provisioning tools like Vagrant, Chef and pupped or simply bash scripting.

###It's time to install some stuff

Other than **git** we need some stuff working on our laptop:
As we said Vagrant works by default with **Virtual Box**, so let's install it. 

[Pick the one compatible with your host OS](https://www.virtualbox.org/wiki/Download_Old_Builds_5_0)
 
*It's the download link for the 5.0.x which is the one I have installed at the time of writing... but I bet everything works also with a future VBox release*

Now clone this repo:

~~~~~~~~~~~~~~~~
#$ git clone git@github.com:Damianofds/vagrant-fresh-os.git
~~~~~~~~~~~~~~~~~~~~~~~~

this repo it's not so fancy: just a basic Vagrantfile, which is the main (and only) vagrant configuration file.

###First vagrant run

Now you have everything you need and you are ready to run your first fresh VirtualMachine with Vagrant: no need for open the VM player, search for the proper ISO to install and waste time installing it...

just go to the root of the previously cloned repo and run:

~~~~~~~~~~~~~~~~
#/home/fds/work/code$ vagrant up
~~~~~~~~~~~~~~~~~~~~~~~~

That's it.

Of course to run a fresh OS installation we need its ISO file. If it is the first time we run this VM Vagrant has to download it. From where? it's configured with a property in the Vagrantfile and we are going to have a look to it soon, but now let's continue to play with the just launched VM.

you can log in in 2 different way:

Simply with

~~~~~~~~~~~~~~~~
#/home/fds/work/code$ vagrant ssh
[vagrant@localhost ~]$ whoami
vagrant
[vagrant@localhost ~]$
~~~~~~~~~~~~~~~~~~~~~~~~

without neither taking care of the login user credentials, of course they are configured in the Vagrantfile. 

Let's say that the user credentials are bagrant/vagrant, we can connect to the VM also with 

~~~~~~~~~~~~~~~~
#/home/fds/work/code$ ssh vagrant@192.168.66.6
~~~~~~~~~~~~~~~~~~~~~~~~
