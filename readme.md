# Contents

---

- [Contents](#contents)
- [Initial Server Setup with Ubuntu 18.04](#initial-server-setup-with-ubuntu-1804)
  - [Introduction](#introduction)
- [Step 1 — Logging in as Root](#step-1--logging-in-as-root)
- [Step 2 - Creating a New User](#step-2---creating-a-new-user)
- [Step 3 - Granting Administrative Priviliges](#step-3---granting-administrative-priviliges)
- [Step 4 - Setting Up a Basic Firewall](#step-4---setting-up-a-basic-firewall)
- [Step 5 — Enabling External Access for Your Regular User](#step-5--enabling-external-access-for-your-regular-user)
  - [If the Root Account Uses Password Authentication](#if-the-root-account-uses-password-authentication)
  - [If the Root Account Uses SSH Key Authentication](#if-the-root-account-uses-ssh-key-authentication)

---

# Initial Server Setup with Ubuntu 18.04

## Introduction

-   After creating a new Ubuntu 18.04 server (on Physical Machine, **not** on vagrant VM), you should take some configuration steps as part of an initial server setup in order to increase security and facilitate management later.

# Step 1 — Logging in as Root

-   Newly installed servers typically have only a root account set up, and that is the account you’ll use to log into your server for the first time.
-   To get started, you’ll need to log into your server. Make sure you know your **server’s public IP address**.
-   To authenticate, you’ll need either the account’s password or the SSH private key for the **root** user’s account, in case you have set up an SSH key for authentication within the server.
-   SSH to your server : `$ ssh root@your_server_ip`
-   If using password authentication, provide the **root** password.

# Step 2 - Creating a New User

-   `$ adduser coder`

# Step 3 - Granting Administrative Priviliges

-   To add `sudo` privileges to your new user, you need to add the new user to the `sudo` group. By default on Ubuntu 18.04, users who belong to the `sudo` group are allowed to use the `sudo` command :<br> `$ usermod -aG sudo coder`

# Step 4 - Setting Up a Basic Firewall

-   **UFW (Uncomplicated Firewall)** is a firewall configuration tool that comes with Ubuntu servers. You can use the UFW firewall to make sure only connections to certain services are allowed on your server.
-   Applications can register their profiles with UFW upon installation. These profiles allow UFW to manage per-application settings by name.
-   OpenSSH, the service allowing you to connect to your server now, has a profile registered within UFW.
-   Run the following command to get a list of all current available profiles :
    ```
    $ ufw app list
    Available applications:
      OpenSSH
    ```
-   You need to make sure that the firewall allows SSH connections so that you can log back in next time. You can allow these connections by typing:<br> `$ ufw allow 'OpenSSH'`
-   Afterwards, you can enable the firewall with :<br>`$ ufw enable`
-   To see enabled firewall apps :

    ```
    $ ufw status
    Status: active

    To                         Action      From
    --                         ------      ----
    OpenSSH                    ALLOW       Anywhere
    OpenSSH (v6)               ALLOW       Anywhere (v6)
    ```

-   As **the firewall is currently blocking all connections except for SSH**, if you install and configure additional services, you will need to adjust the firewall settings to allow acceptable traffic in.

For more detailed study about **ufw**, refer to [ufw branch](https://github.com/CoderChirag/DevOps-Learning/tree/ufw)

# Step 5 — Enabling External Access for Your Regular User

## If the Root Account Uses Password Authentication

-   If you logged in to your **root** account using a password, it means that password authentication is enabled for SSH. You can SSH to your new user account by opening up a new terminal session and using SSH with your new username:<br>`$ ssh coder@your_server-ip`

## If the Root Account Uses SSH Key Authentication

-   If you logged in to your **root** account using SSH keys, it’s likely that password authentication is disabled for SSH.
-   You will need to add a copy of your local public key to the new user’s `~/.ssh/authorized_keys` file to log in successfully.
-   Since your public key is already in the root account’s `~/.ssh/authorized_keys` file on the server, you can copy that file and directory structure to your new user account in your existing session.
-   The simplest way to copy the files with the correct ownership and permissions is with the `rsync` command.
-   This will copy the **root** user’s `.ssh` directory, preserve the permissions, and modify the file owners, all in a single command : <br>`$ rsync --archive --chown=coder:coder ~/.ssh /home/coder`

    **NOTE**

    -   The `rsync` command treats sources and destinations that end with a trailing slash differently than those without a trailing slash. When using `rsync`, be sure that the source directory (`~/.ssh`) **does not** include a trailing slash (check to make sure you are not using `~/.ssh/`).
    -   If you accidentally add a trailing slash to the command, `rsync` will copy the contents of the root account’s `~/.ssh` directory to the sudo user’s home directory instead of copying the entire `~/.ssh` directory structure. The files will be in the wrong location and SSH will not be able to find and use them.

-   You should be now able to log into the new user account without being prompted for the remote user’s SSH password for authentication.
    -   If your SSH key was set up with a keyphrase, you may be asked to unlock the SSH key by providing that password when you use the key for the first time in a terminal session.

And with that we have successfully configured and secure our brand new Ubuntu Server :sunglasses:
