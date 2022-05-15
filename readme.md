# Vagrant

Vagrant is an automation tool to manage VM lifecycle

## VM Management Problems

-   OS installations
-   Time consuming
-   Manual Setup
-   Tough replication for multi VM
-   Documentations for multi VM

## Vagrant for VMs

-   No OS installations
-   VM setupp through images (vagrant boxes)
-   Images (Boxes available in Vagrant Cloud)
-   Manage VM's with a file (Vagrant file)
-   VM changes automatic through Vagrantfile
-   Vagrant commands to manage VMs
-   Provisioning VM/Executing commands and scripts

## Vagrant Architecture

<div align="center">

![vagrant-architecture](https://raw.githubusercontent.com/CoderChirag/DevOps-Learning/main/images/vagrant.jpg)

</div>

## VM Setup with Vagrant

-   To download vagrant, checkout the [prereqs branch](https://github.com/CoderChirag/DevOps-Learning/tree/prereqs)
-   Get a vagrant box name from https://app.vagrantup.com/boxes/search
-   Run the following commands

```
$ mkdir vagrant
$ cd vagrant
$ vagrant init <boxname>
$ cat Vagrantfile           #If you want to view the contents of the vagrant file
$ vagrant up
```

## Basic Commands

-   To initialize the Vagrantfile
    > $ vagrant init \<boxname>
-   To start / create the VM
    > $ vagrant up
-   To ssh into the VM
    > $ vagrant ssh
-   To shut down the VM
    > $ vagrant halt
-   To delete the VM
    > $ vagrant destroy
-   To reload the VM
    > $ vagrant reload
