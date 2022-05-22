# Learning DevOps

This is going to be my journey to learn Devops from very basics to advanced level. I will try to share each and every step of it so it can also benefit others.

I am making this repo so that you don't have to go and search at different different places and you can find every topic here only.

I will try to provide complete notes and practical projects in complete detailed and in an easy to understand manner.

# Contents

---

- [Learning DevOps](#learning-devops)
- [Contents](#contents)
- [1. Introduction to DevOps](#1-introduction-to-devops)
  - [Transforming an Idea into Application](#transforming-an-idea-into-application)
    - [Software Development Life Cycle (SLDC)](#software-development-life-cycle-sldc)
      - [Models in SLDC](#models-in-sldc)
    - [DevOps Lifecycle](#devops-lifecycle)
    - [Continous Integration (CI)](#continous-integration-ci)
    - [Continous Delivery (CD)](#continous-delivery-cd)
- [2. Virtualization](#2-virtualization)
    - [Life before Virtualization](#life-before-virtualization)
  - [Virtualization](#virtualization)
  - [Basic Terminologies](#basic-terminologies)
- [3. Vagrant](#3-vagrant)
  - [VM Management Problems](#vm-management-problems)
  - [Vagrant for VMs](#vagrant-for-vms)
  - [Vagrant Architecture](#vagrant-architecture)
- [4. Basics of Linux](#4-basics-of-linux)
  - [Open Source](#open-source)
  - [Linux History](#linux-history)
  - [Linux principles](#linux-principles)
  - [Why Linux?](#why-linux)
  - [Linux Architecture](#linux-architecture)
  - [Popular Linux distros](#popular-linux-distros)
- [5. Apache Web Server](#5-apache-web-server)
  - [Introduction](#introduction)
- [6. Nginx Web Server](#6-nginx-web-server)
  - [Introduction](#introduction-1)

---

# 1. Introduction to DevOps

## Transforming an Idea into Application

### Software Development Life Cycle (SLDC)

Development of any software comes up with a life cycle of certain steps which are executed in a cyclic manner.
The steps are:

1. Requirement Gathering
2. Planning
3. Designing
4. Development
5. Testing
6. Deploy and Maintain
 <div align="center">

![sldc](./images/sldc.png)

</div>

#### Models in SLDC

-   Waterfall Model - Each phase begins only when previous is completed.
-   Agile Model - Work is divided into smallet lists of several tasks, several iterations of SLDC for each list
-   Spiral Model
-   Big Bang Model
-   etc...

### DevOps Lifecycle

1. Code - Developers commit code
2. Code Test - Unit and Integration test
3. Code Analysis - Vulnerability and best practices analysis
4. Delivery - Deploy changes to staging
5. DB/sec changes - Every other ops changes
6. Software Testing - QA / Functional tests
7. Deploy to production - Go live, user traffic diverted to new changes
8. User Approval - User feedback
9. Keep Monitoring

### Continous Integration (CI)

Continous Integration is the practice of automating the integration of code changes from multiple contributors into a single software project.

In simpler words, it is the process that automates the build and tests after every commit and sends notifications to developers accordingly.
If the code committed by developer is passed by automated tested the developer is notified and if it doesn't the developer is notified and he commits again and the issues are fixed.

<div align="center">

![CI](images/ci.png)

</div>

### Continous Delivery (CD)

Continous Delivery is a strategy for software releases wherein any code commit that passes the automated testing phase is automatically released into the production environment, making changes that are visible to the software's users.

In simpler words, it is the process that automates the deployment process so that every deployment request generated by CI is fulfilled.
It is basically the extension of CI.

<div align="center">

![CI/CD](images/ci-cd.png)

</div>

# 2. Virtualization

### Life before Virtualization

-   To run App/Services we needed servers
-   Physical Computer (Servers in Data Centers)
-   One service - one server
-   Servers were always overprovisioned
-   Servers resources were mostly underutilized
-   Huge capital expenditure and operational expenditure

## Virtualization

-   Allows one computer to run multiple OS
-   Partition physical resource in virtual resources
-   Virtual machines run in completely isolated manner, so no more one server - one service rule
<div align="center">

![vt](images/vt.png)

</div>

## Basic Terminologies

1. **Host OS** => OS of the physical machine in which virtualization is going to take place.
2. **Guest OS** => Os of the Virtual Machine.
3. **Virtual Machine (VM)** => The machine which is virtual running on anothe machine(called host machine) and is isolated from all other virtual machines running on the same host.
4. **Snapshot** => Way of taking a backup of VM.
5. **Hypervisor** => Tool or software which enables the Virtualization and lets us create a VM.

Hypervisor is of 2 types
: 1. **Type 1** - Runs as a base OS, used for production. Eg: VMWare ESXI, XEN Hypervisor
: 2. **Type 2** - Runs as a software, ideal for learning and testing purposes. Eg: Oracle Virtualbox, VMWare Server / Player.

# 3. Vagrant

It is an automation tool to manage VM lifecycle

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

![vagrant-architecture](images/vagrant.jpg)

</div>

To study vagrant in more detail with its practical implementation refer to the [vagrant branch](https://github.com/CoderChirag/DevOps-Learning/tree/vagrant)

# 4. Basics of Linux

## Open Source

Open source software is a software which have its entire source code open, and anybody can inspect, modify, and enhance the software.

<div align="center">

![open source](./images/open_source.png)

</div>

## Linux History

-   **1984**: The GNU Project and the Free Software Foundation
    -   Creates open source version of UNIX utilities
    -   Creates the General Public License (GPL)
        -   Software license enforcing open source principles
-   **1991**: Linus Torvalds
    -   Creates open source, UNIX-like kernel, release under the GPL
    -   Ports some GNU utilities, solicits assistance online
-   **Today**:
    -   Linux kernel + GNU utilities = complete, open source, UNIX-like operating system
        -   Packaged for targeted audiences and distributions

## Linux principles

-   Everything is a file (including hardware)
-   Small Single purpose Programs
-   Ability to chain programs together for complex operations
-   Avoid Captive User Interfaces (GUI which waits for user interaction)
-   Configuration data stored in files

## Why Linux?

-   Opensource
-   Community Support
-   Support Wide Variety of hardware
-   Customization
-   Most Servers run on Linux
-   Automation
-   Security

## Linux Architecture

<div align="center">

![architecture](images/linux_architecture.jpg)

</div>

## Popular Linux distros

-   **Desktop based**
    -   Ubuntu Linux
    -   Linux Mint
    -   Arch Linux
    -   Fedora
    -   Debian
    -   OpenSuse
-   **Server based**
    -   Red Hat Enterprise Linux
    -   Ubuntu Server
    -   Centos
    -   SUSE Enterprise Linux

For more detailed study of linux refer to the [Linux branch](https://github.com/CoderChirag/DevOps-Learning/tree/linux)

# 5. Apache Web Server

## Introduction

-   The Apache HTTP server is the most widely-used web server in the world.
-   It provides many powerful features including dynamically loadable modules, robust media support, and extensive integration with other popular software.

For more details about apache and its implementation, go to [apache2 branch](https://github.com/CoderChirag/DevOps-Learning/tree/apache2)

# 6. Nginx Web Server

## Introduction

-   Nginx is one of the most popular web servers in the world and is responsible for hosting some of the largest and highest-traffic sites on the internet.
-   It is more resource-friendly than Apache in most cases and can be used as a web server or reverse proxy.

For more details about nginx and its implementation, go to [nginx branch](https://github.com/CoderChirag/DevOps-Learning/tree/nginx)
