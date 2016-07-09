---
published: false
title: Build up a backend to make maps
layout: post
---
Ok this is the most technical part of the whole process. If you are only interested in the data creation and management have a look only to the next section and beg your your nerd best friend to read the rest of the post and do the nerd work for you.

# Infrastructure overview

PUT here an amazing Diagram! explaining what are Geoserver, PostGIS and GDAL

# Buy a Virtual Machine

- There are many provider out of there. Digital Ocean for 20$ a month seems a very good solution to start

# Use Ansible as the Orchestration engine

- What's Ansible?
- Not going to show how to create a playbook but use one of mine

# Use the Ansible GIS playbook

All the instructions are reported on the [Readme.md](https://github.com/Damianofds/ansible-provisioning) file of the repo.

First of all we have to [manually configure a control machine](https://github.com/Damianofds/ansible-provisioning#setting-up-the-control-machine) as described here. 

Basically we will create a local virtual machine using VBOX or VMWARE player to run the Ansible Playbook. 

Yes this step is very annoying but one we made it we can use it to run any other Ansible Playbook. Soner or Later I'll make available the **.iso* of the Virtual Machine I usually use.

After Our control machine is up and running we can [run the Ansible Playbook](https://github.com/Damianofds/ansible-provisioning#usage) to orchestrate all the deployment steps.