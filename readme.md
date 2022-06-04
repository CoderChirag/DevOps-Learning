# Contents

---

- [Contents](#contents)
- [Basics of Linux](#basics-of-linux)
  - [Introduction](#introduction)
    - [Open Source](#open-source)
    - [Linux History](#linux-history)
    - [Linux principles](#linux-principles)
    - [Why Linux?](#why-linux)
    - [Linux Architecture](#linux-architecture)
    - [Popular Linux distros](#popular-linux-distros)
    - [Some Important Directories](#some-important-directories)
  - [Basic File Directory and Related Commands](#basic-file-directory-and-related-commands)
  - [Date Command](#date-command)
    - [Syntax](#syntax)
    - [options with Examples :](#options-with-examples-)
    - [Format Specifiers](#format-specifiers)
  - [Matching File Names with Shell Expansions](#matching-file-names-with-shell-expansions)
    - [Command-line Expansions](#command-line-expansions)
      - [Pattern Matching](#pattern-matching)
      - [Tilde Expansion](#tilde-expansion)
      - [Brace Expansion](#brace-expansion)
      - [Variable Expansion](#variable-expansion)
      - [Command Substitution](#command-substitution)
  - [Getting Help in Linux](#getting-help-in-linux)
    - [Man Command](#man-command)
      - [Introduction](#introduction-1)
      - [Navigate and Search Man Pages](#navigate-and-search-man-pages)
      - [Reading Man pages](#reading-man-pages)
      - [Searching for man pages by keyword](#searching-for-man-pages-by-keyword)
  - [Vim Editor](#vim-editor)
    - [Command Mode](#command-mode)
  - [File Types](#file-types)
    - [Symbolic links](#symbolic-links)
      - [Soft link](#soft-link)
      - [Hard link](#hard-link)
      - [Creating symbolic links](#creating-symbolic-links)
  - [Filters](#filters)
    - [grep](#grep)
    - [less](#less)
    - [more](#more)
    - [head](#head)
    - [tail](#tail)
    - [cut](#cut)
    - [awk](#awk)
      - [Built-In Variables in Awk](#built-in-variables-in-awk)
    - [sed](#sed)
  - [Redirections](#redirections)
  - [Pipelines](#pipelines)
    - [tee](#tee)
  - [Users and Groups](#users-and-groups)
    - [Users](#users)
      - [Some Important Points](#some-important-points)
      - [Types of User](#types-of-user)
      - [`/etc/passwd` file](#etcpasswd-file)
      - [`/etc/group` file](#etcgroup-file)
    - [Groups](#groups)
      - [Basic commands](#basic-commands)
      - [Switching users](#switching-users)
  - [File Permissions](#file-permissions)
    - [Changing Permissions](#changing-permissions)
      - [Symbolic Method](#symbolic-method)
      - [Numeric Method](#numeric-method)
    - [Changing ownership](#changing-ownership)
    - [Special Permissions](#special-permissions)
      - [setuid permission](#setuid-permission)
      - [setgid permission](#setgid-permission)
      - [sticky permission](#sticky-permission)
    - [Default File Permissions - umask](#default-file-permissions---umask)
  - [Sudo](#sudo)
  - [Package Management](#package-management)
    - [Basic way of installing any package in CentOS](#basic-way-of-installing-any-package-in-centos)
    - [yum](#yum)
  - [Services](#services)
    - [Basic commands](#basic-commands-1)
  - [Processes](#processes)
    - [Killing Processes](#killing-processes)
  - [Archiving Data](#archiving-data)
  - [Ubuntu Commands](#ubuntu-commands)
  - [Basic Networking Commands](#basic-networking-commands)
    - [Protocols & Port Numbers](#protocols--port-numbers)
    - [ifconfig](#ifconfig)
    - [ip addr show](#ip-addr-show)
    - [ping](#ping)
    - [netstat](#netstat)
    - [ss](#ss)
    - [nmap](#nmap)
    - [dig](#dig)
    - [route](#route)
  - [Server Management in Linux](#server-management-in-linux)
    - [Setting up a website in CentOS7](#setting-up-a-website-in-centos7)
    - [Automating the Static Website Setup - Infrastucture as a Code (IAAC)](#automating-the-static-website-setup---infrastucture-as-a-code-iaac)
    - [Setting up a Wordpress Website using LAMP (Linux, Apache, MySQL, PHP) Stack](#setting-up-a-wordpress-website-using-lamp-linux-apache-mysql-php-stack)
      - [Configuring VM and Installing Dependencies](#configuring-vm-and-installing-dependencies)
      - [Installing WordPress](#installing-wordpress)
      - [Configuring Apache for WordPress](#configuring-apache-for-wordpress)
      - [Configuring database](#configuring-database)
      - [Configuring Wordpress to connect to the database](#configuring-wordpress-to-connect-to-the-database)
    - [Automating Wordpress Website Setup using IAAC](#automating-wordpress-website-setup-using-iaac)
    - [Setting up a Nodejs Application](#setting-up-a-nodejs-application)
      - [Using Apache2](#using-apache2)
        - [Configuring VM](#configuring-vm)
        - [Installing and Starting Apache server](#installing-and-starting-apache-server)
        - [Installing Nodejs](#installing-nodejs)
        - [Setting up MongoDb database](#setting-up-mongodb-database)
          - [Installing MongoDB](#installing-mongodb)
          - [Starting the MongoDB Service and Testing the Database](#starting-the-mongodb-service-and-testing-the-database)
        - [Setting up the Node.js application](#setting-up-the-nodejs-application)
          - [Installing PM2](#installing-pm2)
          - [Configuring Apche Server for Node.js](#configuring-apche-server-for-nodejs)

---

# Basics of Linux

## Introduction

### Open Source

Open source software is a software which have its entire source code open, and anybody can inspect, modify, and enhance the software.

<div align="center">

![open source](https://raw.githubusercontent.com/CoderChirag/DevOps-Learning/main/images/open_source.png)

</div>

### Linux History

-   **1984**: The GNU Project and the Free Software Foundation
    -   Creates open source version of UNIX utilities
    -   Creates the General Public License (GPL)
        -   Software license enforcing open source principles
-   **1991**: Linus Torvalds
    -   Creates open source, UNIX-like kernel, release under the GPL
    -   Ports some GNU utilities, solicits assistance online
-   **Today**:
    -   Linux kernel + GNU utilities = complete, open source, UNIX-like operating system
        -   Packaged for targeted audiences and distributions

### Linux principles

-   Everything is a file (including hardware)
-   Small Single purpose Programs
-   Ability to chain programs together for complex operations
-   Avoid Captive User Interfaces (GUI which waits for user interaction)
-   Configuration data stored in files

### Why Linux?

-   Opensource
-   Community Support
-   Support Wide Variety of hardware
-   Customization
-   Most Servers run on Linux
-   Automation
-   Security

### Linux Architecture

<div align="center">

![architecture](https://raw.githubusercontent.com/CoderChirag/DevOps-Learning/main/images/linux_architecture.jpg)

</div>

### Popular Linux distros

-   **Desktop based**
    -   Ubuntu Linux
    -   Linux Mint
    -   Arch Linux
    -   Fedora
    -   Debian
    -   OpenSuse
-   **Server based**
    -   Red Hat Enterprise Linux
    -   Ubuntu Server
    -   Centos
    -   SUSE Enterprise Linux

### Some Important Directories

-   **Home Directories**: `/root`, `/home/username`
-   **User Executables**: `/bin`, `/usr/bin`, `usr/local/bin`
-   **System Executables**: `/sbin`, `/usr/sbin`, `/usr/local/sbin`
-   **Other Mountpoints**: `/media`, `/mnt`
-   **Configuration**: `/etc`
-   **Temporary Files**: `/tmp`
-   **Kernels and Bootloader**: `/boot`
-   **Server Data**: `/var`, `/srv`
-   **System Information**: `/proc`, `/sys`
-   **Shared Libraries**: `/lib`, `/usr/lib`, `/usr/local/lib`

## Basic File Directory and Related Commands

-   `$ whoami` - shows the current user
-   `$ pwd` - present working directory
-   `$ ls <filename>` - list files from current directory
    -   `-l` : long listing => `<filetype><permissions>. <link_count> <user_owner> <group_owner> <block_size> <last_modification_date&time> <filename>`
    -   `-t` : sort based on last updated
    -   `-r` : reverse sort
    -   `-R` : recursive
    -   `-h` : human readable format for the block size
    -   `-a`: all
-   `$ du <filename>` - Shows the disk usage of files and directories on a system. By default the size is in Kb.
    -   `-a` : List all files and directories size
    -   `-h` : Human readable format
    -   `-c` : Display grand total in the output
    -   `-s` : Display only total
    -   `-0/td>`: End output with null byte
    -   `--block-size=<size>` : Specify block size
    -   `--time` : display time modification
-   `file <filename>` : scans the beginning of a file's contents and displays what type it is.
-   `$ cat <filename>` - view the contents of a file
-   `$ sudo -i` - switch to root user
-   `$ cd <path>` - change directory
-   `$ mkdir [-p|--parents: no error if existing] <dirname>` - makes new directory
-   `$ touch <filename>` - makes new file
-   `$ touch file1{1..10}.txt` - makes 10 new files
-   `$ cp [-r|-R|--recursive: copy recursively] <src> <dest>` - copying files
-   `mv <src> <dest>` - move or rename files
-   `rm [-r|-R|--recursive: recursive] [-f|--force: force] <filename>` - remove file
-   `wc [-c: byte counts] [-m: char counts] [-l: lines count] [-L: max line length] [-w: words counts]` - print newline, word, and bytte counts for each file

## Date Command

-   `date` command is used to display the system date and time.
-   `date` command is also used to set date and time of the system.
-   By default the date command displays the date in the time zone on which unix/linux operating system is configured.
-   You must be the super-user (root) to change the date and time.

### Syntax

-   `$ date [OPTION]... [+++FORMAT]`

### options with Examples :

1. `date (no option)` : With no options, the date command displays the current date and time, including the abbreviated day name, abbreviated month name, day of the month, the time separated by colons, the time zone name, and the year.
    ```
    $ date
    Wed Jun  1 13:41:00 UTC 2022
    ```
2. `-u|--utc|--universal` : Displays the time in GMT(Greenwich Mean Time)/UTC(Coordinated Universal Time )time zone.
    ```
    $ date -u
    Wed Jun  1 13:42:53 UTC 2022
    ```
3. `-d|--date` : Displays the given date string in the format of date. But this will not affect the system’s actual date and time value.Rather it uses the date and time given in the form of string.

    ```
    $ date --date="2/02/2010"
    Tue Feb  2 00:00:00 UTC 2010

    $ date --date="Feb 2 2010"
    Tue Feb  2 00:00:00 UTC 2010

    # Past Dates
    $ date --date="2 year ago"
    Mon Jun  1 13:48:30 UTC 2020

    $ date --date="yesterday"
    Tue May 31 13:48:35 UTC 2022

    $ date --date="10 sec ago"
    Wed Jun  1 13:50:32 UTC 2022

    # Future Dates
    $ date --date="tomorrow"
    Thu Jun  2 13:48:46 UTC 2022

    $ date --date="next week"
    Wed Jun  8 13:51:15 UTC 2022

    $ date --date="2 day"
    Fri Jun  3 13:51:38 UTC 2022
    ```

4. `-s|--set` : To set the system date and time.
    ```
    $ date --set="Tue Nov 13 15:23:34 PDT 2018"
    Tue Nov 13 15:23:34 PDT 2018
    ```

### Format Specifiers

```
%D: Display date as mm/dd/yy.
%r: Displat time in 12-Hour Format
%d: Display the day of the month (01 to 31).
%a: Displays the abbreviated name for weekdays (Sun to Sat).
%A: Displays full weekdays (Sunday to Saturday).
%h: Displays abbreviated month name (Jan to Dec).
%b: Displays abbreviated month name (Jan to Dec).
%B: Displays full month name(January to December).
%m: Displays the month of year (01 to 12).
%y: Displays last two digits of the year(00 to 99).
%Y: Display four-digit year.
%T: Display the time in 24 hour format as HH:MM:SS.
%H: Display the hour.
%M: Display the minute.
%S: Display the seconds.
```

Examples :

```
$ date "+%D"
10/11/17

$ date "+%D %T"
10/11/17 16:13:27

$ date "+%Y-%m-%d"
2017-10-11

$ date "+%Y/%m/%d"
2017/10/11

$ date "+%A %B %d %T %y"
Thursday October 07:54:29 12 17
```

## Matching File Names with Shell Expansions

### Command-line Expansions

The Bash shell has multiple ways of expanding a command line including **pattern matching**, **home directory expansion**, **string expansion**, and **variable substitution**. Perhaps the most powerful of these is the **path name-matching capability**, historically called **globbing**.
The **Bash globbing** feature, sometimes called **“wildcards”**, makes managing large numbers of files easier. Using metacharacters that “expand” to match file and path names being sought, commands perform on a focused set of files at once.

#### Pattern Matching

**Globbing** is a shell command-parsing operation that expands a wildcard pattern into a list of matching path names. Command-line metacharacters are replaced by the match list prior to command execution. Patterns that do not return matches display the original pattern request as literal text. The following are common metacharacters and pattern classes.

| Pattern      | Matches                                                                                                    |
| ------------ | ---------------------------------------------------------------------------------------------------------- |
| \*           | Any string of zero or more characters                                                                      |
| ?            | Any single character                                                                                       |
| [abc...]     | Any one character in the enclosed class (between the square brackets)                                      |
| [!abc...]    | Any one character not in the enclosed class                                                                |
| [^abc...]    | Begins with any one character in the enclosed class                                                        |
| \[[:alpha:]] | Any alphabetic character                                                                                   |
| \[[:lower:]] | Any lowercase character                                                                                    |
| \[[:upper]]  | Any uppercase letter                                                                                       |
| \[[:alnum:]] | Any alphabetic character or digit                                                                          |
| \[[:punct:]] | Any printable character not a space or alphanumeric                                                        |
| \[[:digit:]] | Any single digit from 0 to 9                                                                               |
| \[[:space:]] | Any single white space character. This may include tabs, newlines, carriage returns, form feeds, or spaces |

For the next few examples, pretend that we have run the following commands to create some sample files.

```
$ mkdir glob; cd glob
$ touch alfa bravo charlie delta echo able baker cast dog easy
```

The first example will use simple pattern matches with the asterisk (\*) and question mark (?) characters, and a class of characters, to match some of those file names.

```
$ ls a*
able alfa
$ ls *a*
able alfa bravo cast charlie delta easy
$ ls [ac]*
able alfa cast charlie
$ ls ????
able alfa cast easy echo
$ ls ?????
baker bravo delta
```

#### Tilde Expansion

The tilde character (~), matches the current user's home directory. If it starts a string of characters other than a slash (/), the shell will interpret the string up to that slash as a user name, if one matches, and replace the string with the absolute path to that user's home directory. If no user name matches, then an actual tilde followed by the string of characters will be used instead.

```
$ echo ~root
/root
$ echo ~user
/home/user
$ echo ~/glob
/home/user/glob
```

#### Brace Expansion

-   Brace expansion is used to generate discretionary strings of characters.
-   Braces contain a comma-separated list of strings, or a sequence expression.
-   The result includes the text preceding or following the brace definition.
-   Brace expansions may be nested, one inside another.
-   Also double-dot syntax (`..`) expands to a sequence such that `{m..p}` will expand to `m n o p`.

```
$ echo {Sunday,Monday,Tuesday,Wednesday}.log
Sunday.log Monday.log Tuesday.log Wednesday.log
$ echo file{1..3}.txt
file1.txt file2.txt file3.txt
$ echo file{a..c}.txt
filea.txt fileb.txt filec.txt
$ echo file{a,b}{1,2}.txt
filea1.txt filea2.txt fileb1.txt fileb2.txt
$ echo file {a{1,2},b,c}.txt
filea1.txt filea2.txt fileb.txt filec.txt
```

#### Variable Expansion

-   A variable acts like a named container that can store a value in memory. Variables make it easy to access and modify the stored data either from the command line or within a shell script.
-   We can assign data as a value to a variable using the following syntax:
-   `$ VARIABLENAME=value`
-   We can use variable expansion to convert the variable name to its value on the command line. If a string starts with a dollar sign (`$`), then the shell will try to use the rest of that string as a variable name and replace it with whatever value the variable has.
    ```
    $ USERNAME=operator
    $ echo $USERNAME
    operator
    ```
-   To help avoid mistakes due to other shell expansions, you can put the name of the variable in curly braces, for example `${VARIABLENAME}`.
    ```
    $ USERNAME=operator
    $ echo ${USERNAME}
    operator
    ```

#### Command Substitution

-   Command substitution allows the output of a command to replace the command itself on the command line.
-   Command substitution occurs when a command is enclosed in parentheses, and preceded by a dollar sign (`$`).
-   The `$(command)` form can nest multiple command expansions inside each other.
    ```
    $ echo Today is $(date +%A).
    Today is Saturday.
    $ echo The time is $(date +%M) minutes past $(date +%l%p).
    The time is 50 minutes past 1PM.
    ```
    **NOTE**
-   An older form of command substitution uses backticks: \`command\`.
-   Disadvantages to the backticks form include:
    -   it can be easy to visually confuse backticks with single quote marks
    -   backticks cannot be nested.

## Getting Help in Linux

### Man Command

#### Introduction

-   One source of documentation that is generally available on the local system are system _manual pages_ or _man pages_.
-   These pages are shipped as part of the software packages for which they provide documentation, and can be accessed from the command line by using the `man` command.
-   Common Sections of the Linux Manual

| Section | Content Type                                                        |
| ------- | ------------------------------------------------------------------- |
| 1       | User Commands (both executable and shell programs)                  |
| 2       | System Calls (kernel routines invoked from user space)              |
| 3       | Library functions (provided by program libraries)                   |
| 4       | Special Files (such as device files)                                |
| 5       | File formats (for many configuration files and structures)          |
| 6       | Games (historical section for amusing programs)                     |
| 7       | Conventions, standards, and miscellaneous (protocols, file systems) |
| 8       | System administration and priviliged commands (maintenance tasks)   |
| 9       | Linux kernel API (internal kernel calls)                            |

-   To distinguish identical topic names in different sections, man page references include the section number in parentheses after the topic.
-   For example, **passwd(1)** describes the command to change passwords, while **passwd(5)** explains the `/etc/passwd` file format for storing local user accounts.
-   To read specific man pages, use `$ man topic`.
-   To display the man page topic from a specific section, include the section number argument: `$ man 5 passwd` displays **passwd(5)**.

#### Navigate and Search Man Pages

| Command   | Result                                                |
| --------- | ----------------------------------------------------- |
| Spacebar  | Scroll forward (down) one screen                      |
| PageDown  | Scroll forward (down) one sceeen                      |
| PageUp    | Scroll backward (up) one screen                       |
| DownArrow | Scroll forward (down) one line                        |
| UpArrow   | Scroll backward (up) one line                         |
| D         | Scroll forward (down) one half screen                 |
| U         | Scroll backward (up) one half-screen                  |
| /string   | Search forward (down) for `string` in the man page    |
| N         | Repeat previous search forward (down) in the man page |
| Shift + N | Repeat previous search backward (up) in the man page  |
| G         | Go to the start of the man page                       |
| Shift + G | Go to the end of the man page                         |
| Q         | Exit man and return to the command shell prompt       |

#### Reading Man pages

-   Each topic is separated into several parts. Most topics share the same headings and are presented in the same order. Typically a topic does not feature all headings, because not all headings apply for all topics.

| Headings    | Description                                                           |
| ----------- | --------------------------------------------------------------------- |
| NAME        | Subject name. Usually a command or file name. Very brief description  |
| SYNOPSIS    | Summary of the command syntax                                         |
| DESCRIPTION | In-depth description to provide a basic understanding of the topic    |
| OPTIONS     | Explanation of the command execution options                          |
| EXAMPLES    | Examples of how to use the command, function, or file                 |
| FILES       | A list of files and directories related to the man page               |
| SEE ALSO    | Related information, normally other man page topics                   |
| BUGS        | Known bugs in the software                                            |
| Author      | Information about who has contributed to the development of the topic |

#### Searching for man pages by keyword

-   A keyword search of man pages is performed with `$ man -k keyword`, which displays a list of keyword-matching man page topics with section numbers.

    ```
        $ man -k passwd
        checkPasswdAccess (3) - query the SELinux policy database in the kernel.
        chpasswd (8) - update passwords in batch mode ckpasswd (8) - nnrpd password authenticator
        fgetpwent_r (3) - get passwd file entry reentrantly getpwent_r (3) - get passwd file entry reentrantly
        ...
        passwd (1) - update user's authentication tokens sslpasswd (1ssl) - compute password hashes
        passwd (5) - password file
        passwd.nntp (5) - Passwords for connecting to remote NNTP servers
        passwd2des (3) - RFS password encryption
        ...
    ```

## Vim Editor

-   To install in centos: `sudo yum install vim -y`
-   To start the editor: `vim <filename>`
-   Thera are 3 modes in vim editor
    1. Command Mode
    2. Insert Mode (edit mode)
    3. Extended command mode (using `:`)
-   To set line numbers in vim: `:se nu #set numbers`

### Command Mode

|          |                                                      |
| -------- | ---------------------------------------------------- |
| gg       | To go to the beginning of the page                   |
| G        | To go to the end of the page                         |
| w        | To move the cursor forward, word by word             |
| b        | to move the cusor backward, word by word             |
| nw       | to move the cursor forward to n words (5w)           |
| nb       | To move the cursor backward to n words (5b)          |
| u        | To undo last change (word)                           |
| U        | To undo the previous changes (entire line)           |
| Ctrl + R | To redo the changes                                  |
| yy       | To copy a line                                       |
| nyy      | To copy n lines (5yy or 4yy)                         |
| p        | To paste line below the cursor position              |
| P        | To paste line above the cursor position              |
| dw       | To delete the word letter by letter (like Backspace) |
| x        | To delete the word letter by letter (like DEL key)   |
| dd       | To delete entire line                                |
| ndd      | To delete n no. of lines from cursor pointer (5dd)   |
| /        | To search a word in the file                         |

-   We can directly search in vim editor by writing forward slash(/) in **command mode**, eg: `/abcd`.
-   We can directly search and replace in vim editor in command mode by writing like this, `%s/coronavirus/covid19` (all coronavirus would be replaced by covid19).
-   It will be replaced once in every line, if you want to replace globally: `%s/coronavirus/covid19/g`

## File Types

-   `file <filename>`: command for checking the **file type**.

|              |     |                                                                                                            |
| ------------ | --- | ---------------------------------------------------------------------------------------------------------- |
| Regular File | -   | Normal files such as text, data, or executable files                                                       |
| Directory    | d   | Files that are lists of other files                                                                        |
| Link         | l   | A shortcut that points to the location of the actual file                                                  |
| Special File | c   | Mechanism used for input and output, such as files in /dev                                                 |
| Socket       | s   | A special file that provides inter-process networking protected by the file system's access control        |
| Pipe         | p   | A special file that allows processes to communicate with each other without using network socket semantics |

### Symbolic links

-   With symbolic links an administrator can assign a file or directory multiple identities.
-   Can be thought of as a pointer to original file. There are 2 types of symbolic links:
    1. hard links
    2. soft links

#### Soft link

-   Can cross the file system
-   Allows to link b/w directories
-   Has different inodes number and file permissions than original file
-   permissions will not be updated
-   has only the path of the original file, not the contents

#### Hard link

-   can't cross the filesystem boundaries
-   can't link directories
-   has the same inodes number and permissions of original file
-   permissions will be updated if we change the permissions of source file
-   has actual contents of original file, so that you can still view the contents, even if the original file moved or removed

#### Creating symbolic links

-   `ln -s <sourcefile> <soflink file>`: creates a **softlink**
-   `ln <sourcefile> <hardlink-file>`: creates a **hardlink**

## Filters

### grep

-   Used for searching the keywords of regexp in a file
-   > grep [-i : case insensitive] [-r|-R : recursive] [-v: don't show the given keyword] \<keyword | regexp> \<input>
-   Example
    -   Searching for a config in all files in afolder and its subfolders
    -   > grep -R SELINUX /etc/\*

### less

-   A reader to read the contents of 1 or more file and looks just like vim.
-   You can use up/down arrow to move, forward slash(/) to search, and q to quit.
-   > less \<filename>

### more

-   Just like less but have to press enter to move down.
-   Difference is that `more` allows us to view one or more files as a single file spearated by lines, whereas `less` allows us to switch between them.
-   > more \<filename>

### head

-   Shows the specified no. of lines from the beginning of a file
-   > head [-\<n>: no of lines to show, by default 10] \<filename>
-   For eg `$ head -20 anaconda-ks.cfg`

### tail

-   Shows the specified no. of lines from the bottom of a file
-   have very useful functionality of displaying the content in raltime by using `-f` option.
-   > tail [-\<n>: no o lines to show, by default 10] [-f : realtime changes to file] \<filename>

### cut

-   Used to separate the required content from a file in which content is separated by a fixed delimiter
-   > cut [-d\<d>: delimiter `d`] [-f\<n>: column `n`] \<file>
-   Example
    -   Getting all the usernames from /etc/passwd file
    -   `$ cut -d: f1 /etc/passwd`

### awk

-   Advanced pattern scanning and processing language
-   Operations:
    1. Scans a file line by line
    2. Splits each input line into fields
    3. Compares input line/fields to pattern
    4. Performs action(s) on matched lines
-   Useful for:
    1. Transform data files
    2. Produce formatted reports
-   Programming Constructs:
    1. Formal output lines
    2. Arithmetic and string operations
    3. Conditionals and loops
-   Syntax: `awk options 'selection_criteria {action}' input-file > output-file`
-   Options:
    ```
    -f program-file : Reads the AWK program source from the file program-file, instead of from the first command line argument.
    -F fs           : Use fs for the input field separator
    ```
-   Sample Commands
    -   **Example:** Consider the following text file as the input file for all cases below:
        `$ cat > employee.txt`
        <br>
        ```
        ajay manager account 45000
        sunil clerk account 25000
        varun manager sales 50000
        amit manager account 47000
        tarun peon sales 15000
        deepak clerk sales 23000
        sunil peon sales 13000
        satvik director purchase 80000
        ```
    1.  **Default behaviour of Awk:** By default Awk prints every line of data from the specified file.<br>`$ awk {print} employee.txt`<br>**Output:**
        ```
        ajay manager account 45000
        sunil clerk account 25000
        varun manager sales 50000
        amit manager account 47000
        tarun peon sales 15000
        deepak clerk sales 23000
        sunil peon sales 13000
        satvik director purchase 80000
        ```
    2.  Print the lines which match the given pattern<br>`$ awk '/manager/ {print}' employee.txt`<br>**Output:**
        ```
        ajay manager account 45000
        varun manager sales 50000
        amit manager account 47000
        ```
    3.  **Splitting a line into fields:** For each record, i.e, line, the awk command splits the record delimited by whitespace character by default and stores it in the `$n` variables.<br>Also, `$0` represents the whole line.<br>`$ awk '{print $1,$4}' employee.txt`<br>**Output:**
        ```
        ajay 45000
        sunil 25000
        varun 50000
        amit 47000
        tarun 15000
        deepak 23000
        sunil 13000
        satvik 80000
        ```

#### Built-In Variables in Awk

Awk's built-in variables include the field variables - `$1`, `$2`, `$3`, and so on (`$0` is the entire line) - that break a line of text into individual words or pieces called **fieds**.

-   **NR:** NR command keeps a current count of the number of input records (lines).
-   **NF:** NF command keeps a count of the number of fields within the current input record.
-   **FS:** FS command contains the field separator character which is used to divide fields on the input line. The default is “white space”, meaning space and tab characters. FS can be reassigned to another character (typically in BEGIN) to change the field separator.
-   **RS:** RS command stores the current record separator character. Since, by default, an input line is the input record, the default record separator character is a newline.
-   **OFS:** OFS command stores the output field separator, which separates the fields when Awk prints them. The default is a blank space. Whenever print has several parameters separated with commas, it will print the value of OFS in between each parameter.
-   **ORS:** ORS command stores the output record separator, which separates the output lines when Awk prints them. The default is a newline character. print automatically outputs the contents of ORS at the end of whatever it is given to print.
-   **Examples:**

    -   **Use of NR built-in variables (Display Line Number)**<br> `$ awk '{print NR,$0}' employee.txt`<br>**Output:**

        ```
            1 ajay manager account 45000
            2 sunil clerk account 25000
            3 varun manager sales 50000
            4 amit manager account 47000
            5 tarun peon sales 15000
            6 deepak clerk sales 23000
            7 sunil peon sales 13000
            8 satvik director purchase 80000
        ```

    -   **Use of NF built-in variables (Display Last Field)**<br>`$ awk '{print $1,$NF}' employee.txt`<br>**Output:**
        ```
        ajay 45000
        sunil 25000
        varun 50000
        amit 47000
        tarun 15000
        deepak 23000
        sunil 13000
        satvik 80000
        ```
    -   **Another use of NR built-in variables (Display Line From 3 to 6)**<br>`$ awk 'NR==3, NR==6 {print NR,$0}' employee.txt`<br>**Output:**
        ```
        3 varun manager sales 50000
        4 amit manager account 47000
        5 tarun peon sales 15000
        6 deepak clerk sales 23000
        ```
    -   **Practical Example:** Finding all the usernames from `/etc/passwd` file<br> `$ awk -F':' '{print $1}' /etc/passwd`

### sed

-   SED command in UNIX stands for stream editor and it can perform lots of functions on file like searching, find and replace, insertion or deletion.
-   Most common use of SED command is for find and replace.
-   By using SED we can edit files even without opening them.
-   **Syntax:**
    `$ sed OPTIONS... [SCRIPT] [INPUTFILE...]`
-   **Example:**<br>Consider the below text file as an input.<br>`$ cat > inputfile.txt`<br><br>

    ```
    unix is great os. unix is opensource. unix is free os.
    learn operating system.
    unix linux which one you choose.
    unix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
    ```

    1.  **Replacing or substituting string :** Sed command is mostly used to replace the text in a file. The below simple sed command replaces the word “unix” with “linux” in the file.<br>`$ sed 's/unix/linux' inputfile.txt #Changes the 1st occurence of 'unix' in every line `<br>**Output:**
        ```
        linux is great os. unix is opensource. unix is free os.
        learn operating system.
        linux linux which one you choose.
        linux is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful
        ```
        Here the`s` specifies the substitution operation. The `/` are delimiters. The `unix` is the search pattern and the `linux` is the replacement string.
    2.  **Replacing the nth occurrence of a pattern in a line :** Use the /1, /2 etc flags to replace the first, second occurrence of a pattern in a line.<br>`$ sed 's/unix/linux/2' inputfile.txt`<br>**Output:**
        ```
        unix is great os. linux is opensource. unix is free os.
        learn operating system.
        unix linux which one you choose.
        unix is easy to learn.linux is a multiuser os.Learn unix .unix is a powerful.
        ```
    3.  **Replacing all the occurrence of the pattern in a line :** The substitute flag /g (global replacement) specifies the sed command to replace all the occurrences of the string in the line.<br>`$ sed 's/unix/linux/g' inputfile.txt`<br>**Output:**
        ```
        linux is great os. linux is opensource. linux is free os.
        learn operating system.
        linux linux which one you choose.
        linux is easy to learn.linux is a multiuser os.Learn linux .linux is a powerful.
        ```
    4.  **Replacing from nth occurrence to all occurrences in a line :** Use the combination of /1, /2 etc and /g to replace all the patterns from the nth occurrence of a pattern in a line. The following sed command replaces the third, fourth, fifth… “unix” word with “linux” word in a line.<br>`$ sed 's/unix/linux/3g' inputfile.txt`<br>**Output:**
        ```
        unix is great os. unix is opensource. linux is free os.
        learn operating system.
        unix linux which one you choose.
        unix is easy to learn.unix is a multiuser os.Learn linux .linux is a powerful.
        ```
    5.  **Parenthesize first character of each word**<br>`$ echo "Welocme To The Family" | sed 's/\(\b[A-Z]\)/\(\1\)/g'`<br>**Explaination:**

        ```
        sed 's/\b(pattern1)/pattern2/g' --- Does " A word by word search"

            sed 's/\(\b[A-Z]\)/pattern2/g'  --- Does " Matches a single uppercase letter"
            sed 's/\(\b[A-Z]\)/\(\1\)/g'     --- Does " sed 's/\(\b[A-Z]\)/\([A-Z]\)/g' "
                                            \1 is a back reference.
        ```

        **Output:**<br>`(W)elcome (T)o (T)he (G)eek (S)tuff`

    6.  **Replacing string on a specific line number :**<br>`$ sed '3 s/unix/linux' inputfile.txt`<br>**Output:**

        ```
        unix is great os. unix is opensource. unix is free os.
        learn operating system.
        linux linux which one you choose.
        unix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
        ```

    7.  **Duplicating the replaced line with /p flag :** The /p print flag prints the replaced line twice on the terminal. If a line does not have the search pattern and is not replaced, then the /p prints that line only once.<br>`$ sed 's/unix/linux/p' inputfile.txt`<br>**Output:**
        ```
        linux is great os. unix is opensource. unix is free os.
        linux is great os. unix is opensource. unix is free os.
        learn operating system.
        linux linux which one you choose.
        linux linux which one you choose.
        linux is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
        linux is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
        ```
    8.  **Printing only the replaced lines :** Use the -n option along with the /p print flag to display only the replaced lines. Here the -n option suppresses the duplicate rows generated by the /p flag and prints the replaced lines only one time.<br>`$ sed -n 's/unix/linux/p' inputfile.txt`<br>**Output:**
        ```
        linux is great os. unix is opensource. unix is free os.
        linux linux which one you choose.
        linux is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
        ```
    9.  **Replacing string on a range of lines :**<br>`$ sed '1,3 s/unix/linux' inputfile.txt`<br>**Output:**
        ```
        linux is great os. unix is opensource. unix is free os.
        learn operating system.
        linux linux which one you choose.
        unix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
        ```
    10. **Deleting lines from a particular file :** SED command can also be used for deleting lines from a particular file. SED command is used for performing deletion operation without even opening the file<br>**Examples:**<br>1. **To Delete a particular line say n in this example**<br>&nbsp;&nbsp;&nbsp;&nbsp;**Syntax:** `$ sed 'nd' filename.txt`<br>&nbsp;&nbsp;&nbsp;&nbsp;**Example:** `$ sed '5d' filename.txt`<br>2. **To Delete a last line**<br>&nbsp;&nbsp;&nbsp;&nbsp;**Syntax:** `$ sed '$d' filename.txt`<br>3. **To Delete line from range x to y**<br>&nbsp;&nbsp;&nbsp;&nbsp;**Syntax:** `$ sed 'x,yd' filename.txt`<br>&nbsp;&nbsp;&nbsp;&nbsp;**Example:** `$ sed '3,6d' filename.txt`<br>4. **To Delete from nth to last line**<br>&nbsp;&nbsp;&nbsp;&nbsp;**Syntax:** `$ sed 'nth,$d' filename.txt`<br>&nbsp;&nbsp;&nbsp;&nbsp;**Example:** `$ sed '12,$d' filename.txt`<br>5. **To Delete pattern matching line**<br>&nbsp;&nbsp;&nbsp;&nbsp;**Syntax:** `$ sed '/pattern/d' filename.txt`<br>&nbsp;&nbsp;&nbsp;&nbsp;**Example:** `$ sed '/abc/d' filename.txt`

## Redirections

|                                                |                                                                 |
| ---------------------------------------------- | --------------------------------------------------------------- |
| `$ command > file`                             | redirect **stdout** to **overwrite** a file                     |
| `$ command >> file`                            | redirect **stdout** to **append** a file                        |
| `$ command 2> file`                            | redirect **stderr** to **overwrite** a file                     |
| `$ command 2>> file`                           | redirect **stderr** to **append** a file                        |
| `$ command > /dev/null`                        | discard / dump **stdout** messages                              |
| `$ command 2> /dev/null`                       | discard / dump **stderr** messages                              |
| `$ command &> file` or `command > file 2>&1`   | redirect both **stdout** and **stderr** to **overwrite** a file |
| `$ command &>> file` or `command >> file 2>&1` | redirect both **stdout** and **stderr** to **append** a file    |
| `$ command &>> /dev/null`                      | dump / discard all the output                                   |

-   `/dev/null` is a dump file. We can dump any content in it and it would always be empty.
-   `>file` is equivalent to `1>file` and `>>file` is equivalent to `1>>file`

## Pipelines

-   A **pipeline** is a sequence of one or more commands seperated by the pipe character `|`.
-   A pipe connects the standard output of the 1st command to the standard input of the next command.

<div align='center'>

![pipelines](./images/pipe-redirection-1.png)

</div>

-   **Examples:**
    -   `$ ls -l /usr/bin | less`
    -   `$ ls | wc -l`

### tee

-   **Limitation of pipelining:** When redirection is combined with a pipeline, the shell sets up the entire pipeline first, then it redirects input / output.
    If output redirection is used in the middle of a pipeline, the output will go to the file and **not** to the next command in the pipeline.

-   **Example:** `$ ls > /tmp/saved-output | less #output goes to the file and less displays nothing on the screen`
-   The **_tee_** command overcomes this limitation.
<div align='center'>

![tee](./images/tee.png)

</div>

-   **Example:** `$ ls -l | tee /tmp/seved-output | less`

## Users and Groups

### Users

A **_user_** account is used to provide security boundaries between different people and programs that can run commands.

There are three main types of user account: the superuser, system users, and regular users.

The superuser account is for administration of the system. The name of the superuser is root and the account has UID 0. The superuser has full access to the system.

The system has system user accounts which are used by processes that provide supporting services. These processes, or daemons, usually do not need to run as the superuser. They are assiged non-privileged accounts that allow them to secure their files and other resources from each other and from regular users on the system. Users do not interactively log in using a system user account.

Most users have regular user accounts which they use for their day-to-day work. Like system users, regular users have limited access to the system.

#### Some Important Points

-   Users and groups are used to control access to files and resources
-   Users login to the system by supplying their username and password
-   Every file on the system is owned by a user and associated with a group
-   Every process has an owner and group affiliation, and can only access the resources its owner or group can access
    <br>

-   Every user of the system is assigned a **unique user ID number (the UID)**
-   Users name and UID are stored in `/etc/passwd`
-   User's password is stored in `/etc/shadow` in encrypted form
-   Users are assigned a **home directory** and a program that is run when they login (Usually a shell)
-   Users cannot read, write or execute each other's files without permission

#### Types of User

| Type        | Example              | User ID (ID)  | Group ID (GID) | Home Dir         | Shell           |
| ----------- | -------------------- | ------------- | -------------- | ---------------- | --------------- |
| **Root**    | root                 | 0             | 0              | `/root`          | `/bin/bash`     |
| **Regular** | coderchirag, vagrant | 1000 to 60000 | 1000 to 60000  | `/home/username` | `/bin/bash`     |
| **Service** | ftp, ssh, apache     | 1 to 999      | 1 to 999       | `/var/ftp etc`   | `/sbin/nologin` |

#### `/etc/passwd` file

-   Total 7 columns seperated by colon (`:`)
-   user_name:pwd(link to shadow file):uid:gid:comment:home_dir:login_shell

#### `/etc/group` file

-   Total 4 columns seperated by colon (`:`)
-   group_name:pwd(link to shadow file):gid:users_list(separated by commas (`,`)

### Groups

-   A **group** is a collection of users that need to share acess to fiiles and other system resources.
-   Every user has exactly one **primary group** and zero or more **supplementary groups**.

#### Basic commands

-   `$ id <username>` - outputs uid, gid, and groups
-   `$ useradd <username>` - creates new user
-   `$ groupadd <groupname>` - create new group
-   `$ usermod [-g group: primary group] [-G groups: supplementary groups] [-c comment] <username>` - modify the user config
-   `$ userdel [-r: delete the home directory also] <username>` - delete a user
-   `passwd <username>` - set the password of user
-   `$ groupmod [-n name: new name] [-g gid: new gid] <groupname>` - modify the group
-   `$ groupdel <groupname>` - delete the group (cannot delete a group if it is a primary group for any existing user).
-   `$ last` - give the details of which user logged in at what time
-   `$ who` - which user is currently logged in
-   `$ lsof -u <user>` - list files opened by the user (to install `$ yum install lsof -y`).

#### Switching users

-   Firstly create a password for **root** user - `$ sudo passwd`.
-   The `su` command allows you to switch users
    -   switching as root user - `$ sudo su -` or `$ sudo -i #Switch to root acount properly` or `$ sudo -s #Just run the root shell only`.
    -   switching to other users - `$ sudo su - user01`.

## File Permissions

-   Four symbols are used when displaying permissions:
    -   **r :** permission to read a file or list a directory's contents
    -   **w :** permission to write to a file or create and remove files from a directory
    -   **x :** permission to execute a program or change into a directory and do a long listing of the directory
    -   **- :** no permission (in place of the r, w or x)
-   File permissions may be viewed using `ls -l`<br>`$ ls -l /bin/login`

    ```
    -rwxr-xr-x. 1 root root 19080 Apr 1 18:26 /bin/login

    - => filetype
    rw- => user
    r-x => group
    r-x => others
    1 => link count
    root => owner user
    root => owner group
    19080 => filesize
    Apr 1 18:26 => Last modification date & time
    /bin/login => filename
    ```

### Changing Permissions

#### Symbolic Method

-   To change access modes :
    -   `$ chmod [-option]... mode[,mode] file|directory...`
-   **mode** includes :
    -   **u**, **g** or **o** for user, group and other
    -   **+**, **-** or **=** for grant, deny or set
    -   **r**, **w**, or **x** for read, write and execute
-   Options include :
    -   **-R** Recursive
    -   **-v** Verbose
    -   **--reference** Reference another file for its mode
-   Examples :
    -   `$ chmod ugo+r file` - Grant read access to all for file
    -   `$ chmod o-wx dir` - Deny write and execute to others for dir

#### Numeric Method

-   Uses a three-digit mode nummber
    -   first digit specifies **owner's** permissions
    -   second digit specifies **group** permissions
    -   third digit represents **others'** permissions
-   Permissions are calculated by adding :
    -   4 (for read)
    -   2 (for write)
    -   1 (for execute)
-   Example :
    -   `$ chmod 640 myfile`

### Changing ownership

-   To change ownership :
    -   `$ chown [-option]... user:group file|directory`
-   Options include :
    -   **-R** Recursive
    -   **-v** Verbose
-   Examples :
    -   `$ chown -R student dir` - Changes the user ownership to _student_ recursively
    -   `$ chown :admins file` - Changesthe group ownership to _admins_
    -   `$ chown aws:devops file` - Changes the user ownership to _aws_ and group ownership to _devops_

### Special Permissions

| Special Permission | Effect on files                                                              | Effect on directories                                                                               |
| ------------------ | ---------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **u+s** (suid)     | File executes as the user that owns the file, not the user that ran the file | No effect                                                                                           |
| **g+s** (guid)     | Files executes as the group that owns the file                               | Files newly created in the directory have their owner set to match the group owner of the directory |
| **o+t** (sticky)   | No effect                                                                    | Users with write access to directory can only remove files they own                                 |

-   Can be set as follows :
    -   **Symbolically** : **setuid** = `u+s`; **setgid** = `g+s`; **sticky** = `o+t`
    -   **Numerically** (4th preceding digit) : **setuid** = 4; **setgid** = 2; **sticky** = 1
    -   Examples :
        -   `$ chmod g+s directory`
        -   `$ chmod 2770 directory #equivalent to g+s as 2 is written`

#### setuid permission

-   The **setuid** (suid) permission on an executable means that commands run as the user owning the file, not as the user that ran the command.
-   Example :
    -   `$ ls -l /usr/bin/passwd`
        ```
        -rwsr-xr-x. 1 root root 35504 Jul 16 2010 /usr/bin/passwd
        ```
    -   Here **s** is the **setuid** permission, written where normally execute (x) permission is written.
-   If the owner does not have execute permission, it will be replaced by an uppercase **S**.

#### setgid permission

-   The **setgid** (sgid) permission on a directory means that files created in the directory inherit their group ownership from the directory, rather than inheriting it from the creating user.
-   Example :
    -   `$ ls -ld /run/log/journal`
        ```
        drwxr-sr-x 3 root systemd-journal 60 May 18 09:15 /run/log/journal
        ```
    -   Here **s** is the **setgid** permission, written where normally execute (x) permission is written.
-   If the group does not have execute permission, it will be replaced by an uppercase **S**.

#### sticky permission

-   The **sticky bit** for a directory sets a special restriction on deletion of files.
-   Only the owner of the file (and root) can delete files within the directory.
-   Example :
    -   `$ ls -ld /tmp`
        ```
        drwxrwxrwt 39 root 4096 Feb 8 20:52 /tmp
        ```
    -   Here **t** is the **sticky** permission, written where normally execute (x) permission is written.
-   If others do not have execute permission, it will be replaced by an uppercase **T**.

### Default File Permissions - umask

-   If we create a new directory, default permissions are : 0777 (drwxrwxrwx)
-   If we create a new regular file, default permissions are : 0666 (-rw-rw-rw-)
    <br>
-   However, we can change it using **umask**
-   If a bit is set in **umask**, then corresponding permission will be cleared on new file.
-   For eg, the `$ umask 0002` clears the wirte bit for other users.
-   The `umask ` command without arguments will display the current value of the shell's umask. `$ umask` - `002`

## Sudo

-   sudo gives power to a normal user to execue commands which is owned by root user.
-   Not every user have the privilige to do sudo.
-   For giving any user or group acces to sudo we have to add them in `/etc/sudoers` file.
    -   `$ ls -l /etc/sudoers` - `-r--r-----. 1 root root 4328 Feb 12 17:54 /etc/sudoers`
    -   So event root user don't have write access to this file
    -   We **should not** change this file's permissions, instead we should write command `$ visudo` which will open this file in write mode.
    -   Search for the line `root ALL=(ALL) ALL` and insert new line below it to give any other user or group sudo privilige.
    -   For giving privilige to group, add a percentage sign (%) before the name.
    -   For giving privilige to a user or group so that password is not required for doing sudo, write `NOPASSWD:`
    -   Example :
        ```
        ....
        root        ALL=(ALL)       ALL
        ansible     ALL=(ALL)       NOPASSWD:       ALL
        %aws        ALL=(ALL)       ALL
        ...
        ```
-   For preventing any human error in `/etc/sudoers` file, we make a new file for the user or group we want to give sudo privilige in the `/etc/sudoers.d` directory.

## Package Management

### Basic way of installing any package in CentOS

-   Go to browser, search for package and copy the link
-   `$ curl <package_link> -o <package_name>.rpm`
-   `rpm [-i: interactive] [-v: verbose] [-h: human readable] <package_name>.rpm`

|                                                   |                                                                                |                                                              |
| ------------------------------------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------ |
| `$ rpm -qa`                                       | Display list of all installed packages                                         | `$ rpm -qa`                                                  |
| `$ rpm -qi {package}`                             | Display installed information along with package version and short description | `$ rpm -qi mozilla-mail`                                     |
| `$ rpm -qf {/path/to/file}`                       | Find out what package a file belongs to, i.e., find what package owns the file | `$ rpm -qf /etc/passwd`                                      |
| `$ rpm -qc {package-name} `                       | Display list of configuration file(s) for a package                            | `$ rpm -qc httpd`                                            |
| `$ rpm -qcf {/path/to/file}`                      | Display list of configuration files for a command                              | `$ rpm -qcf /usr/X11R6/bin/xeyes`                            |
| `$ rpm -qa --last`                                | Display list of all recently installed RPMs                                    | `$ rpm -qa --last`                                           |
| `$ rpm -qpR {.rpm-file}`<br>`$ rpm -qR {package}` | Find out what dependencies a rpm file has                                      | `$ rpm -qpR mediawiki-1.4rc1-4.i586.rpm`<br>`$ rpm -qR bash` |

### yum

-   **yum** is a package manager which manages and downloads all the dependencies along with the required package

|                                              |                                            |
| -------------------------------------------- | ------------------------------------------ |
| `$ yum search PACKAGE`                       | search from available repositories         |
| `$ yum -y install PACKAGE`                   |
| `$ yum reinstall PACKAGE`                    |
| `$ yum remove package`                       |
| `$ yum update`                               | update all packages                        |
| `$ yum updata PACKAGE`                       | update only a package                      |
| `$ yum grouplist`                            | List all available Group Packages          |
| `yum group install "GROUPNAME"`              | Installs all the packages in a group       |
| `$ yum repolist`                             | List enabled yum repositories              |
| `$ yum --enablerepo=epel install phpmyadmin` | Install a package from Specific repository |
| `$ yum clean all`                            | Clean yum cache                            |
| `$ yum history`                              | View history of yum                        |
| `$ yum info packagename`                     |

## Services

-   A service is a program that runs in the background otuside the interactive control of system users as they lack an interface.
-   `systemctl` command is used for managing services in linux

### Basic commands

-   `$ systemctl status httpd`
-   `$ systemctl start httpd`
-   `$ systemctl restart httpd`
-   `$ systemctl reload httpd` - reloads the config without restarting it
-   `$ systemctl stop httpd`
-   `$ systemctl enable httpd` - enables the service to start at boot time.
-   `$ systemctl is-active httpd`
-   `$ systemctl is-enabled httpd`

## Processes

-   `$ top` - show all the process running on realtime basis
-   `$ ps aux` - similar to top but displays info and quits
-   `$ps -ef` - shows Parent Process ID (PPID) but don't show the CPU and MEM Utilization

### Killing Processes

-   `kill` command is used to kill the processes.

| Signal Number | Short Name | Definition         | Purpose                                                                                                                                                  |
| ------------- | ---------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1             | HUP        | Hangup             | Used to report termination of the controlling of a terminal. Also used to request process reinitialization (config reload) without termination           |
| 2             | INT        | Keyboard Interrupt | Causes program termination. Can be blocked or handled. Sent by pressing INTR key sequence (Ctrl + c)                                                     |
| 3             | QUIT       | Keyboard Quit      | Similar to SIGINT; adds a process dump at termination. Sent by pressing QUIT key sequence (Ctrl + \\)                                                    |
| 9             | KILL       | Kill, unblockable  | Causes abruoot program termination. Cannot be blocked, ignored, or handled; always fatal                                                                 |
| 15 default    | TERM       | Terminate          | Causes program termination. Unlike SIGKILLm can be blocked, ignored, or handled. The "polite" way to use ask a program to terminate; allows self-cleanup |
| 18            | CONT       | Continue           | Sent to a process to resume, if stopped. Cannot be blocked. Even if handled, always resumes the process                                                  |
| 19            | STOP       | Stop, unblockable  | Suspends the process. Cannot be bolocked or handled                                                                                                      |
| 20            | TSIP       | keyboard Stop      | Unlike SIGSTOP, can be blocked, ignored or handled. Sent by pressing SUSP key sequence (Ctrl + z)                                                        |

-   Signals can be specified to `kill` command as options either by name (eg -HUP or -SIGHUP) or by number (the related -1).
-   Users can kill their own processes, but root privilige is required to kill processes owned by others.

## Archiving Data

-   `$ tar -czvf <archivename>.tar.gz file1 file2...` - Archiving Data
-   `$ tar -xzvf archivename.tar.gz -C /opt/ #-C is for giving a path where we want to unzip files` - Unziping data

## Ubuntu Commands

-   In ubuntu `useradd` command don't create any home directory and many other things, so instead we can use `adduser` command.

## Basic Networking Commands

### Protocols & Port Numbers

| Protocol | Service Name              | UDP & TCP Port No.s        |
| -------- | ------------------------- | -------------------------- |
| DNS      | Domain Name Service - UDP | UDP 53                     |
| DNS TCP  | Domain Name Service - TCP | TCP 53                     |
| HTTP     | Web                       | TCP 80                     |
| HTTPS    | Secure Web (SSL)          | TCP 443                    |
| SMTP     | Simple Mail Transport     | TCP 25                     |
| POP      | Post Office Protocol      | TCP 109, 110               |
| SNMP     | Simple Network Management | TCP 161, 162, UDP 161, 162 |
| TELNET   | Telnet Terminal           | TCP 23                     |
| FTP      | File Transfer Protocol    | TCP 20, 21                 |
| SSH      | Secure Shell (terminal)   | TCP 22                     |
| AFP IP   | Apple File Protocol/IP    | TCP 447, 548               |

### ifconfig

-   Shows the active network interfaces, their names, and IP Addresses.
-   ```
    $ ifconfig

      enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
              inet 10.0.2.15  netmask 255.255.255.0  broadcast 10.0.2.255
              inet6 fe80::50:97ff:feae:4744  prefixlen 64  scopeid 0x20<link>
              ether 02:50:97:ae:47:44  txqueuelen 1000  (Ethernet)
              RX packets 592  bytes 70109 (70.1 KB)
              RX errors 0  dropped 0  overruns 0  frame 0
              TX packets 419  bytes 75825 (75.8 KB)
              TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

      enp0s8: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
              inet 192.168.33.10  netmask 255.255.255.0  broadcast 192.168.33.255
              inet6 fe80::a00:27ff:fe2c:8992  prefixlen 64  scopeid 0x20<link>
              ether 08:00:27:2c:89:92  txqueuelen 1000  (Ethernet)
              RX packets 0  bytes 0 (0.0 B)
              RX errors 0  dropped 0  overruns 0  frame 0
              TX packets 8  bytes 656 (656.0 B)
              TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

      enp0s9: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
              inet 192.168.1.33  netmask 255.255.255.0  broadcast 192.168.1.255
              inet6 fe80::a00:27ff:fe94:7c  prefixlen 64  scopeid 0x20<link>
              ether 08:00:27:94:00:7c  txqueuelen 1000  (Ethernet)
              RX packets 4  bytes 1300 (1.3 KB)
              RX errors 0  dropped 0  overruns 0  frame 0
              TX packets 10  bytes 1342 (1.3 KB)
              TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

      lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
              inet 127.0.0.1  netmask 255.0.0.0
              inet6 ::1  prefixlen 128  scopeid 0x10<host>
              loop  txqueuelen 1000  (Local Loopback)
              RX packets 0  bytes 0 (0.0 B)
              RX errors 0  dropped 0  overruns 0  frame 0
              TX packets 0  bytes 0 (0.0 B)
              TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

    ```

### ip addr show

-   In newer linux systems, `ifconfig` command is not available.
-   ```
    $ ip addr show

      1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
          link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
          inet 127.0.0.1/8 scope host lo
          valid_lft forever preferred_lft forever
          inet6 ::1/128 scope host
          valid_lft forever preferred_lft forever
      2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
          link/ether 02:50:97:ae:47:44 brd ff:ff:ff:ff:ff:ff
          inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic enp0s3
          valid_lft 86302sec preferred_lft 86302sec
          inet6 fe80::50:97ff:feae:4744/64 scope link
          valid_lft forever preferred_lft forever
      3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
          link/ether 08:00:27:2c:89:92 brd ff:ff:ff:ff:ff:ff
          inet 192.168.33.10/24 brd 192.168.33.255 scope global enp0s8
          valid_lft forever preferred_lft forever
          inet6 fe80::a00:27ff:fe2c:8992/64 scope link
          valid_lft forever preferred_lft forever
      4: enp0s9: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
          link/ether 08:00:27:94:00:7c brd ff:ff:ff:ff:ff:ff
          inet 192.168.1.33/24 brd 192.168.1.255 scope global dynamic enp0s9
          valid_lft 86303sec preferred_lft 86303sec
          inet6 fe80::a00:27ff:fe94:7c/64 scope link
          valid_lft forever preferred_lft forever
    ```

### ping

-   Sends `icmp` packets to the specified IP.
-   ```
    $ ping 192.168.33.10

      PING 192.168.33.10 (192.168.33.10) 56(84) bytes of data.
      64 bytes from 192.168.33.10: icmp_seq=1 ttl=64 time=0.054 ms
      64 bytes from 192.168.33.10: icmp_seq=2 ttl=64 time=0.057 ms
      64 bytes from 192.168.33.10: icmp_seq=3 ttl=64 time=0.052 ms
      64 bytes from 192.168.33.10: icmp_seq=4 ttl=64 time=0.091 ms
      ^C
      --- 192.168.33.10 ping statistics ---
      4 packets transmitted, 4 received, 0% packet loss, time 3079ms
    ```

### netstat

-   Prints network connections, routing tables, interface statistics, masquerade connections, and multicast memberships.
-   ```
    # Basic Options for netstat :
        # [--all|-a]: Show both listening and non-listening sockets
        # [--numeric|-n]: Show numerical addresses instead of trying to determine symbolic host, port or user names.
        # [--tcp|-t]
        # [--program|-p]: Show th PID and name of the program to which each socket belongs.

    $ sudo -i       # So that below command shows the PID
    $ netstat -antp

        Active Internet connections (servers and established)
        Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
        tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      650/systemd-resolve
        tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      851/sshd
        tcp        0      0 10.0.2.15:51362         91.189.91.39:80         TIME_WAIT   -
        tcp        0      0 10.0.2.15:22            10.0.2.2:63898          ESTABLISHED 1599/sshd: vagrant
        tcp6       0      0 :::22                   :::*                    LISTEN      851/sshd
        tcp6       0      0 :::80                   :::*                    LISTEN      2425/apache2
    ```

-   A **Practical Usecase**, finding the port no for **Apache2** Service:

    ```
    $ ps -ef | grep apache2

      root      2425     1  0 12:19 ?        00:00:00 /usr/sbin/apache2 -k start
      www-data  2426  2425  0 12:19 ?        00:00:00 /usr/sbin/apache2 -k start
      www-data  2428  2425  0 12:19 ?        00:00:00 /usr/sbin/apache2 -k start
      root      6091  1815  0 12:23 pts/0    00:00:00 grep --color=auto apache2

    $ netstat -antp | grep 2425          # PID obtained using above command

      tcp6       0      0 :::80                   :::*                    LISTEN      2425/apache2
    ```

### ss

-   Another utility to investigate sockets
-   ```
    $ ss -tunlp

      NetidState  Recv-Q Send-Q         Local Address:Port   Peer Address:Port
      udp  UNCONN 0      0              127.0.0.53%lo:53          0.0.0.0:*     users:(("systemd-resolve",pid=650,fd=12))
      udp  UNCONN 0      0        192.168.1.33%enp0s9:68          0.0.0.0:*     users:(("systemd-network",pid=1432,fd=25))
      udp  UNCONN 0      0           10.0.2.15%enp0s3:68          0.0.0.0:*     users:(("systemd-network",pid=1432,fd=22))
      tcp  LISTEN 0      128            127.0.0.53%lo:53          0.0.0.0:*     users:(("systemd-resolve",pid=650,fd=13))
      tcp  LISTEN 0      128                  0.0.0.0:22          0.0.0.0:*     users:(("sshd",pid=851,fd=3))
      tcp  LISTEN 0      128                     [::]:22             [::]:*     users:(("sshd",pid=851,fd=4))
      tcp  LISTEN 0      128                        *:80                *:*     users:(("apache2",pid=2428,fd=4),("apache2",pid=2426,fd=4),("apache2",pid=2425,fd=4))
    ```

### nmap

-   Network exploration tool and security / port explorer
-   Can be used to detect all open ports of a machine
-   ```
    $ nmap localhost

      Starting Nmap 7.60 ( https://nmap.org ) at 2022-05-29 12:34 UTC
      Nmap scan report for localhost (127.0.0.1)
      Host is up (0.0000090s latency).
      Not shown: 998 closed ports
      PORT   STATE SERVICE
      22/tcp open  ssh
      80/tcp open  http
    ```

### dig

-   A **DNS** Lookup Utility
-   ```
    $ dig www.coderchirag.tech
      ; <<>> DiG 9.11.3-1ubuntu1.17-Ubuntu <<>> www.coderchirag.tech
      ;; global options: +cmd
      ;; Got answer:
      ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 23161
      ;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

      ;; OPT PSEUDOSECTION:
      ; EDNS: version: 0, flags:; udp: 65494
      ;; QUESTION SECTION:
      ;www.coderchirag.tech.          IN      A

      ;; ANSWER SECTION:
      www.coderchirag.tech.   28800   IN      CNAME   cname.vercel-dns.com.
      cname.vercel-dns.com.   46      IN      A       76.76.21.164
      cname.vercel-dns.com.   46      IN      A       76.76.21.93

      ;; Query time: 511 msec
      ;; SERVER: 127.0.0.53#53(127.0.0.53)
      ;; WHEN: Sun May 29 12:37:05 UTC 2022
      ;; MSG SIZE  rcvd: 115
    ```

### route

-   Show / Manipulate the IP routing table
-   ```
    $ route -n    #[--numeric|-n]: Show numerical addresses

    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    0.0.0.0         10.0.2.2        0.0.0.0         UG    100    0        0 enp0s3
    0.0.0.0         192.168.1.1     0.0.0.0         UG    100    0        0 enp0s9
    10.0.2.0        0.0.0.0         255.255.255.0   U     0      0        0 enp0s3
    10.0.2.2        0.0.0.0         255.255.255.255 UH    100    0        0 enp0s3
    192.168.1.0     0.0.0.0         255.255.255.0   U     0      0        0 enp0s9
    192.168.1.1     0.0.0.0         255.255.255.255 UH    100    0        0 enp0s9
    192.168.33.0    0.0.0.0         255.255.255.0   U     0      0        0 enp0s8
    ```

## Server Management in Linux

**NOTE, Before going forward to this section make sure :**

-   You have learnt about **vagrant** from the [vagrant branch](https://github.com/CoderChirag/DevOps-Learning/tree/vagrant)
-   Have learnt about **apache** from the [apache2 branch](https://github.com/CoderChirag/DevOps-Learning/tree/apache2)
-   Have learnt about **nginx** from the [nginx branch](https://github.com/CoderChirag/DevOps-Learning/tree/nginx)

### Setting up a website in CentOS7

```
$ mkdir crispy_kitchen
$ cd crispy_kitchen/
$ vagrant init geerlingguy/centos7
$ vim Vagrantfile
```

-   Give Bridged IP and required RAM by uncommenting and changing the following blocks in **Vagrantfile** accordingly :
    ```
    ...
    config.vm.network "public_network"
    ...
     config.vm.provider "virtualbox" do |vb|
    #    # Display the VirtualBox GUI when bootin the machine
    #    vb.gui = true
    #
    #    # Customize the amount of memory on tthe VM:
        vb.memory = "1024"
    end
    ...
    ```

```
$ vagrant up
$ vagrant ssh
$ sudo -i
$ yum install httpd wget unzip -y
$ systemctl start httpd
$ systemctl enable httpd
```

-   Now run `$ ifconfig` command, copy the bridged network IP and paste in the browser to check if default page appears.

<div align='center'>

![apache_default_page](./images/default_page_apache.png)

</div>

-   Now go to the `/var/www` directory and creat an `index.html` file to show the content you want, instead of the default page.

**Serving a website by downloading a template**

```
$ cd /tmp/
$ wget https://www.tooplate.com/zip-templates/2129_crispy_kitchen.zip
$ unzip 2129_crispy_kitchen.zip
$ cd 2129_crispy_kitchen
$ cp -r * /var/www/html
$ systemctl restart httpd
```

-   And with this we have hosted a website :sunglasses: . Go to browser, type the bridged IP and check if it shows the website.

### Automating the Static Website Setup - Infrastucture as a Code (IAAC)

-   We can use Vagrant provisioning to automate the setup
-   `vim Vagrantfile`
-   ```
    Vagrant.configure("2") do |config|

      config.vm.box = "geerlingguy/centos7"

      config.vm.network "private_network", ip: "192.168.33.10"

      config.vm.network "public_network"

      config.vm.provision "shell", inline: <<-SHELL
          yum install httpd wget unzip -y
          systemctl start httpd
          systemctl enable httpd
          cd /tmp/
          wget https://www.tooplate.com/zip-templates/2129_crispy_kitchen.zip
          unzip -o 2129_crispy_kitchen.zip
          cp -r 2129_crispy_kitchen.zip/* /var/www/html
          systemctl restart httpd
      SHELL
    end
    ```

-   Now, just do `vagrant up`, and our website would be hosted automatically
-   This process is called **Infrastructure as a Code (IAAC)**.

### Setting up a Wordpress Website using LAMP (Linux, Apache, MySQL, PHP) Stack

#### Configuring VM and Installing Dependencies

```
$ mkdir wordpress
$ cd wordpress
$ vagrant init ubuntu/bionic64
$ vim Vagrantfile
```

-   Give Static IP and bridged IP by uncommenting and changing the following blocks in **Vagrantfile** accordingly :
    ```
    ...
    config.vm.network "private_network", ip: "192.168.33.11"
    config.vm.network "public_network"
    ...
    ```

```
$ vagrant up
$ vagrant ssh
$ sudo -i
$ apt update
$ apt install apache2 \
              ghostscript \
              libapache2-mod-php \
              mysql-server \
              php \
              php-bcmath \
              php-curl \
              php-imagick \
              php-intl \
              php-json \
              php-mbstring \
              php-mysql \
              php-xml \
              php-zip -y
```

#### Installing WordPress

```
$ mkdir -p /srv/www
$ chown www-data: /srv/www
$ curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www
```

#### Configuring Apache for WordPress

-   Create Apache site for Wordpress. Create `/etc/apache2/sites-available/wordpress.conf` with following lines :
    ```
    <VirtualHost *:80>
        DocumentRoot /srv/www/wordpress
        <Directory /srv/www/wordpress>
            Options FollowSymLinks
            AllowOverride Limit Options FileInfo
            DirectoryIndex index.php
            Require all granted
        </Directory>
        <Directory /srv/www/wordpress/wp-content>
            Options FollowSymLinks
            Require all granted
        </Directory>
    </VirtualHost>
    ```
-   Enable the site with `$ sudo a2ensite wordpress`. Now if we check there will be a link in `/etc/apache2/sites-enabled` to the `wordpess.conf` file present in the `/etc/apache2/sites-available` directory.
-   Enable URL rewriting with : `$ sudo a2enmod rewrite`. This will enable the URL rewriting rules.
-   Disable the default "It Works site with : `$ sudo a2dissite 000-default`. Now the link of file `000-default.conf` file would be gone from `/etc/apache2/sites-enabled` directory, which means we have disabled the default page of apache2.
-   Finally reload apache2 to apply the changes `$ sudo service apache2 reload`

#### Configuring database

```
$ sudo mysql -u root
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 7
Server version: 5.7.20-0ubuntu0.16.04.1 (Ubuntu)

Copyright (c) 2000, 2017, Oracle and/or it affiliates. All rights reserved.

Oracle is a registered trademark of oracle Corporation and/or its affiliates. Other names may be trademarks of thier respective owners.

Type 'help;' or '\h' for help. type '\c' to clear the current input statement.

mysql> CREATE DATABASE wordpress;
Query Ok, 1 row affected (0,00 sec)

mysql> CREATE USER wordpress@localhost IDENTIFIED by '<your-password>';
Query OK, 1 row affected (0,00 sec)

mysql> GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
    -> ON wordpress.*
    -> TO wordpress@localhost;
Query OK, 1 row affected (0,00 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 1 row affected (0,00 sec)

mysql> quit
Bye
```

#### Configuring Wordpress to connect to the database

-   First, copy the sample configuration file to wp-config.php : <br> `$ sudo -u www-data cp /srv/www/wordpress/wp-config-sample.php /srv/www/wordpress/wp-config.php`
-   Now, set the database credentials in the configuration file (do not replace **database_name_here** or **username_here** in the commands below.) Do replace **\<your-password>** with your database password :
    ```
    $ sudo -u www-data sed -i 's/database_name_here/wordpress/' /srv/www/wordpress/wp-config.php
    $ sudo -u www-data sed -i 's/username_here/wordpress/' /srv/www/wordpress/wp-config.php
    $ sudo -u www-data sed -i 's/password_here/<your-password>/' /srv/www/wordpress/wp-config.php
    ```
-   Finally, in a terminal session open the connfig file in vim : `$ sudo -u www-data nano /srv/www/wordpress/wp-config.php`
-   Find the following :
    ```
    define('AUTH_KEY',         'put your unique phrase here');
    define('SECURE_AUTH_KEY',  'put your unique phrase here');
    define('LOGGED_IN_KEY',    'put your unique phrase here');
    define('NONCE_KEY',        'put your unique phrase here');
    define('AUTH_SALT',        'put your unique phrase here');
    define('SECURE_AUTH_SALT', 'put your unique phrase here');
    define('LOGGED_IN_SALT',   'put your unique phrase here');
    define('NONCE_SALT',       'put your unique phrase here');
    ```
    Delete those line (8dd will delete all lines). Then replace with the content of [this](https://api.wordpress.org/secret-key/1.1/salt). (This adress is a randomiser that returns completely random keys each time it is opened.) This step is impoortant to ensure that our site is not vulnerable to "known secrets" attacks.
    <br><br>
    Save and close the configuration file by hitting `esc` and then writing `:wq` and pressing `enter`.
    <br><br>
-   Now go to the bridged IP or static IP in the browser and configure the owrdpress there.
    <br><br>

With this we have hosted wordpress in a VM server successfuly :sunglasses:.

### Automating Wordpress Website Setup using IAAC

-   `vim Vagrantfile`
-   ```
    Vagrant.configure("2") do |config|

      config.vm.box = "ubuntu/bionic64"

      config.vm.network "private_network", ip: "192.168.33.11"

      config.vm.network "public_network"

      config.vm.provision "shell", inline: <<-SHELL
          sudo apt update
          sudo apt install apache2 \
                           ghostscript \
                           libapache2-mod-php \
                           mysql-server \
                           php \
                           php-bcmath \
                           php-curl \
                           php-imagick \
                           php-intl \
                           php-json \
                           php-mbstring \
                           php-mysql \
                           php-xml \
                           php-zip -y

          sudo mkdir -p /srv/www
          sudo chown www-data: /srv/www
          curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www

          cp /vagrant/wordpress.conf /etc/apache2/sites-available/wordpress.conf
          sudo a2ensite wordpress
          sudo a2enmod rewrite
          sudo a2dissite 000-default
          sudo service apache2 reload

          mysql -u root -e 'CREATE DATABASE wordpress;'
          mysql -u root -e 'CREATE USER wordpress@localhost IDENTIFIED BY "admin123";'
          mysql -u root -e 'GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, ALTER ON wordpress.* TO wordpress@localhost;'
          mysql -u root -e 'FLUSH PRIVILEGES;'

          sudo -u www-data cp /srv/www/wordpress/wp-config-sample.php /srv/www/wordpress/wp-config.php
          sudo -u www-data sed -i 's/database_name_here/wordpress/' /srv/www/wordpress/wp-config.php
          sudo -u www-data sed -i 's/username_here/wordpress/' /srv/www/wordpress/wp-config.php
          sudo -u www-data sed -i 's/password_here/admin123/' /srv/www/wordpress/wp-config.php
      SHELL
    end
    ```

-   Now simply do `vagrant up` and you are good to go :sunglasses:

### Setting up a Nodejs Application

#### Using Apache2

##### Configuring VM

```
$ mkdir nodejs
$ cd nodejs
$ vagrant init ubuntu/bionic64
$ vim VagrantFile       # Configure the bridged and static IP address
$ vagrant up
$ vagrant ssh
```

##### Installing and Starting Apache server

```
$ sudo -i
$ apt-get update
$ apt-get install apache2
```

-   Open the browser and go to bridged or static IP and check if default page of apache2 is showing.
-   Confirm the status of apache2 : `$ systemctl status apache2`

##### Installing Nodejs

-   First, install the NodeSource PPA in order to get access to its contents. Make sure you’re in your home directory, and use `curl` to retrieve the installation script for the Node.js 16.x archives:
    ```
    $ cd ~
    $ curl -sL https://deb.nodesource.com/setup_16.x -o nodesource_setup.sh
    ```
-   We can inspect the contents of this script with `vim` : `$ vim nodesource_setup.sh`
-   Run the script : `$ bash nodesource_setup.sh`
-   The PPA will be added to our configuration and our local package cache will be updated automatically. After running the setup script from Nodesource, we can install the Node.js package : `$ apt install nodejs`.
-   To check which version of Node.js you have installed after these initial steps, type :
    ```
    $ nodejs -v
    v8.11.3
    ```
-   In order for some npm packages to work (those that require compiling code from source, for example), we will need to install the build-essential package: <br> `$ apt install build-essential`

##### Setting up MongoDb database

###### Installing MongoDB

-   To obtain the most recent version of this software, we must include MongoDB’s dedicated package repository to our APT sources. Then, we’ll be able to install `mongodb-org`, a meta-package that always points to the latest version of MongoDB.
-   To start, import the public GPG key for the latest stable version of MongoDB by running the following command.
    `$ curl -fsSL https://www.mongodb.org/static/pgp/server-4.4.asc | apt-key add -`
    -   cURL prints the content of the GPG key file and then pipes it into the following `sudo apt-key add -` command, thereby adding the GPG key to our list of trusted keys.
    -   Also, note that this `curl` command uses the options `-fsSL` which, together, essentially tell cURL to fail silently. This means that if for some reason cURL isn’t able to contact the GPG server or the GPG server is down, it won’t accidentally add the resulting error code to your list of trusted keys.
    -   If you’d like to double check that the key was added correctly, you can do so with the following command:
        `$ apt-key list`
-   At this point, our APT installation still doesn’t know where to find the `mongodb-org` package we need to install the latest version of MongoDB.
-   There are two places on our server where APT looks for online sources of packages to download and install: the `sources.list` file and the `sources.list.d` directory. `sources.list` is a file that lists active sources of APT data, with one source per line and the most preferred sources listed first. The `sources.list.d` directory allows us to add such `sources.list` entries as separate files.
    `$ echo "deb [ arch=amd64,arm64 ] https://repo.monoodb.org/apt/ubuntu bionic/mongodb-org/4.4 multiverse"`
    This single line tells APT everything it needs to know about what the source is and where to find it
-   ```
    $ apt update
    $ apt install mongodb-org -y
    ```

###### Starting the MongoDB Service and Testing the Database

```
$ systemctl start mongod.service
$ systemctl status mongod       # Check if mongod is running
```

-   If it is showing the following error :

    ```
    ● mongod.service - MongoDB Database Server
     Loaded: loaded (/lib/systemd/system/mongod.service; disabled; vendor preset: enabled)
     Active: failed (Result: exit-code) since Sat 2022-05-21 17:30:45 UTC; 4min 10s ago
       Docs: https://docs.mongodb.org/manual
    Process: 2887 ExecStart=/usr/bin/mongod --config /etc/mongod.conf (code=exited, status=14)
    Main PID: 2887 (code=exited, status=14)

    May 21 17:30:45 ubuntu-bionic systemd[1]: Started MongoDB Database Server.
    May 21 17:30:45 ubuntu-bionic systemd[1]: mongod.service: Main process exited, code=exited, status=14/n/a
    May 21 17:30:45 ubuntu-bionic systemd[1]: mongod.service: Failed with result 'exit-code'.
    ```

    Change the owner of `.sock` file to `mongodb` user :

    ```
    $ chown -R mongodb:mongodb /var/lib/mongodb
    $ chown mongodb:mongodb /tmp/mongodb-27017.sock
    ```

-   `$ systemctl enable mongod`
-   We can further verify that the database is operational by connecting to the database server and executing a diagnostic command. The following command will connect to the database and output its current version, server address, and port. It will also return the result of MongoDB’s internal connectionStatus command:
    `$ mongo --eval 'db.runCommand({connectionStatus: 1})'`

##### Setting up the Node.js application

-   `$ cd /srv/www/`
-   Clone the repository from GitHub : `$ git clone https://github.com/debemenitammy/API_for_a_Store.git`
-   `cd API_for_a_Store`
-   Run `$ npm install` command
-   Make a new `.env` file and write `DB_CONNECTION=mongodb://127.0.0.1:27017`

###### Installing PM2

-   **PM2** is a process manager for Node.js applications. PM2 makes it possible to daemonize applications so that they will run in the background as a service.
-   ```
    $ npm install pm2@latest -g
    $ pm2 start app.js
    $ pm2 startup sytemd
    ```

###### Configuring Apche Server for Node.js

-   `$ cd /etc/apache2/sites-available`
-   Open the `000-default.conf` default configuration file in order to make edits:
    `$ vim 000-default.conf`
-   We’ll configure the `000-default.conf` file so that all requests coming in via `port 80` will be proxied, or forwarded, to the Node application running on `port 3000`. We use `ProxyPass` to map the root URL at the specified address: `http://localhost:3000/`
-   Copy the following into the `000-default.conf` file inside `VirtualHost` tag: `ProxyPass / http://localhost:3000/`
-   Enable `proxy` and `proxy_http` modules :
    ```
    $ a2enmod proxy
    $ a2enmod proxy_http
    ```
-   Restart apache2 to apply the config : `$ systemctl reload apche2`
-   To check ifApche config is configured correctly, go to bbrowser and paste bridged or static IP, you'll see the node application redirecting you to postman documentation page.
    <br><br><br>

With this we have configure Node.js application for the apache server successfully :sunglasses:.
