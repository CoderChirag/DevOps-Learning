# AWS Cloud for Project Set Up | Lift & Shift

## Contents

- [AWS Cloud for Project Set Up | Lift & Shift](#aws-cloud-for-project-set-up--lift--shift)
  - [Contents](#contents)
  - [About The Project](#about-the-project)
  - [AWS Services to be Used](#aws-services-to-be-used)
  - [Objective](#objective)
  - [Architecture](#architecture)
  - [Flow of Execution](#flow-of-execution)
  - [Security Groups and Keypairs](#security-groups-and-keypairs)
    - [ACM](#acm)
    - [Security Groups](#security-groups)
    - [Key Pairs](#key-pairs)
  - [EC2 Instances](#ec2-instances)
    - [MySql Instance](#mysql-instance)
    - [MemCache Instance](#memcache-instance)
    - [RabbitMQ Instance](#rabbitmq-instance)
    - [Route 53](#route-53)
    - [Tomcat Instance](#tomcat-instance)

## About The Project

-   Multi Tier Web Application Stack [VPROFILE]
-   Host & Run on AWS Cloud for Production
-   Lift & Shift Strategy

## AWS Services to be Used

-   EC2 Instances - VM for Tomcat, RabbitMQ, Memcahced, MySQL
-   Elastic Load Balancer (ELB) - Replacement for Nginx Load Balancers
-   Autoscaling - Automation for VM Scaling
-   S3/EFS Storage - Shared Storage
-   Route 53 - Private DNS Service
-   IAM, ACM, EBS etc

## Objective

-   Flexible Infrastructure
-   No Upfront Cost
-   Modernize Effectively
-   Infrastructure as a Code (IAAC) - Automation

## Architecture

-   Users will access our website by using a URL, and URL would point to an Elastic Load Balancer (ELB) endpoint. This entry would be added in the DNS provider.
-   User browser will use this endpoint to connect an Application Load Balancer using HTTPS. The certificate for HTTPS will be mentioned in Amazon Certificate Manager (ACM) service.
-   Application Load Balancer would route the request to Tomcat Instances running on `port 8080`. Apache Tomcat servers would be running on some EC2 Instances which would be managed by our Auto Scaling group.
-   S3 Bucket would be used to store our software artifacts attached to Tomcat Instances.
-   Information of backend services of the backend server IP Address will be mentioned in a Route 53 Private DNS Zone and our Tomcat servers would use this to connect with backend servers.
-   These backend servers (EC2 instances) running MySQL, RabbitMQ and Memcache would be in separate security group.

## Flow of Execution

-   Login to AWS Account
-   Create Key Pairs
-   Create Security Groups
-   Launch Instances with user data [BASH SCRIPTS]
-   Update IP to name mapping in Route 53
-   Build Application from source code
-   Upload to S3 bucket
-   Download artifact to Tomcat EC2 Instance
-   Setup ELB with HTTPS [Cert from Amazon Certificate Manager]
-   Map ELB Endpoint to website name in our DNS provider.
-   Verify

**Note**

-   Here we will talk about just what to do for hosting this multi-tier web application.
-   But for how to do part, you have to visit the [aws branch](https://github.com/CoderChirag/DevOps-Learning/tree/aws). There we have covered everything in detail.
-   So make sure you have read that branch thoroughly before coming to this.

## Security Groups and Keypairs

### ACM

-   Before begining, make sure that you have generated an Certificate from Amazon Certificate Manager (ACM). If not, refer to [aws branch](https://github.com/CoderChirag/DevOps-Learning/tree/aws#contents) in the **AWS Account Setup** > **Setting Up a SSL Certificate for our domain** section for learning how to create it.

### Security Groups

-   Create a **Security Group for our Load Balancers**, named `vprofile-ELB-sg` having inboud rules for `HTTP (port:80)` and `HTTPS (port:443)` from `anywhere (0.0.0.0/0 and ::/0)`.
-   Create a **Security Group for tomcat instances**, named `vprofile-app-sg` having inbound rules : `Custom TCP` with `port:8080` with the source as security group of our ELB, i.e, `vprofile-ELB-sg`.
-   Create a **Security Group for backend services (Memcached, RabbitMQ, and MySQL)**, named `vprofile-backend-sg` having inbound rules :
    -   **Type** `MYSQL/Aurora`, **Protocol** `TCP`, **Port range** `3306`, **Source** `custom (vprofile-app-sg)`.
    -   **Type** `Custom TCP`, **Protocol** `TCP`, **Port range** `11211`, **Source** `custom (vprofile-app-sg)`.
    -   **Type** `Custom TCP`, **Protocol** `TCP`, **Port range** `5672`, **Source** `custom (vprofile-app-sg)`.
-   Now go to edit inbound rules of `vprofile-backend-sg` and add 1 more inbound rule :
    -   **Type** `All Traffic`, **Protocol** `All`, **Port range** `All`, **Source** `custom (vprofile-backend-sg)`. This is to allow all traffic from the same security group, to allow internal traffic to flow on all ports.

### Key Pairs

-   Create a key pair named `vprofile-prod-key` which we will use later to connect to our instances.

## EC2 Instances

### MySql Instance

-   Create an instance for MySql with following config :
    -   **Name :** `vprofile-db01`
    -   **AMI :** `CentOs 7`
    -   **Instance Type :** `t2.mirco`
    -   **Key Pair :** `vprofile-prod-key`
    -   **Security Group :** `vprofile-backend-sg`
    -   **EBS Volume :** `10GB`
    -   **Termination Protection :** `enable`
    -   **User Data :** Copy from the script `/userdata/mysql.sh` from the code files given above.
-   To confirm that instance is set up properly, add an inbound rule in the `vprofile-backend-sg` security group of type `SSH` on `My IP`.
-   Now, go to the folder where your key is present and open git bash and connect to the Instance using the public IP Address :

    ```
      $ ssh -i vprofile-prod-key.pem centos@<public-ip>

      $ sudo -i
      $ curl http://169.254.169.254/latest/user-data    #This will show the provided user data script

      $ systemctl status mariadb      #Confirm that the service is running

      #Log in to database and verify everything
      Welcome to the MariaDB monitor.  Commands end with ; or \g.
      Your MariaDB connection id is 4
      Server version: 5.5.68-MariaDB MariaDB Server

      Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

      Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

      MariaDB [(none)]> show databases;
      +--------------------+
      | Database           |
      +--------------------+
      | information_schema |
      | accounts           |
      | mysql              |
      | performance_schema |
      | test               |
      +--------------------+
      5 rows in set (0.00 sec)

      MariaDB [(none)]> use accounts;
      Reading table information for completion of table and column names
      You can turn off this feature to get a quicker startup with -A

      Database changed
      MariaDB [accounts]> show tables;
      +--------------------+
      | Tables_in_accounts |
      +--------------------+
      | role               |
      | user               |
      | user_role          |
      +--------------------+
      3 rows in set (0.00 sec)

      MariaDB [accounts]> exit
      Bye
    ```

### MemCache Instance

-   Create an instance for MySql with following config :
    -   **Name :** `vprofile-mc01`
    -   **AMI :** `CentOs 7`
    -   **Instance Type :** `t2.mirco`
    -   **Key Pair :** `vprofile-prod-key`
    -   **Security Group :** `vprofile-backend-sg`
    -   **EBS Volume :** `10GB`
    -   **Termination Protection :** `enable`
    -   **User Data :** Copy from the script `/userdata/memcache.sh` from the code files given above.
-   To confirm that instance is set up properly, go to the folder where your key is present and open git bash and connect to the Instance using the public IP Address :

    ```
      $ ssh -i vprofile-prod-key.pem centos@<public-ip>

      $ sudo -i
      $ curl http://169.254.169.254/latest/user-data    #This will show the provided user data script

      $ systemctl status memcached     #Confirm that the service is running
    ```

### RabbitMQ Instance

-   Create an instance for MySql with following config :
    -   **Name :** `vprofile-rmq01`
    -   **AMI :** `CentOs 7`
    -   **Instance Type :** `t2.mirco`
    -   **Key Pair :** `vprofile-prod-key`
    -   **Security Group :** `vprofile-backend-sg`
    -   **EBS Volume :** `10GB`
    -   **Termination Protection :** `enable`
    -   **User Data :** Copy from the script `/userdata/rabbitmq.sh` from the code files given above.
-   To confirm that instance is set up properly, go to the folder where your key is present and open git bash and connect to the Instance using the public IP Address :

    ```
      $ ssh -i vprofile-prod-key.pem centos@<public-ip>

      $ sudo -i
      $ curl http://169.254.169.254/latest/user-data    #This will show the provided user data script

      $ systemctl status rabbitmq-server     #Confirm that the service is running
    ```

### Route 53

-   Now create a **hosted zone** using Route 53 with following configurations :
    -   **Domain name :** `vprofile.in`
    -   **Description :** `Hosted one for vprofile backend servers`
    -   **Type :** `Private hosted zone`
    -   **Region :** `US East (N.Virginia)[us-east-1]` and select the default VPC.
-   Now **Create Records** giving the configs :
    -   **Record Name** `db01`, **Value :** Internal IP address of `vprofile-db01`, **Routing Policy :** `Simple routing`
    -   **Record Name** `mc01`, **Value :** Internal IP address of `vprofile-mc01`, **Routing Policy :** `Simple routing`
    -   **Record Name** `rmq01`, **Value :** Internal IP address of `vprofile-rmq01`, **Routing Policy :** `Simple routing`

### Tomcat Instance

-   Create an instance for MySql with following config :
    -   **Name :** `vprofile-app01`
    -   **AMI :** `Ubuntu 18`
    -   **Instance Type :** `t2.mirco`
    -   **Key Pair :** `vprofile-prod-key`
    -   **Security Group :** `vprofile-app-sg`
    -   **EBS Volume :** `8GB`
    -   **Termination Protection :** `enable`
    -   **User Data :** Copy from the script `/userdata/tomcat_ubuntu.sh` from the code files given above.
-   To confirm that instance is set up properly, add an inbound rule in the `vprofile-app-sg` security group of type `SSH` on `My IP`.
-   Now, go to the folder where your key is present and open git bash and connect to the Instance using the public IP Address :

    ```
      $ ssh -i vprofile-prod-key.pem ubuntu@<public-ip>

      $ sudo -i
      $ curl http://169.254.169.254/latest/user-data    #This will show the provided user data script
    ```
