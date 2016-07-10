---
published: false
title: Build up a backend to make maps
layout: post
---
This is the most technical part of the whole process and we are going to use some DevOps tools.

If you are only focused on the creation and management of the data have a look only to the System Overview section and beg your your nerd best friend to read the rest of the post and do the nerd work for you.

## System overview



* Geoserver

* PostGIS 

* GDAL

## Buy a Virtual Machine

If you didn't realize it yet, we need a server steadily up and running in order to serve the maps to a client. We can easilly deploy all the static code of a map browser on github pages but we need a backend to host our custom dataset created in the previous post.

Unless you are a very insane and weird person you won't use your old desktop computer for this pourpouse. 

The reason is simple: your partner will break up with you due to the 24h server noise in your bedroom, your electrical company will kindly send each month high bills and you have also to pay a lot for a very good internet connection and a static IP address.

Fortunately we are in the ~2016 AD and there are a lot of hosting services which give us what we need for a reasonable price.

In order to start I would choose a Virtual Server (aka Virtual Machine aka **VM**) with at least 
* `2GB of RAM`
* `2 cores`
* `~50GB of SSD disk`. 

It's also important that the VM can be easilly expanded adding more RAM, CPU cores and disk space in case of need.

1. [This company](https://www.digitalocean.com/pricing/) gives you the minimum requirements described above for 20$ each month.

2. [This other one](https://www.linode.com/pricing) tries to compete with the previous adding for the same price 2 GB of more RAM. Actually I never tried it, but for 4GB of RAM it could be worth it.

Important: For this specific tutorial choose Ubuntu 12.04 or 14.04 as Operating System! you will know why in the next section.

## Use Ansible for the configuration management

Ok now we have an empty Virtual machine. How to deploy all the stuff described above, taking care of proper configurations, users settings and more?

The answer is: use a configuration management software!

from wikipedia:

>In software engineering, software configuration management (SCM or S/W CM)[1] is the task of tracking and controlling changes in the software, part of the larger cross-disciplinary field of configuration management.

[...]

>However, "configuration" is generally understood to cover changes typically made by a system administrator.

Basically what we do is to replace a System administrator with some code that do its work.

Of course we need to write the code that will replace all the operations performed by the sysadmin but here comes the cool part:

* We are going to use **Ansible** as our configuration management software. Ansible has a lot of modules we can re-use to deploy our backend, we basically only have to glue each module to create what is called a **Playbook** that will do the work for us

* I already made the Playbook we need and you are very welcome to use [it](https://github.com/Damianofds/ansible-provisioning). Nothing is perfect in this world so Pull Request to improve it are always welcome!

So we are not going to enter in the details of how to make Ansible Playbooks in the rest of this post, but just how to use the one I made to do the work.

## Use the Ansible GIS playbook

OK, I'll be rude but all the instructions are reported on the [README.md](https://github.com/Damianofds/ansible-provisioning) file of the repo... so just read it!

A couple of comments:

First of all we have to [manually configure a control machine](https://github.com/Damianofds/ansible-provisioning#setting-up-the-control-machine) as described here. 

Basically we will create a local virtual machine using VBOX or VMWARE player to run the Ansible Playbook. 

Yes this step is very annoying but one we made it we can use it to run any other Ansible Playbook. Soner or Later I'll make available the **.iso* of the Virtual Machine I usually use.

After Our control machine is up and running we can [run the Ansible Playbook](https://github.com/Damianofds/ansible-provisioning#usage) to perform all the deployment steps.

## Summary

**Celebrate again!** You created your first *production-like* geospatial backend from scratch!

In this post we talked about the main components of the backend and how to make all the complex deploy tasks automatically.

In the next (and last post of this serie) we we'll see how to load and update the geospatial vector dataset on the system we made.