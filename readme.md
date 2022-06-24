# Contents

---

- [Contents](#contents)
- [Introduction to AWS](#introduction-to-aws)
  - [AWS Global Infrastructure](#aws-global-infrastructure)
    - [Availability Zones (AZ)](#availability-zones-az)
    - [Regions](#regions)
- [AWS Account Setup](#aws-account-setup)
  - [Creating Free Tier Account](#creating-free-tier-account)
  - [Setting Up IAM with MFA](#setting-up-iam-with-mfa)
  - [Setting up Biling Alarms](#setting-up-biling-alarms)
  - [Setting Up a SSL Certificate for our domain](#setting-up-a-ssl-certificate-for-our-domain)
- [Amazon Elastic Compute Cloud (EC2)](#amazon-elastic-compute-cloud-ec2)
  - [EC2 Pricing](#ec2-pricing)
  - [Components in EC2 Instance](#components-in-ec2-instance)
  - [Creating EC2 Instance](#creating-ec2-instance)
    - [Launching an EC2 Instance](#launching-an-ec2-instance)
      - [Creating the Instance](#creating-the-instance)
      - [Connecting to the Instance](#connecting-to-the-instance)
- [AWS CLI](#aws-cli)
  - [Downloading and Installing AWS CLI](#downloading-and-installing-aws-cli)
  - [Configuring IAM](#configuring-iam)
  - [Configuring AWS CLI](#configuring-aws-cli)
- [Elastic Block Storage](#elastic-block-storage)
  - [EBS Types](#ebs-types)
  - [Creating a new EBS Volume](#creating-a-new-ebs-volume)
  - [Configuring and Partitioning the Volume](#configuring-and-partitioning-the-volume)

---

# Introduction to AWS

## AWS Global Infrastructure

-   The AWS Global Cloud Infrastructure is the most secure, extensive, and reliable cloud platform, offering over **200** fully featured data centers globally.
-   AWS now spans 84 Availability Zones within 26 geographic regions around the world.

### Availability Zones (AZ)

-   An **Availability Zone (AZ)** is one or more discrete data centers with redundant power, networking, and connectivity in an AWS Region.
-   AZs give customers the ability to operate production applications and databases that are more highly available, fault tolerant, and scalable than would be possible from a single data center.
-   All AZs in an AWS Region are interconnected with high-bandwidth, low-latency networking, over fully redundant, dedicated metro fiber providing high-throughput, low-latency networking between AZs.
-   All traffic between AZs is encrypted.
-   The network performance is sufficient to accomplish synchronous replication between AZs.
    <br>
-   AZs make partitioning applications for high availability easy.
-   If an application is partitioned across AZs, companies are better isolated and protected from issues such as power outages, lightning strikes, tornadoes, earthquakes, and more.
-   AZs are physically separated by a meaningful distance, many kilometers, from any other AZ, although all are within 100 km (60 miles) of each other.

### Regions

-   **Region** is a physical location around the world where AWS cluster data centers.
-   Each AWS Region consists of multiple, isolated, and physically separate Availability Zones within a geographic area.
-   The multiple AZ design of every AWS Region offers advantages for customers.
-   Each AZ has independent power, cooling, and physical security and is connected via redundant, ultra-low-latency networks.
-   AWS customers focused on high availability can design their applications to run in multiple AZs to achieve even greater fault-tolerance.
-   AWS infrastructure Regions meet the highest levels of security, compliance, and data protection.
    <br>
-   AWS maintains multiple geographic Regions, including Regions in North America, South America, Europe, China, Asia Pacific, South Africa, and the Middle East.

<div align='center'>

![azs_and_regions](./images/azs_and_regions.jpg)

</div>

# AWS Account Setup

<!-- Free Tier Account, IAM with MFA, Billing Alarm , Certificate Setup -->

## Creating Free Tier Account

-   Go to aws.amazon.com and click on **Create an AWS Account** at the top right corner of the screen.

![create_account_1](./images/aws/create_account_1.jpg)

-   Fill in all the details and complete the signup process.

![create_account_2](./images/aws/create_account_2.jpg)

-   Click on **Sign in to the Console**.

![create_account_2](./images/aws/create_account_3.jpg)

-   Fill in the details and sign in to the root account.

## Setting Up IAM with MFA

-   After signing in, search for **IAM** service in the search box and click on it.

![mfa_1](./images/aws/mfa_1.jpg)

-   Click on Add MFA to setup Multi Factor Authentication for root user.
-   Click on Activate MFA > Virtual MFA Device > Continue. After that install any MFA app like **Google Authenticator** on your mobile and activate the MFA.

![mfa_2](./images/aws/mfa_2.jpg)

-   Now go to **Users** from the left navigation menu > Add Users.

![iam_1](./images/aws/iam_1.jpg)

-   Give some username
-   Select **Access Type** to **Password**

![iam_2](./images/aws/iam_2.jpg)

-   Select Autogenerate password
-   Go to Premissions section. Select Attach existing policies directly and select **AdministratorAccess**

![iam_3](./images/aws/iam_3.jpg)

-   Go next > next > create user
-   Copy the username and password somewhere.
-   Now go to dashboard from the left navigation panel and create an alias for the IAM account we just set up.

![iam_4](./images/aws/iam_4.jpg)

-   Now for setting MFA for this user, go to **Users** from left navigation panel, select the user we just created, go to **Security Credentials** section, and click on **Manage** option in the **Assigned MFA device** section.
-   Now, assign MFA for this user as we did for the root user.

![iam_5](./images/aws/iam_5.jpg)

## Setting up Biling Alarms

-   Firstly, select **Global region** from the top right section of the console and then open the dropdown menu of the profile (where your username is written), and go to **Billing Dashboard** page.

![billing_1](./images/aws/billing_1.jpg)

-   Go to **Billing prefrences** from the left navigation panel > Select **Recieve PDF invoice By Email**, **Recieve Free Tier Usage Alerts**, enter your email there, select **Recieve Billing Alerts**.
-   Now click on **Save Prefrences**.

![billing_2](./images/aws/billing_2.jpg)

-   Now search a service **Cloudwatch** from the top search bar and go to the cloudwatch page.
-   **Cloudwach** is basically a monitoring and observability service provided by AWS to monitor all your applications, respond to system-wide performance changes, and optimize resource utilization.

![cloudwatch_1](./images/aws/cloudwatch_1.jpg)

-   Select the US East(N. Virginia) region.

![cloudwatch_2](./images/aws/cloudwatch_2.jpg)

-   Go to left navigation menu > Alarms > All Alarms.
-   Now on this page click on Create Alarm

![cloudwatch_3](./images/aws/cloudwatch_3.jpg)

-   Now click on Select Metric > Billing > Total Estimated Charge > select USD > Select metric.
-   Now go down the page, select **Threshold type** to **Static** and **Whenever EstimatedCharges is...** to **Greater** and then in the below input box write down **5**, and then click on next.
-   So with this we are creating a billing alar, so that whenever our bill crosses 5 USD we would be getting email notification.

![cloudwatch_4](./images/aws/cloudwatch_4.jpg)

-   Now create a new **SNS topic**, give it a name and email which will recieve the notification, and then click on create topic.
-   **SNS** stand for **Simple Notification Service**, and it is basically an AWS sevice which provides us email or sms notifications.

![cloudwatch_5](./images/aws/cloudwatch_5.jpg)

-   Click on next.
-   Give alarm a name and description and click on next and then click on Create Alarm.
-   So this will create an new alarm.
-   Now, go to your email, and confirm this subscription by clicking on the confirm subscription button on the email you would be getting from AWS.

## Setting Up a SSL Certificate for our domain

-   Search **acm** on the search bar and click on the **Certificate Manager** service.
-   Click on **Request a certificate**
-   Select Request a public certificate > click on next.
-   In the Domain names section, give `*.<your_domain>`.
-   Select **DNS validation**
-   Now click on Request.
-   Now click on the certificate you just created, scroll down and copy the **CNAME name** and **CNAME value** values, and create a corresponding record with these values in your domain.

So, with this we have setup our AWS Free Tier account, created an IAM user and set up MFA, set up Billing Alarms using CloudWatch, and create a SSL Certificate for our domain :sunglasses:.

# Amazon Elastic Compute Cloud (EC2)

-   **EC2** provides web services API for provisioning, managingm and deprovisioning virtual servers inside Amazon Cloud.
-   Ease in Scaling Up / Down.
-   Pay only for what we use.
-   Can be integrated into several other services.

## EC2 Pricing

1. On Demand
    - Pay per hour or seconds
2. Reserved
    - Reserve Capacity (1 or 3 yrs) for discounts
3. Spot
    - Bid your price for unused EC2 capacity
4. Dedicated Hosts
    - Physical Server dedicated for you

## Components in EC2 Instance

**Amazon Machine Image (AMI)**

-   Amazon Machine Image (AMI) is a supported and maintained image provided by AWS that provides the information required to launch an instance.

**Instance Type**

-   When we launch an instance, the instance type that we specify determines the hardware of the host computer used for our instance. Eg, M4 Instance, C4 Instance, F1 Instance, I3 Instance etc

**Amazon Elastic Block Store (EBS)**

-   Amazon Elastic Block Store (Amazon EBS) is an easy-to-use, scalable, high-performance block-storage service designed for Amazon Elastic Compute Cloud (Amazon EC2).
-   These are basically the **Virtual Hard Drive Volumes** which we can mount as devices on our instances.

**Tags**

-   Tag is a simpel label consisting of a customer-defined key and an optional value that can make it easier to manage, search for, and filter resources.

**Security Group**

-   A security group acts as a **virtual firewall** that controls the traffic for one or more instances.

**Key Pair**

-   Amazon EC2 uses **public-key cryptography** to encrypt and decrypt login information.
-   These are basically **SSH Keys** to connect to the instances remotely.

## Creating EC2 Instance

Steps for creation of an EC2 Instance:

1. Choose an Amazon Machine Image (AMI)
2. Choose an Instance Type
3. Configure the Instance
4. Add Storage
5. Add Tags
6. Configure Security Group
7. Review

![ec2_1](./images/aws/ec2_1.jpg)

### Launching an EC2 Instance

#### Creating the Instance

-   Log in to the AWS Console and switch to **North Virginia** region.
-   Search for EC2 in the search box and navigate to the Dashboard page of EC2.

![ec2_2](./images/aws/ec2__2.jpg)

-   Navigate to Instances from the left navigation menu.
-   Click on Launch Instance.
-   Give a **name** to the instance.
-   If you want you can give additional tags according to your project.
-   Now select an **AMI**. I will be going here with **Cent OS7**.
-   Then select an **Instance Type**. I will be chosing **t2 micro**.
-   Create a **key-pair**, download it and store it in a safe accessible location on your pc.
-   Configure the **Network Settings**. Give a **Security Group** and for security you can select the option **My IP** in the IP section.
-   Now configure the **Storage**.
-   Go to Advanced details > User Data. This is just like Vagrant Provisioning in the local setup. Whatever commands we give here, it will be executing that commands on the startup of the Instance.
    Give the following commands. (Make sure you have choosen Cent OS7 if you are using these commands).
    `#!/bin/bash sudo yum install http -y sudo systemctl start httpd sudo systemctl enable httpd mkdir /tmp/test1`
-   Now finally click on **Launch Instance**.

#### Connecting to the Instance

**Using SSH**

-   Open a git bash terminal on your system and type in the following commands.

    ```
    $ ssh -i <path_to_your_key> centos@<public_ip>
    # You can find your public IP by clicking on the instance you just created from the AWS console.

    # So with this we are logged in to our instance

    $ sudo systemctl status httpd
    ```

**Accessing via HTTP**

For accessing your instance via HTTP, we have to configure the security rules.
As of now, we have only one security rule through which our instance is listening on Port `22` (for SSH). So lets create a new security rule, for the Port `80` (for HTTP).

-   So select the Instance in the AWS Console.
-   Go to Security tab.
-   Click on the Security Group.
-   Go to **Inbound Rules** section and click on **Edit inbound rules**.
-   Click on Add rule.
-   Enter following info:
    -   **Type :** Custom TCP
    -   **Protocol :** TCP
    -   **Port Range :** 80
    -   **Source :** MY IP (if you are using it for private, else you can select Anywhere-IPv4 or Anywhere-IPv6 or both by creating 2 rules).
    -   **Description - Optional :** If you want you can give this also.
-   Then click on save rules.
-   Now type your instance's external IP Address on the Browser and you would see the Default Apache 2 page.

So with this we have created and set up an EC2 instance successfully :sunglasses:

**Note**

-   Don't forget to **terminate the instance** after using it.

# AWS CLI

The **AWS Command Line Interface** (AWS CLI) is a unified tool to manage our AWS services. With just one tool to download and configure, we can control multiple AWS services from the command line and automate them through scripts.

## Downloading and Installing AWS CLI

The downloading and installation of AWS CLI is covered in the [prereqs](https://github.com/coderchirag/DevOps-Learning/tree/prereqs) branch. Refer to that branch if you have not downloaded and installed AWS CLI on your machine.

## Configuring IAM

-   On the AWS console, search for **IAM** and open the IAM dashboard page.
-   Go to **Users section** from the left navigation panel and click on **Add User**.
    ![iam-add-user](./images/aws/iam-add-user.jpg)
-   Name the user as **awscli** and select the **Access key - Programmatic access**, and then click on **Next: Permissions** button.
    ![iam-add-user-awscli](./images/aws/iam-add-user-awscli.jpg)
-   Now select **Attach existing policies directly** and select **AdministratorAcess** from the policy list, and the **Next: Tags** button.
    ![iam-add-user-awscli-permissions](./images/aws/iam-add-user-awscli-permissions.jpg)
-   Click on **Next: Review** button > **Create User** button
-   Now download the credentials csv file and store it safely as we would be needing the credentials later, and then click on **close**.
    ![iam-add-user-awscli-credentials](./images/aws/iam-add-user-awscli-credentials.jpg)

## Configuring AWS CLI

-   Open git bash and type the following command to configure the **AWS CLI** so that it can authenticate us.

    ```
    $ aws configure
    AWS Access Key ID [None]: <your_access_key>
    AWS Secret Access Key [None]: <your_secret_key>
    Default region name [None]: us-east-1
    Default output format [None]: json

    $ ls ~/.aws
    config  credentials

    $ cat ~/.aws/config
    [default]
    region = us-east-1
    output = json

    $ cat ~/.aws/credentials
    [default]
    aws_access_key_id = <acess_key>
    aws_secret_access_key = <secret_key>
    ```

# Elastic Block Storage

-   **Amazon Elastic Block Store** (Amazon EBS) is an easy-to-use, scalable, high-performance block-storage service designed for Amazon Elastic Compute Cloud (Amazon EC2).
    ![ebs](./images/aws/ebs.png)

-   Block Based Storage.
-   Run EC2 OS, store data from db, file data etc.
-   Placed in specific Availability Zone (AZ) **replicated within the AZ** to protect from failure.
-   Can only be attached to the instances placed within the same Availability Zone (AZ).
-   Snapshot is backup of a volume.

## EBS Types

-   General Purpose (SSD)
    -   Most Work Loads
-   Provisioned IOPS
    -   Large Databases
-   Throughput Optimized HD
    -   Big Data & Data Warehouses
-   Cold HDD
    -   File Servers
-   Magnetic
    -   Backups & Archives

## Creating a new EBS Volume

-   From AWS console Go to **EC2** > **Elastic Block Store** > **Volumes** > **Create Volume**.
-   Now create a new **gp2** volume of **5 GB**.
-   After the volume is created, select it from the volumes dashboard, click on **Actions** > **Attach Volume** and the select the EC2 instance running.

## Configuring and Partitioning the Volume

-   SSH into your EC2 instance and follow the commands :
    <br>
-   Switch to sudo user : `$ sudo -i`
    <br>
-   List all the available disks :

    ```
    $ fdisk -l
    Disk /dev/xvda: 10.7 GB, 10737418240 bytes, 20971520 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x000b050e

        Device Boot      Start         End      Blocks   Id  System
    /dev/xvda1   *        2048    20971486    10484719+  83  Linux

    Disk /dev/xvdf: 5368 MB, 5368709120 bytes, 10485760 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    ```

    -   Here `/dev/xvda` is our root volume, which we alloted during creation of the instance. It is having a partition `/dev/xvda1` of size 10 GB, i.e., the entire disk size.
    -   Also, `/dev/xvdf` is the 5 GB General Purpose 2 (gp2) disk we just attached to our VM. At present it is not having any partition.
        <br>

-   To confirm that `/dev/xvda1` is the root volume.
    ```
    $ df  -h
    Filesystem      Size  Used Avail Use% Mounted on
    devtmpfs        471M     0  471M   0% /dev
    tmpfs           495M     0  495M   0% /dev/shm
    tmpfs           495M   13M  482M   3% /run
    tmpfs           495M     0  495M   0% /sys/fs/cgroup
    /dev/xvda1       10G  1.4G  8.7G  14% /
    tmpfs            99M     0   99M   0% /run/user/1000
    ```
    So we can clearly see that `/dev/xvda1` is mounted to `/` that is, the **root directory**.
    <br>
-   Now let's create a partition of `/dev/xvdf` disk using `fdisk` utility.

    ```
    $ fdisk /dev/xvdf
    Welcome to fdisk (util-linux 2.23.2).

    Changes will remain in memory only, until you decide to write them.
    Be careful before using the write command.

    Device does not contain a recognized partition table
    Building a new DOS disklabel with disk identifier 0xe84a5c8e.

    Command (m for help): m
    Command action
    a   toggle a bootable flag
    b   edit bsd disklabel
    c   toggle the dos compatibility flag
    d   delete a partition
    g   create a new empty GPT partition table
    G   create an IRIX (SGI) partition table
    l   list known partition types
    m   print this menu
    n   add a new partition
    o   create a new empty DOS partition table
    p   print the partition table
    q   quit without saving changes
    s   create a new empty Sun disklabel
    t   change a partition's system id
    u   change display/entry units
    v   verify the partition table
    w   write table to disk and exit
    x   extra functionality (experts only)

    Command (m for help): n
    Partition type:
    p   primary (0 primary, 0 extended, 4 free)
    e   extended
    Select (default p): p
    Partition number (1-4, default 1): 1
    First sector (2048-10485759, default 2048):
    Using default value 2048
    Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
    Using default value 10485759
    Partition 1 of type Linux and of size 5 GiB is set

    Command (m for help): p

    Disk /dev/xvdf: 5368 MB, 5368709120 bytes, 10485760 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0xe84a5c8e

        Device Boot      Start         End      Blocks   Id  System
    /dev/xvdf1            2048    10485759     5241856   83  Linux

    Command (m for help): w
    The partition table has been altered!

    Calling ioctl() to re-read partition table.
    Syncing disks.
    ```

    So with this, we have created a partition `/dev/xvdf1` of **10 GB** of the `/dev/xvdf` disk.
    <br>

-   Now format the disk using `ext4` format :

    ```
    $ mkfs.ext4 /dev/xvdf1
        mke2fs 1.42.9 (28-Dec-2013)
    Filesystem label=
    OS type: Linux
    Block size=4096 (log=2)
    Fragment size=4096 (log=2)
    Stride=0 blocks, Stripe width=0 blocks
    327680 inodes, 1310464 blocks
    65523 blocks (5.00%) reserved for the super user
    First data block=0
    Maximum filesystem blocks=1342177280
    40 block groups
    32768 blocks per group, 32768 fragments per group
    8192 inodes per group
    Superblock backups stored on blocks:
            32768, 98304, 163840, 229376, 294912, 819200, 884736

    Allocating group tables: done
    Writing inode tables: done
    Creating journal (32768 blocks): done
    Writing superblocks and filesystem accounting information: done
    ```

      <br>

-   Mounting our disk temporarily in `/media/xvdf1` dir temporarily and then unmounting it.

    ```
    $ mkdir -p /media/xvdf1
    $ mount /dev/xvdf1 /media/xvdf1
    $ df -h
    [root@ip-172-31-27-115 ~]# df -h
    Filesystem      Size  Used Avail Use% Mounted on
    devtmpfs        471M     0  471M   0% /dev
    tmpfs           495M     0  495M   0% /dev/shm
    tmpfs           495M   13M  482M   3% /run
    tmpfs           495M     0  495M   0% /sys/fs/cgroup
    /dev/xvda1       10G  1.4G  8.7G  14% /
    tmpfs            99M     0   99M   0% /run/user/1000
    /dev/xvdf1      4.8G   20M  4.6G   1% /media/xvdf1

    $ umount /dev/xvdf1
    ```

      <br>

-   Mounting the disk permanently so that it is not unmounted on the reboot of the VM by editing the `/etc/fstab` file.

    ```
    $ echo "/dev/xvdf1    /media/xvdf1    ext4 defaults 0 0" >> /etc/fstab
    $ cat /etc/fstab

    #
    # /etc/fstab
    # Created by anaconda on Sun Nov 14 11:52:41 2021
    #
    # Accessible filesystems, by reference, are maintained under '/dev/disk'
    # See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
    #
    UUID=44a6a613-4e21-478b-a909-ab653c9d39df /                       xfs     defaults        0 0
    /dev/xvdf1    /media/xvdf1    ext4 defaults 0 0

    $ mount -a
    $ df -h
    Filesystem      Size  Used Avail Use% Mounted on
    devtmpfs        471M     0  471M   0% /dev
    tmpfs           495M     0  495M   0% /dev/shm
    tmpfs           495M   13M  482M   3% /run
    tmpfs           495M     0  495M   0% /sys/fs/cgroup
    /dev/xvda1       10G  1.4G  8.7G  14% /
    tmpfs            99M     0   99M   0% /run/user/1000
    /dev/xvdf1      4.8G   20M  4.6G   1% /media/xvdf1
    ```
