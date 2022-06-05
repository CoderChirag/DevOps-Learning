# Contents

---

- [Contents](#contents)
- [Introduction to Cloud Computing](#introduction-to-cloud-computing)
  - [What is Cloud Computing ?](#what-is-cloud-computing-)
  - [Who is using Cloud Computing ?](#who-is-using-cloud-computing-)
  - [Benefits of Cloud Computing](#benefits-of-cloud-computing)
    - [Agility](#agility)
    - [Elasticity](#elasticity)
    - [Cost Savings](#cost-savings)
    - [Deploy globally in minutes](#deploy-globally-in-minutes)
- [Types of Cloud Computing](#types-of-cloud-computing)
  - [Infrastructure as a Service (IaaS)](#infrastructure-as-a-service-iaas)
  - [Platform as a Service (PaaS)](#platform-as-a-service-paas)
  - [Software as a Service (SaaS)](#software-as-a-service-saas)
- [Introduction to AWS](#introduction-to-aws)
  - [AWS Global Infrastructure](#aws-global-infrastructure)
    - [Availability Zones (AZ)](#availability-zones-az)
    - [Regions](#regions)
- [AWS Account Setup](#aws-account-setup)
  - [Creating Free Tier Account](#creating-free-tier-account)
  - [Setting Up IAM with MFA](#setting-up-iam-with-mfa)
  - [Setting up Biling Alarms](#setting-up-biling-alarms)
  - [Setting Up a SSL Certificate for our domain](#setting-up-a-ssl-certificate-for-our-domain)

---

# Introduction to Cloud Computing

## What is Cloud Computing ?

Cloud computing is the on-demand delivery of IT resources over the Internet with pay-as-you-go pricing. Instead of buying, owning, and maintaining physical data centers and servers, you can access technology services, such as computing power, storage, and databases, on an as-needed basis from a cloud provider like Amazon Web Services (AWS).

## Who is using Cloud Computing ?

Organizations of every type, size, and industry are using the cloud for a wide variety of use cases, such as data backup, disaster recovery, email, virtual desktops, software development and testing, big data analytics, and customer-facing web applications. For example, healthcare companies are using the cloud to develop more personalized treatments for patients. Financial services companies are using the cloud to power real-time fraud detection and prevention. And video game makers are using the cloud to deliver online games to millions of players around the world.

## Benefits of Cloud Computing

### Agility

<div align="center">

![agility](./images/agility.png)

</div>

The cloud gives us easy access to a broad range of technologies so that we can innovate faster and build nearly anything that we can imagine. We can quickly spin up resources as we need them–from infrastructure services, such as compute, storage, and databases, to Internet of Things, machine learning, data lakes and analytics, and much more.

We can deploy technology services in a matter of minutes, and get from idea to implementation several orders of magnitude faster than before. This gives us the freedom to experiment, test new ideas to differentiate customer experiences, and transform our business.

### Elasticity

<div align="center">

![elasticity](./images/elasticity.png)

</div>

With cloud computing, we don’t have to over-provision resources up front to handle peak levels of business activity in the future. Instead, we provision the amount of resources that we actually need. We can scale these resources up or down to instantly grow and shrink capacity as our business needs change.

### Cost Savings

<div align="center">

![cost_savings](./images/cost_savings.png)

</div>

The cloud allows us to trade fixed expenses (such as data centers and physical servers) for variable expenses, and only pay for IT as you consume it. Plus, the variable expenses are much lower than what we would pay to do it ourself because of the economies of scale.

### Deploy globally in minutes

<div align="center">

![deploy](./images/deploy.png)

</div>

With the cloud, we can expand to new geographic regions and deploy globally in minutes. For example, AWS has infrastructure all over the world, so we can deploy our application in multiple physical locations with just a few clicks. Putting applications in closer proximity to end users reduces latency and improves their experience.

# Types of Cloud Computing

The three main types of cloud computing include **Infrastructure as a Service**, **Platform as a Service**, and **Software as a Service**. Each type of cloud computing provides different levels of control, flexibility, and management so that we can select the right set of services for our needs.

## Infrastructure as a Service (IaaS)

<div align="center">

![iaas](./images/iaas.png)

</div>

IaaS contains the basic building blocks for cloud IT. It typically provides access to networking features, computers (virtual or on dedicated hardware), and data storage space. IaaS gives us the highest level of flexibility and management control over our IT resources. It is most similar to the existing IT resources with which many IT departments and developers are familiar.

## Platform as a Service (PaaS)

<div align="center">

![paas](./images/paas.png)

</div>

PaaS removes the need for us to manage underlying infrastructure (usually hardware and operating systems), and allows us to focus on the deployment and management of our applications. This helps us be more efficient as we don’t need to worry about resource procurement, capacity planning, software maintenance, patching, or any of the other undifferentiated heavy lifting involved in running our application.

## Software as a Service (SaaS)

<div align="center">

![saas](./images/saas.png)

</div>

SaaS provides us with a complete product that is run and managed by the service provider. In most cases, people referring to SaaS are referring to end-user applications (such as web-based email). With a SaaS offering, we don’t have to think about how the service is maintained or how the underlying infrastructure is managed. We only need to think about how we will use that particular software.

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
