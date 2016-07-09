---
published: false
title: Build up a backend to make maps
layout: post
---

**Discalmer** this is the most technical part of the whole process. If you are only interested in the data creation and management have a look only to the next section and beg your your nerd best friend to read the rest of the post and do the nerd work for you.

## Infrastructure overview

PUT here an amazing Diagram! explaining what are Geoserver, PostGIS and GDAL

## Buy a Virtual Machine

If you already didn't realize it we need a server steadily up and running in order to serve the maps. 

Unless you are a very insane and weird person you won't use your old desktop computer for this pourpouse. 

Your partner will break up with you due to the 24h noise in your bedroom, your electrical company will kindly send each month high bills and you have also to pay a lot for a very good internet connection and a static IP address.

Fortunately we are in the ~2016 AD and there are a lot of Virtual server providers which provide us what we need for a reasonable price.

In order to start I would choose a Virtual Server (aka Virtual Machine aka **VM**) with at least 
* `2GB of RAM`
* `2 cores`
* `~50GB of SSD disk`. 

It's also important that the VM can be easilly expanded adding more RAM, CPU cores and disk space.

1. [This](https://www.digitalocean.com/pricing/) gives you the minimum requirements described above for 20$ each month.

2. [This](https://www.linode.com/pricing) tries to compete with the previous one adding for the same price 2 GB of more RAM. Never tried it, but for 4GB of RAM I'll do for my next project.

Important: choose Ubuntu 12.04 or 14.04 as Operating System! you will know why in the next section.

## Use Ansible as the Orchestration engine

Ok now we have an empty Virtual machine. How to deploy all the stuff described above, taking care of proper configurations, users and more?

- What's Ansible?
- Not going to show how to create a playbook but use one of mine

## Use the Ansible GIS playbook

All the instructions are reported on the [Readme.md](https://github.com/Damianofds/ansible-provisioning) file of the repo.

First of all we have to [manually configure a control machine](https://github.com/Damianofds/ansible-provisioning#setting-up-the-control-machine) as described here. 

Basically we will create a local virtual machine using VBOX or VMWARE player to run the Ansible Playbook. 

Yes this step is very annoying but one we made it we can use it to run any other Ansible Playbook. Soner or Later I'll make available the **.iso* of the Virtual Machine I usually use.

After Our control machine is up and running we can [run the Ansible Playbook](https://github.com/Damianofds/ansible-provisioning#usage) to orchestrate all the deployment steps.