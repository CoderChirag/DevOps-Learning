# Contents

---

- [Contents](#contents)
- [Apache Web Server](#apache-web-server)
  - [Introduction](#introduction)
  - [Prerequisites](#prerequisites)
  - [Step 1 - Installing Apache](#step-1---installing-apache)
  - [Step 2 - Adjusting the Firewall](#step-2---adjusting-the-firewall)
  - [Step 3 - Checking the Web Server](#step-3---checking-the-web-server)
  - [Step 4 - Managing the Apache Process](#step-4---managing-the-apache-process)
  - [Step 5 - Setting up Virtual Hosts (Recommended)](#step-5---setting-up-virtual-hosts-recommended)
  - [Step 6 - Getting Familiar with Important Apache Files and Directories](#step-6---getting-familiar-with-important-apache-files-and-directories)
    - [Content](#content)
    - [Server Configuration](#server-configuration)
    - [Server Logs](#server-logs)

---

# Apache Web Server

## Introduction

-   The Apache HTTP server is the most widely-used web server in the world.
-   It provides many powerful features including dynamically loadable modules, robust media support, and extensive integration with other popular software.

## Prerequisites

-   A ubuntu-18 vagrant machine with a bridged and static IP configured using Vagrantfile.

If you don't know how to create above mentioned VM then please head back to [vagrant branch](https://github.com/CoderChirag/DevOps-Learning/tree/vagrant) first.

## Step 1 - Installing Apache

-   `$ sudo apt update`
-   `$ sudo apt install apache2`

## Step 2 - Adjusting the Firewall

-   Before testing Apache, it’s necessary to modify the firewall settings to allow outside access to the default web ports
-   Enable UFW firewall : `$ sudo ufw enable`
-   During installation, Apache registers itself with UFW to provide a few application profiles that can be used to enable or disable access to Apache through the firewall.
    ```
    $ sudo ufw app list
    Available applications:
      Apache
      Apache Full
      Apache Secure
      OpenSSH
    ```
-   This list indicates that there are three profiles available for Apache:
    -   **Apache :** This profile opens only port 80 (normal, unencrypted web traffic)
    -   **Apache Full :** This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)
    -   **Apache Secure :** This profile opens only port 443 (TLS/SSL encrypted traffic)
-   It is recommended that we enable the most restrictive profile that will still allow the traffic we’ve configured. Since we haven’t configured SSL for our server yet in this guide, we will only need to allow traffic on port 80:

    ```
    $ sudo ufw allow 'Apache'
    $ sudo ufw status
    Status: active

    To                         Action      From
    --                         ------      ----
    OpenSSH                    ALLOW       Anywhere
    Apache                     ALLOW       Anywhere
    OpenSSH (v6)               ALLOW       Anywhere (v6)
    Apache (v6)                ALLOW       Anywhere (v6)
    ```

-   The `Apache` profile has now been activated to allow access to the web server.

For more detailed study about the `ufw` and firewall settings, go to the [ufw branch](https://github.com/CoderChirag/DevOps-Learning/tree/ufw)

## Step 3 - Checking the Web Server

-   Check with the `systemd` init system to make sure the service is running :
    ```
    $ sudo systemctl status apache2
    ● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset:
    Drop-In: /lib/systemd/system/apache2.service.d
             └─apache2-systemd.conf
     Active: active (running) since Tue 2021-09-28 16:52:56 UTC; 1min 14s ago
    Main PID: 9409 (apache2)
      Tasks: 55 (limit: 4915)
     CGroup: /system.slice/apache2.service
             ├─9409 /usr/sbin/apache2 -k start
             ├─9410 /usr/sbin/apache2 -k start
             └─9411 /usr/sbin/apache2 -k start
    ```
-   For checking, go to browser and visit the bridged or static IP and verify that it is showing the Apache Ubuntu default page.

## Step 4 - Managing the Apache Process

-   Stop the web server : `$ sudo systemctl stop apache2`
-   Start the web server : `$ sudo systemctl start apache2`
-   Restart the web server : `$ sudo systemctl restart apache2`
-   Reload to update the config changes : `sudo systemctl reload apache2`
-   Disable the start of web server on system boot : `sudo systemctl disable apache2`
-   Enable the web server to run on system boot : `sudo systemctl enable apache2`

## Step 5 - Setting up Virtual Hosts (Recommended)

-   When using the Apache web server, we can use virtual hosts (similar to server blocks in Nginx) to encapsulate configuration details and host more than one domain from a single server.
-   Apache on Ubuntu 18.04 has one server block enabled by default that is configured to serve documents from the `/var/www/html` directory.
-   While this works well for a single site, it can become unwieldy if you are hosting multiple sites.
-   Instead of modifying `/var/www/html`, create a directory structure within `/var/www` for a **your_domain** site, leaving `/var/www/html` in place as the default directory to be served if a client request doesn’t match any other sites.
-   Create the directory for your_domain as follows :<br>`$ sudo mkdir /var/www/your_domain`
-   Next, assign ownership of the directory with the `$USER` environment variable :<br>`$ sudo chown -R $USER:$USER /var/www/your_domain`
-   The permissions of our web roots should be correct if we haven’t modified our unmask value, but we can make sure by typing the following:<br>`$ sudo chmod -R 755 /var/www/your_domain`
-   Next, create a sample `/var/www/your_domain/index.html` page using `vim`. Add the folowing content in it :

```
<html>
  <head>
      <title>Welcome to Your_domain!</title>
  </head>
  <body>
      <h1>Success!  The your_domain virtual host is working!</h1>
  </body>
</html>
```

-   In order for Apache to serve this content, it’s necessary to create a virtual host file with the correct directives. Instead of modifying the default configuration file located at `/etc/apache2/sites-available/000-default.conf` directly, make a new one at `/etc/apache2/sites-available/your_domain.conf`:
    ```
    <VirtualHost *:80>
      ServerAdmin webmaster@localhost
      ServerName your_domain
      ServerAlias www.your_domain
      DocumentRoot /var/www/your_domain
      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
    ```
-   Enable the file with the `a2ensite` tool :<br> `$ sudo a2ensite your_domain.conf`
-   Disable the default site defined in `000-default.conf` : <br> `$ sudo a2dissite 000-default.conf`
-   Test for configuration errors :
    ```
    $ sudo apache2ctl configtest
    Syntax OK
    ```
-   Restart Apache to implement your changes :<br> `$ sudo systemctl restart apache2`

Apache should now serve your domain name. You can test this by navigating to http://your_domain, where you should receive something like the following:

<div align='center'>

![success](./images/success.png)

</div>

## Step 6 - Getting Familiar with Important Apache Files and Directories

### Content

-   `/var/www/html` : The actual web content, which by default only consists of the default Apache page we saw earlier, is served out of the `/var/www/html` directory. This can be changed by altering Apache configuration files.

### Server Configuration

-   `/etc/apache2` : The Apache configuration directory. All of the Apache configuration files reside here.
-   `/etc/apache2/apache2.conf` : The main Apache configuration file. This can be modified to make changes to the Apache global configuration. This file is responsible for loading many of the other files in the configuration directory.
-   `/etc/apache2/ports.conf` : This file specifies the ports that Apache will listen on. By default, Apache listens on `port 80` and additionally listens on `port 443` when a module providing SSL capabilities is enabled.
-   `/etc/apache2/sites-available/` : The directory where per-site virtual hosts can be stored. Apache will not use the configuration files found in this directory unless they are linked to the `sites-enabled` directory. Typically, all server block configuration is done in this directory and then enabled by linking to the other directory with the `a2ensite` command.
-   `/etc/apache2/sites-enabled/`: The directory where enabled per-site virtual hosts are stored. Typically, these are created by linking to configuration files found in the `sites-available` directory with the `a2ensite`. Apache reads the configuration files and links found in this directory when it starts or reloads to compile a complete configuration.
-   `/etc/apache2/conf-available/`, `/etc/apache2/conf-enabled/` : These directories have the same relationship as the `sites-available` and `sites-enabled` directories, but are used to store configuration fragments that do not belong in a virtual host. Files in the `conf-available` directory can be enabled with the `a2enconf` command and disabled with the `a2disconf` command.
-   `/etc/apache2/mods-available`/, `/etc/apache2/mods-enabled/` : These directories contain the available and enabled modules, respectively. Files ending in `.load` contain fragments to load specific modules, while files ending in `.conf` contain the configuration for those modules. Modules can be enabled and disabled using the `a2enmod` and `a2dismod` commands.

### Server Logs

-   `/var/log/apache2/access.log` : By default, every request to our web server is recorded in this log file unless Apache is configured to do otherwise.
-   `/var/log/apache2/error.log` : By default, all errors are recorded in this file. The `LogLevel` directive in the Apache configuration specifies how much detail the error logs will contain.
