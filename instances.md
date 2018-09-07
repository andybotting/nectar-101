---
title: Instances
permalink: /instances
icon: server
order: 02
---

An *Instance* is a virtual machine (or server). The Nectar Research Cloud has many physical hardware servers, known as *hypervisors*, which are then used as a pool of resources for creating virtual machines on.

Based on your requirements and quota, you can create Instances of varying sizes to handle your workload.

### Launching an Instance

In the Dashboard, go to the left-hand navigation panel, choose [Project -> Compute -> Instances](https://dashboard.rc.nectar.org.au/project/instances/). This window will show you any running Instances you may have, and allow you to launch new Instances.

Click the `Launch Instance` button to begin the wizard that will guide you through the process of launching your Instance.

### Choosing an Availability Zone

The Availability Zone field allows you to choose which site you'd like your resources to be created in. You do not necessarily need to choose an Availability Zone, but it may be important if you need to use certain features, or if you have a relationship with a particular site.

### Choosing an Image

An image is a pre-prepared *Operating System* which you can use for your Instance. From the `Source` tab, you can see a list of all the available images for you to choose. We recommend using a *Nectar Official Image*, which we prepare with the latest updates and include enhancements to make them work well for the Nectar Research Cloud. They are prfixed with `NeCTAR` and are marked as `Public`.

For this workshop, we're going to use the latest version of the Ubuntu Linux distrubution. Scroll down and choose `NeCTAR Ubuntu 18.04 LTS (Bionic) amd64` by clicking the up arrow next to the item.

### Choosing a Flavor

A flavor is a profile for your new Instance, defining how much virtual CPU, RAM and disk your Instance will have.

If you're using a `project trial`, you'll have enough quota for 2 virtual CPUs. For this workshop, we won't need much, so choose the `m2.xsmall`. This will give you:

* 1 Virtual CPU
* 2GB RAM
* 10GB of Root Disk

### Selecting your Key Pair

From the `Key Pair` tab, ensure the key pair we created in the previous step has been set as `Allocated`. If it hasn't, click the up arrow to move it from the `Available` section to the `Allocated` section.

Click the `Launch Instance` button to boot our new instance.

The new Instance should be created and go into the `Build` status. After a short period of time, it should then become `Active`. Take note of the `IP Address` as we'll need this in the next step.
