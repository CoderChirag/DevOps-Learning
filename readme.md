# Contents

---

- [Contents](#contents)
- [Vagrant](#vagrant)
  - [VM Management Problems](#vm-management-problems)
  - [Vagrant for VMs](#vagrant-for-vms)
  - [Vagrant Architecture](#vagrant-architecture)
  - [VM Setup with Vagrant](#vm-setup-with-vagrant)
  - [Basic Commands](#basic-commands)
  - [Vagrant IP, RAM and CPU](#vagrant-ip-ram-and-cpu)
  - [Vagrant Sync Directories](#vagrant-sync-directories)
  - [Provisioning](#provisioning)

---

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

![vagrant-architecture](https://raw.githubusercontent.com/CoderChirag/DevOps-Learning/main/main/images/vagrant.jpg)

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
-   To view information about all known Vagrant environments on the machine
    > vagrant global-status
-   To view the status of current machine
    > vagrabt status

## Vagrant IP, RAM and CPU

-   To configure a **bridged network adapter** for the VM, uncomment the following line in **Vagrantfile** :<br>`# config.vm.network "public_network"`
-   This bridged adapter would help the VM to fetch an IP address from the host WiFi Router, and thus the VM would behave like it is an independent machine connected to the same network as of the host machine.
    <br><br><br>
-   To give the VM a **static IP address**, uncomment the following line in **Vagrantfile** :<br>`# config.vm.network "private_network", ip: "192.168.25.12"`
-   Make sure that the network address of the IP is not same as the network address of the host machine.
    <br><br><br>
-   To change the **RAM Size** or **No. of CPUs** given to the VM, go to the following block of code in **Vagrantfile** and uncomment and change accordingly :
    ```
    ...
    config.vm.provider "virtualbox" do |vb|
    #    # Display the VirtualBox GUI when booting the machine
    #    vb.gui = true
    #
    #    # Customize the amount of memory on the VM:
        vb.memory = "1600"
        vb.cpus = 2
    end
    ...
    ```
    <br><br>
-   After the required changes are done in **Vagrantfile**, run `$ vagrant reload` if VM was already running, to see the changes.

## Vagrant Sync Directories

-   The directory where your **Vagrantfile** is present is by default synced with the `/vagrant` directory in the VM.
-   To create a custom **synced directory**, uncomment and change accordingly the following line in the **Vagrantfile** : <br> `# config.vm.synced_folder "F:\\myshellscripts", "/opt/scripts"`
    <br><br><br>
-   **NOTE :** If you have created a script in Windows using any editor, and trying to run in Linux and are getting error, make sure that your editor is writing the script in `LF` format instead of `CRLF`.
-   After the required changes are done in **Vagrantfile**, run `$ vagrant reload` if VM was already running, to see the changes.

## Provisioning

-   **Provisioning** in vagrant means executing your commands when the VM starts, or is booted up.
-   Also called **bootstraping** in other technologies.
-   Can be done by uncommenting and changing the following lines accordingly in the **Vagrantfile** :
    ```
    ...
    config.vm.provision "shell", inline: <<-SHELL
       apt-get update
       apt-get install -y apache2
    SHELL
    ...
    ```
    <br><br>
-   After the required changes are done in **Vagrantfile**, run `$ vagrant reload --provision` if VM was already running, to see the changes.
