# Contents

- [Contents](#contents)
- [UFW Essentials: Common Firewall Rules and Commands](#ufw-essentials-common-firewall-rules-and-commands)
  - [Verify UFW Status](#verify-ufw-status)
  - [Enable UFW](#enable-ufw)
  - [Disable UFW](#disable-ufw)
  - [Block an IP Address](#block-an-ip-address)
  - [Block a Subnet](#block-a-subnet)
  - [Block Incoming Connections to a Network Interface](#block-incoming-connections-to-a-network-interface)
  - [Allow an IP Address](#allow-an-ip-address)
  - [Allow Incoming Connections to a Network Interface](#allow-incoming-connections-to-a-network-interface)
  - [Delete UFW Rule](#delete-ufw-rule)
  - [List Available Application Profiles](#list-available-application-profiles)
  - [Enable and Disable Application Profile](#enable-and-disable-application-profile)
  - [Allow SSH](#allow-ssh)
    - [Allow Incoming SSH from Specific IP Address or Subnet](#allow-incoming-ssh-from-specific-ip-address-or-subnet)
    - [Allow Incoming Rsync from Specific IP Address or Subnet](#allow-incoming-rsync-from-specific-ip-address-or-subnet)
  - [Allow All Incoming HTTP (port 80)](#allow-all-incoming-http-port-80)
  - [Allow All Incoming HTTPS (port 443)](#allow-all-incoming-https-port-443)
  - [Allow All Incoming HTTP and HTTPS](#allow-all-incoming-http-and-https)

# UFW Essentials: Common Firewall Rules and Commands

-   **UFW (uncomplicated firewall)** is a firewall configuration tool that runs on top of `iptables`, included by default within Ubuntu distributions.
-   It provides a streamlined interface for configuring common firewall use cases via the command line.

## Verify UFW Status

-   To check if `ufw` is enabled, run :
    ```
    $ sudo ufw status
    Status: inactive
    ```

## Enable UFW

-   If you got a `Status: inactive` message when running `ufw status`, it means the firewall is not yet enabled on the system. You’ll need to run a command to enable it :
    ```
    $ sudo ufw enable
    Firewall is active and enabled on system startup
    ```
-   To see what is currently blocked or allowed, you may use the `verbose` parameter when running `ufw status`, as follows:
    ```
    $ sudo ufw status verbose
    Status: active
    Logging: on (low)
    Default: deny (incoming), allow (outgoing), deny (routed)
    New profiles: skip
    ```

## Disable UFW

-   `$ sudo ufw disable`

## Block an IP Address

-   To block all network connections that originate from a specific IP address : <br> `$ sudo ufw deny from IP_ADDRESS #eg 203.0.113.100`
-   All connections, coming in or going out, are blocked for the specified IP address.

## Block a Subnet

-   If you need to block a full subnet, you may use the subnet address as `from` parameter on the `ufw deny` command : <br> `$ sudo ufw deny from IP_ADDRESS/subnetmask # eg 203.0.113.0/24`

## Block Incoming Connections to a Network Interface

-   To block incoming connections from a specific IP address to a specific network interface : <br> `$ sudo ufw deny in on eth0 from IP_ADDRESS`

## Allow an IP Address

-   To allow all network connections that originate from a specific IP address : <br> `$ sudo ufw allow from IP_ADDRESS`
-   Similarly, you can allow for a subnet

## Allow Incoming Connections to a Network Interface

-   To allow incoming connections from a specific IP address to a specific network interface : <br> `$ sudo ufw allow in on eth0 from IP_ADDRESS`

## Delete UFW Rule

-   To delete a rule that you previously set up within UFW : <br> `$ sudo ufw delete allow from IP_ADDRESS`
-   Another way to specify which rule you want to delete is by providing the rule ID.

    ```
    $ sudo ufw status numbered
    Status: active

       To                         Action      From
       --                         ------      ----
    [ 1] Anywhere                DENY IN     203.0.113.100
    [ 2] Anywhere on eth0        ALLOW IN    203.0.113.102

    $ sudo ufw delete 1
    ```

## List Available Application Profiles

-   Upon installation, applications that rely on network communications will typically set up a UFW profile that you can use to allow connection from external addresses : <br>`$ sudo ufw app list`

## Enable and Disable Application Profile

-   To enable a UFW application profile : <br> `$ sudo ufw allow 'OpenSSH'`
-   To Disable Application Profile : <br> `$ sudo ufw delete allow 'Nginx Full'`

## Allow SSH

-   Enable the OpenSSH UFW application profile and allow all connections to the default SSH port on the server: <br> `$ sudo ufw allow OpenSSH` or `$ sudo ufw allow 22 ## 22 is the port no of ssh service`

### Allow Incoming SSH from Specific IP Address or Subnet

-   Allow only **SSH connections** coming from the IP address/subnet `$ suod ufw allow from IP_ADDRESS/Subnet proto tcp to any port 22`

### Allow Incoming Rsync from Specific IP Address or Subnet

-   The `Rsync` program, runs on `port 873`, and can be used to transfer files from one computer to another.
-   `$ sudo ufw allow from IP_ADDRESS/SUBNET to any port 873`

## Allow All Incoming HTTP (port 80)

-   Web servers, such as Apache and Nginx, typically listen for HTTP requests on `port 80`.
-   If your default policy for incoming traffic is set to drop or deny, you’ll need to create a UFW rule to allow external access on `port 80` : <br> `$ sudo ufw allow http` or `$ sudo ufw allow 80`

## Allow All Incoming HTTPS (port 443)

-   HTTPS typically runs on `port 443`.
-   If your default policy for incoming traffic is set to drop or deny, you’ll need to create a UFW rule to allow external access on `port 443`. : <br> `$ sudo ufw allow https` or `$ sudo ufw allow 443`

## Allow All Incoming HTTP and HTTPS

-   `$ sudo ufw allow proto tcp from any to any port 80,443`
