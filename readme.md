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
