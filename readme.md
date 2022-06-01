# Contents

---

- [Contents](#contents)
- [Nginx Web Server](#nginx-web-server)
  - [Step 1 - Installing Nginx](#step-1---installing-nginx)
  - [Step 2 - Adjusting the Firewall](#step-2---adjusting-the-firewall)
  - [Step 3 - Checking the Web Server](#step-3---checking-the-web-server)
  - [Step 4 - Managing the Nginx Process](#step-4---managing-the-nginx-process)
  - [Step 5 - Setting up Server Blocks (Recommended)](#step-5---setting-up-server-blocks-recommended)
  - [Step 6 – Getting Familiar with Important Nginx Files and Directories](#step-6--getting-familiar-with-important-nginx-files-and-directories)
    - [Content](#content)
    - [Server Configuration](#server-configuration)
    - [Sever Logs](#sever-logs)

---

# Nginx Web Server

If you don't know how to create above mentioned VM then please head back to [vagrant branch](https://github.com/CoderChirag/DevOps-Learning/tree/vagrant) first.

## Step 1 - Installing Nginx

-   ```
    $ sudo apt update
    $ sudo apt install nginx
    ```

## Step 2 - Adjusting the Firewall

-   Before testing Nginx, the firewall software needs to be adjusted to allow access to the service.
-   Firstly enable ufw firewall : `$ sudo ufw enable`
-   Nginx registers itself as a service with `ufw` upon installation, making it straightforward to allow Nginx access.
    ```
    $ sudo ufw app list
    Available applications:
        Nginx Full
        Nginx HTTP
        Nginx HTTPS
        OpenSSH
    ```
-   This list displays three profiles available for Nginx:
    -   **Nginx Full :** This profile opens both `port 80` (normal, unencrypted web traffic) and `port 443` (TLS/SSL encrypted traffic)
    -   **Nginx HTTP :** This profile opens only `port 80` (normal, unencrypted web traffic)
    -   **Nginx HTTPS :** This profile opens only `port 443` (TLS/SSL encrypted traffic)
-   It is recommended that we enable the most restrictive profile that will still allow the traffic we’ve configured.

    ```
    $ sudo ufw allow 'Nginx HTTP'
    $ sudo ufw status
    Status: active

    To                         Action      From
    --                         ------      ----
    OpenSSH                    ALLOW       Anywhere
    Nginx HTTP                 ALLOW       Anywhere
    OpenSSH (v6)               ALLOW       Anywhere (v6)
    Nginx HTTP (v6)            ALLOW       Anywhere (v6)
    ```

-   The **Nginx HTTP** profile has now been activated to allow access to the web server.

For more detailed study about the ufw and firewall settings, go to the [ufw branch](https://github.com/CoderChirag/DevOps-Learning/tree/ufw)

## Step 3 - Checking the Web Server

-   Check with the `systemd` init system to make sure the service is running:
    ```
    $ sudo systemctl status nginx
    ● nginx.service - A high performance web server and a reverse proxy server
    Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: en
    Active: active (running) since Fri 2021-10-01 21:36:15 UTC; 35s ago
     Docs: man:nginx(8)
    Main PID: 9039 (nginx)
    Tasks: 2 (limit: 1151)
    CGroup: /system.slice/nginx.service
           ├─9039 nginx: master process /usr/sbin/nginx -g daemon on; master_pro
           └─9041 nginx: worker process
    ```
-   For checking, go to browser and visit the bridged or static IP and verify that it is showing the Nginx default page.

<div align='center'>

![nginx_default_page](./images/default_page.png)

</div>

## Step 4 - Managing the Nginx Process

-   To stop the web server : `$ sudo systemctl stop nginx`
-   To start the web server : `$ sudo systemctl start nginx`
-   To restart the web server : `$ sudo systemctl restart nginx`
-   To update the configuration changes : `$ sudo systemctl reload nginx`
-   To disable on startup : `$ sudo systemctl disable nginx`
-   To enable on startup : `$ sudo systemctl enable nginx`

## Step 5 - Setting up Server Blocks (Recommended)

-   When using the Nginx web server, server blocks (similar to virtual hosts in Apache) can be used to encapsulate configuration details and host more than one domain from a single server.
-   Nginx on Ubuntu 18.04 has one server block enabled by default that is configured to serve documents out of a directory at `/var/www/html`.
-   While this works well for a single site, it can become unwieldy if you are hosting multiple sites.
-   Instead of modifying `/var/www/html`, let’s create a directory structure within `/var/www` for our **your_domain** site, leaving `/var/www/html` in place as the default directory to be served if a client request doesn’t match any other sites.
-   ```
    $ sudo mkdir -p /var/www/your_domain/html
    $ sudo chown -R $USER:$USER /var/www/your_domain/html
    $ sudo chmod -R 755 /var/www/your_domain
    $ vim /var/www/your_domain/html/index.html
    ```
-   Inside `index.html`, add the following content :
    ```
    <html>
        <head>
            <title>Welcome to your_domain!</title>
        </head>
        <body>
            <h1>Success! The your_domain server block is working!</h1>
        </body>
    </html>
    ```
-   Add the following configuration block to `/etc/nginx/sites-available/your_domain`

    ```
    server {
        listen 80;
        listen [::]:80;

        root /var/www/your_domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain.com www.your_domain;

        location / {
                try_files $uri $uri/ =404;
        }
    }
    ```

-   Next, enable the file by creating a link from it to the `sites-enabled` directory, which Nginx reads from during startup: <br> `$ sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled`
-   Two server blocks are now enabled and configured to respond to requests based on their `listen` and `server_name` directives :

    -   `your_domain` : Will respond to requests for `your_domain` and `www.your_domain`.
    -   `default` : Will respond to any requests on `port 80` that do not match the other two blocks.

-   To avoid a possible **hash bucket memory problem** that can arise from adding additional server names, it is necessary to adjust a single value in the `/etc/nginx/nginx.conf` file :
    ```
    ...
    http {
        ...
        server_names_hash_bucket_size 64;   # Find and Uncomment this line
        ...
    }
    ...
    ```
-   Next, test to make sure that there are no syntax errors in any of your Nginx files : `$ sudo nginx -t`
-   If there aren’t any problems, restart Nginx to enable the changes : `$ sudo systemctl restart nginx`
-   Nginx should now be serving **your_domain** name. We can test this by navigating to `http://your_domain`, where we should see something like the following:

<div align='center'>

![success](./images/success.png)

</div>

## Step 6 – Getting Familiar with Important Nginx Files and Directories

### Content

-   `/var/www/html` : The actual web content, which by default only consists of the default Nginx page we saw earlier, is served out of the `/var/www/html` directory. This can be changed by altering Nginx configuration files

### Server Configuration

-   `/etc/nginx` : The Nginx configuration directory. All of the Nginx configuration files reside here.
-   `/etc/nginx/nginx.conf` : The main Nginx configuration file. This can be modified to make changes to the Nginx global configuration.
-   `/etc/nginx/sites-available/` : The directory where per-site server blocks can be stored. Nginx will not use the configuration files found in this directory unless they are linked to the `sites-enabled` directory. Typically, all server block configuration is done in this directory, and then enabled by linking to the other directory.
-   `/etc/nginx/sites-enabled/` : The directory where enabled per-site server blocks are stored. Typically, these are created by linking to configuration files found in the `sites-available` directory.
-   `/etc/nginx/snippets` : This directory contains configuration fragments that can be included elsewhere in the Nginx configuration. Potentially repeatable configuration segments are good candidates for refactoring into snippets.

### Sever Logs

-   `/var/log/nginx/access.log` : Every request to our web server is recorded in this log file unless Nginx is configured to do otherwise.
-   `/var/log/nginx/error.log` : Any Nginx errors will be recorded in this log.

With this we have successfully installed and configured Nginx Web Server :sunglasses:
