# AWS Cloud for Project Set Up | Lift & Shift

## Contents

- [AWS Cloud for Project Set Up | Lift & Shift](#aws-cloud-for-project-set-up--lift--shift)
  - [Contents](#contents)
  - [About The Project](#about-the-project)
  - [AWS Services to be Used](#aws-services-to-be-used)
  - [Objective](#objective)
  - [Architecture](#architecture)
  - [Flow of Execution](#flow-of-execution)

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
-   Update IP to name mapping in route 53
-   Build Application from source code
-   Upload to S3 bucket
-   Download artifact to Tomcat EC2 Instance
-   Setup ELB with HTTPS [Cert from Amazon Certificate Manager]
-   Map ELB Endpoint to website name in our DNS provider.
-   Verify
