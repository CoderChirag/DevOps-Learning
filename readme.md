# Contents

---

- [Contents](#contents)
- [Bash Scripting](#bash-scripting)
  - [VM Setup](#vm-setup)
  - [First Script](#first-script)
  - [Variables](#variables)
  - [Command Line Arguments](#command-line-arguments)
    - [Some System Variables](#some-system-variables)
  - [Quotes](#quotes)
  - [Command Substitution](#command-substitution)
  - [Exporting Variables](#exporting-variables)
  - [User Input](#user-input)
  - [Decision Making](#decision-making)
    - [Basic If Statements](#basic-if-statements)
    - [Operators](#operators)
  - [Practical Example](#practical-example)
  - [Loops](#loops)
    - [For loop](#for-loop)
    - [While loop](#while-loop)
  - [Remote Command Execution](#remote-command-execution)
    - [Setting VMs](#setting-vms)

---

# Bash Scripting

## VM Setup

-   Go to the `bash_scripts` folder, copy the contents of `VagrantFile` and setup the VM by `vagrant up`.
-   ```
    $ vagrant ssh scriptbox
    $ sudo -i
    $ echo "scriptbox" > /etc/hostname
    $ hostname scriptbox
    $ logout
    ```

## First Script

-   ```
    $ vagrant ssh scriptbox
    $ sudo -i
    $ mkdir -p /opt/scripts
    $ cd /opt/scripts
    $ ls
    $ yum install vim -y
    ```
-   `$ vim firstscript.sh`

    ```
      #!/bin/bash

      ### This script prints system info ###

      echo "Welcome to bash script."
      echo

      # Checking system uptime
      echo "#############################################"
      echo "The uptime of the system is: "
      uptime
      echo

      # Memory Utilization
      echo "#############################################"
      echo "Memory Utilization"
      free -m
      echo

      # Disk Utilization
      echo "#############################################"
      echo "Disk Utilization"
      df -h
    ```

-   `$ chmod +x firstscript.sh`
-   `$ ./firstscript.sh`

    ```
    Welcome to bash script.

    #############################################
    The uptime of the system is:
    17:13:18 up  1:28,  1 user,  load average: 0.00, 0.01, 0.05

    #############################################
    Memory Utilization
                total        used        free      shared  buff/cache   available
    Mem:            990         111         449           6         429         733
    Swap:          1023           0        1023

    #############################################
    Disk Utilization
    Filesystem               Size  Used Avail Use% Mounted on
    devtmpfs                 485M     0  485M   0% /dev
    tmpfs                    496M     0  496M   0% /dev/shm
    tmpfs                    496M  6.7M  489M   2% /run
    tmpfs                    496M     0  496M   0% /sys/fs/cgroup
    /dev/mapper/centos-root   50G  1.7G   49G   4% /
    /dev/mapper/centos-home   28G   33M   28G   1% /home
    /dev/sda1               1014M  167M  848M  17% /boot
    vagrant                  466G  382G   85G  82% /vagrant
    tmpfs                    100M     0  100M   0% /run/user/1000
    ```

## Variables

-   To assign a value to a variable, `VariableName=Value`.
-   To read / access the value of a variable, `$VariableName`

## Command Line Arguments

-   We use `$n` to represent the `nth` argument.
-   For eg, `$1` will represent the `1st` argument, `$2` will represent the `2nd` and so on upto `$9`.
-   These are automatically set by the system when we run our script, so all we need to do is refer to them.
-   For example :

    ```
    copyscript.sh

    #!/bin/bash
    # A simple copy script

    cp $1 $2
    ```

### Some System Variables

|           |                                                               |
| --------- | ------------------------------------------------------------- |
| $0        | The name of the Bash script                                   |
| \$1 - $9  | The first 9 arguments to the Bash script                      |
| $#        | How many arguments were passed to the Bash script             |
| $@        | All the arguments supplied to the Bash script                 |
| $?        | The exit status of the most recently run process              |
| $$        | The process ID of the current process                         |
| $USER     | The username of the user running the script                   |
| $HOSTNAME | The hostname of the machine the script is running             |
| $SECONDS  | The number of seconds since the script was started            |
| $RANDOM   | Returns a different random number each time it is referred to |
| $LINENO   | Returns the current line number in the Bash script            |

## Quotes

-   Storing a single word in a variable works fine without quotes, but if we want to store a sentence or special characters like `$`, `%`, `@` atc, our normal assignment will not work.
-   **Single Quotes** every character as it is whereas **Double Quotes** tries to interprets the special characters, if they are not escaped.
-   Examples :

    ```
    $ NAME=CODER

    $ echo "Hello $NAME"        # By default interprets the special character
    Hello CODER

    $ echo "Hello \$NAME"       # Escaping the special character
    Hello $CODER

    $ echo 'HELLO $CODER'       # By default escapes the special character automatically
    Hello $CODER
    ```

## Command Substitution

-   Takes the output of the command and stores it in the variable.
-   Can be achieved by using _backticks_ (\`) or _dollar and parantheses_( `$()` ).
-   Examples :

    ```
    $ file=`ls`
    $ echo $file
    copyscript.sh dir1 dir2

    $ file=$(ls)
    $ echo $file
    copyscript.sh dir1 dir2
    ```

-   Printing the **free RAM** available :
    ```
    $ FREE_RAM=`free -m | grep MEM | awk '{print $4}'`
    $ echo "Free RAM available is $FREE_RAM mbs."
    Free RAM available is 450 mbs.
    ```

## Exporting Variables

-   Exporting the variables make them globally available for all the child processes. Eg, `export NAME=CODER`
-   For eg, by exporting a variable defined on the current shell, we can use it inside any script executed with the current shell, as executing a script would mean opening a new child shell using the current shell as parent.
    <br>

-   But the exported variables are still temporary, and are removed as soon as we logout from the shell
-   To make the variables **permanent** to the current user, we can export the variable in the `.bashrc` file of the current user. Eg, `$ echo 'export NAME=CODER' >> ~/.bashrc`
-   If you want to make the variable available globally for every user, export it inside `/etc/profile` file. Eg, `$ echo 'export NAME=CODER' >> /etc/profile`.

## User Input

-   Scripts can be made interactive just using the keyword `read`.
-   Example :

    ```
    #!/bin/bash

    echo "Enter your skills:"
    read SKILL      # Skills would be read from user input and stored in SKILL Variable

    echo "Your $SKILL skill is in high demand in the IT Industry."

    read -p 'Username: ' USR        # -p : prompt with some text ('Username: ' in this case)
    read -sp 'Password: ' pass      # -s : suppress the input so that it is not visible what user inputs

    echo

    echo "Login Successfull: Welcom USER $USR."
    ```

## Decision Making

### Basic If Statements

-   ```
      #syntax :
            if [ <some test> ]
            then
                <command>
            elif [ <some other test> ]
            then
                <command>
            else
                <command>
            fi

      # Example :
            #!/bin/bash

            read -p "Enter a number: " NUM
            echo

            if [ $NUM -gt 100 ]
            then
                echo "We have entered in IF block."
                sleep 3
                echo "Your number is greater than 100"
                echo
                date
            elif [ $NUM -eq 100 ]
            then
                echo "Number is equal to 100."
            else
                echo "You have entered number less than 100."
            fi

            echo "Script execution completed successfully."
    ```

### Operators

| Operator              | Decription                                                             |
| --------------------- | ---------------------------------------------------------------------- |
| ! EXPRESSION          | The EXPRESSION is false                                                |
| -n STRING             | The length of STRING is greater than 0                                 |
| -z STRING             | The length f STRING is zero (i.e, it is empty)                         |
| STRING1 = STRING2     | STRING1 is equal to STRING2                                            |
| STRING1 != STRING2    | STRING1 is not equal to STRING2                                        |
| INTEGER1 -eq INTEGER2 | INTEGER1 is numerically equal to INTEGER2                              |
| INTEGER1 -gt INTEGER2 | INTEGER1 is numerically greater than INTEGER2                          |
| INTEGER1 -lt INTEGER2 | INTEGER1 is numerically less than INTEGER2                             |
| -d FILE               | FILE exists and is a directory                                         |
| -e FILE               | FILE exists                                                            |
| -r FILE               | FILE exists and the read permission is granted                         |
| s FILE                | FILE exists and it's size is greater than zero (i.e, it is not empty). |
| -w FILE               | FILE exists and the write permission is granted.                       |
| -x FILE               | FILE exists and the execute permission is granted.                     |

**Points to Note :**

-   `=` is slightly different to `-eq`. [ 001 `=` 1 ] will return false as `=` doeas a **string comparison**, whereas `-eq` does a **numerical comparison**.
-   When we refer to `FILE` above, we are actually meaning a **path**.

## Practical Example

Writing a script to check if a process is running and if it is not running then start it.

```
# Writing a script to check if httpd process is running, af if not then start it.
$ vim /opt/scripts/11_monit.sh

    #!/bin/bash

    echo "#####################################################"
    date
    ls /var/run/httpd/httpd.pid &> /dev/null

    if [ $? -eq 0 ]
    then
        echo "Httpd process is running."
    else
        echo "Httpd process is NOT Running."
        echo "Starting the process"
        systemctl start httpd
        if [ $? -eq 0 ]
        then
            echo "Process started successfully."
        else
            echo "Process Starting Failed, contact the admin."
        fi
    fi
    echo "####################################################"
    echo
$ chmod +x /opt/scripts/11-monit.sh
# Scheduling the script to run automatically every minute, and log the output in a log file.
$ crontab -e        # It will open the vim editor

    # MM HH DOM mm DOW COMMAND       # Required Format
    # 30 20 * * 1-5 COMMAND     # Will run the command every monday to friday, every month at 8:30 p.m
    # Scheduling our script to run every minute everyday and logging the output in a file
    * * * * * /opt/scripts/11_monit.sh &>> /var/log/monit_httpd.log

crontab: installing new crontab
$ systemctl stop httpd

# Checking the log file after some minutes
$ cat /var/log/monit_httpd.log

    #####################################################
    Mon May 30 19:48:01 UTC 2022
    Httpd process is NOT Running.
    Starting the process
    Process started successfully.
    ####################################################

    #####################################################
    Mon May 30 19:49:01 UTC 2022
    Httpd process is running.
    ####################################################

    #####################################################
    Mon May 30 19:50:01 UTC 2022
    Httpd process is running.
    ####################################################

    #####################################################
    Mon May 30 19:51:01 UTC 2022
    Httpd process is running.
    ####################################################

    #####################################################
    Mon May 30 19:52:01 UTC 2022
    Httpd process is running.
    ####################################################
```

## Loops

### For loop

```
# Syntax:
    for var in <list>
    do
        <command>
    done

# Example 1:
    #!/bin/bash

    for VAR1 in java .net python php ruby
    do
        echo "Looping..."
        sleep 1
        echo "############################################"
        echo "Value of VAR1 is $VAR1"
        echo "############################################"
        date
        echo
    done

# Example 2:
    #!/bin/bash
    echo "Bash version ${BASH_VERSION}"
    for i in {0..10..2}
    do
        echo "Welcome $i times"
    done

# Example 3:
    #!/bin/bash
    for (( c=1; c<=5; c++ ))
    do
    echo "Welcome $c times"
    done
```

### While loop

```
# Syntax:
    while [ condition ]
    do
        <command>
    done

# Example:
    #!/bin/bash
    counter=0
    while [ $counter -lt 5 ]
    do
        echo "Looping..."
        echo "Value of counter is $counter."
        counter=$(($counter + 1))
    done
```

## Remote Command Execution

### Setting VMs

-   `$ vagrant up`
-   Setting up hostnames in all machines.

    ```
    $ vagrant ssh web01
    $ sudo -i
    $ echo "web01" > /etc/hostname
    $ hostname web01
    $ logout
    $ logout

    $ vagrant ssh web02
    $ sudo -i
    $ echo "web02" > /etc/hostname
    $ hostname web02
    $ logout
    $ logout

    $ vagrant ssh web03
    $ sudo -i
    $ echo "web03" > /etc/hostname
    $ hostname web03
    $ logout
    $ logout
    ```

-   Setting up a name to IP mapping in `scriptbox` machine
    ```
    $ vagrant ssh scriptbox
    $ sudo -i
    $ cat << EOF >> /etc/hosts
    > 192.168.10.13 web01
    > 192.168.10.14 web02
    > 192.168.10.15 web03
    > EOF
    $ ping web01
        PING web01 (192.168.10.13) 56(84) bytes of data.
        64 bytes from web01 (192.168.10.13): icmp_seq=1 ttl=64 time=0.424 ms
        64 bytes from web01 (192.168.10.13): icmp_seq=2 ttl=64 time=1.50 ms
        64 bytes from web01 (192.168.10.13): icmp_seq=3 ttl=64 time=0.717 ms
        64 bytes from web01 (192.168.10.13): icmp_seq=4 ttl=64 time=0.986 ms
        ^C
        --- web01 ping statistics ---
        4 packets transmitted, 4 received, 0% packet loss, time 3005ms
        rtt min/avg/max/mdev = 0.424/0.909/1.509/0.399 ms
    $ ping web02
        PING web02 (192.168.10.14) 56(84) bytes of data.
        64 bytes from web02 (192.168.10.14): icmp_seq=1 ttl=64 time=1.21 ms
        64 bytes from web02 (192.168.10.14): icmp_seq=2 ttl=64 time=1.00 ms
        64 bytes from web02 (192.168.10.14): icmp_seq=3 ttl=64 time=1.07 ms
        64 bytes from web02 (192.168.10.14): icmp_seq=4 ttl=64 time=1.15 ms
        ^C
        --- web02 ping statistics ---
        4 packets transmitted, 4 received, 0% packet loss, time 3005ms
        rtt min/avg/max/mdev = 1.000/1.112/1.219/0.081 ms
    $ ping web03
        PING web03 (192.168.10.15) 56(84) bytes of data.
        64 bytes from web03 (192.168.10.15): icmp_seq=1 ttl=64 time=1.44 ms
        64 bytes from web03 (192.168.10.15): icmp_seq=2 ttl=64 time=0.857 ms
        64 bytes from web03 (192.168.10.15): icmp_seq=3 ttl=64 time=0.885 ms
        64 bytes from web03 (192.168.10.15): icmp_seq=4 ttl=64 time=0.565 ms
        ^C
        --- web03 ping statistics ---
        4 packets transmitted, 4 received, 0% packet loss, time 3006ms
        rtt min/avg/max/mdev = 0.565/0.938/1.445/0.318 ms
    ```
-   Logging in remotely from `scriptbox` machine to other machines and creating a user named `devops` in all machines

    ```
    [root@localhost ~]# ssh vagrant@web01         # password for vagrant user is : vagrant
    [vagrant@web01 ~]$ sudo -i
    [root@web01 ~]# useradd devops
    [root@web01 ~]# passwd devops
    [root@web01 ~]# echo "devops    ALL=(ALL)    NOPASSWD: ALL" > /etc/sudoers.d/devops
    [root@web01 ~]# exit
    [vagrant@web01 ~]$ exit

    [root@localhost ~]# ssh vagrant@web02
    [vagrant@web02 ~]$ sudo -i
    [root@web02 ~]$ useradd devops
    [root@web02 ~]$ passwd devops
    [root@web02 ~]$ echo "devops    ALL=(ALL)    NOPASSWD: ALL" > /etc/sudoers.d/devops
    [root@web02 ~]$ exit
    [vagrant@web02 ~]$ exit
    [root@localhost ~]# logout

    $ vagrant ssh web03
    vagrant@web03:~$ sudo -i
    root@web03:~# sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
    root@web03:~# systemctl restart ssh
    root@web03:~# logout
    vagrant@web03:~# logout
    $ vagrant ssh scriptbox
    [vagrant@localhost ~]$ sudo -i
    [root@localhost ~]$ ssh vagrant@web03
    vagrant@web03:~$ sudo -i
    root@web03:~# adduser devops
    root@web03:~# echo "devops    ALL=(ALL:ALL)    NOPASSWD: ALL" > /etc/sudoers.d/devops
    root@web03:~# logout
    vagrant@web03:~$ logout
    ```

-   Configuring a **key based authentication** for ssh. Key based authentication is always considered more safer than the password based authentication.
    ```
    [root@scriptbox ~]# ssh-keygen
    [root@scriptbox ~]# ssh-copy-id devops@web01
    [root@scriptbox ~]# ssh-copy-id devops@web02
    [root@scriptbox ~]# ssh-copy-id devops@web03
    ```
