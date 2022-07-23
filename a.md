## Managing SELinux Security

### Changing the SELinux Enforcement Mode

#### How SELinux Protects Resources

-   SELinux is an important security feature of Linux.
-   Access to files and other resources is controlled at a very granular level.
-   Processes are only permitted to access the resources specified by their policy, or SELinux boolean settings.
    <br>

-   File permissions control which users or groups of users can access which specific files.
-   However, a user given read or write access to any specific file can use that file in any way that user chooses, even if that use is not how the file should be used.
    <br>

-   For example, with write access to a file, should a structured data file designed to be written to using only a particular program, be allowed to be opened and modified by other editors that could result in corruption?
    <br>

-   File permissions cannot stop such undesired access. They were never designed to control how a file is used, but only who is allowed to read, write, or run a file.
    <br>

-   SELinux consists of sets of policies, defined by the application developers, that declare exactly what actions and accesses are proper and allowed for each binary executable, configuration file, and data file used by an application.
-   This is known as a **targeted policy** because one policy is written to cover the activities of a single application.
-   Policies declare predefined labels that are placed on individual programs, files, and network ports.

#### Why use Security Enhanced Linux?

-   Not all security issues can be predicted in advance.
-   SELinux enforces a set of access rules preventing a weakness in one application from affecting other applications or the underlying system.
-   SELinux provides an extra layer of security; it also adds a layer of complexity which can be off-putting to people new to this subsystem.
-   Learning to work with SELinux may take time but the enforcement policy means that a weakness in one part of the system does not spread to other parts.
-   If SELinux works poorly with a particular subsystem, we can turn off enforcement for that specific service until we find a solution to the underlying problem.
    <br>

-   SELinux has three modes:

    -   **Enforcing :** SELinux is enforcing access control rules. Computers generally run in this mode.
    -   **Permissive :** SELinux is active but instead of enforcing access control rules, it records warnings of rules that have been violated. This mode is used primarily for testing and troubleshooting.
    -   **Disabled :** SELinux is turned off entirely: no SELinux violations are denied, nor even recorded. Discouraged!

#### Basic SELinux security concepts

-   Security Enhanced Linux (SELinux) is an additional layer of system security.
-   The primary goal of SELinux is to protect user data from system services that have been compromised.
-   Most Linux administrators are familiar with the standard `user/group/other` permission security model. This is a user and group based model known as **discretionary access control**.
-   SELinux provides an additional layer of security that is object-based and controlled by more sophisticated rules, known as **mandatory access control**.
    <br>

-   To allow remote anonymous access to a web server, firewall ports must be opened.
-   However, this gives malicious people an opportunity to compromise the system through a vulnerability. If they succeed in compromising the web server process they gain its permissions. Specifically, the permissions of the apache user and the apache group.
-   That user and group has read access to the document root, `/var/www/html`. It also has access to `/tmp`, and `/var/tmp`, and any other files and directories that are world writable.
    <br>

-   SELinux is a set of security rules that determine which process can access which files, directories, and ports.
-   Every file, process, directory, and port has a special security label called an SELinux context.
-   A context is a name used by the SELinux policy to determine whether a process can access a file, directory, or port.
-   By default, the policy does not allow any interaction unless an explicit rule grants access. If there is no allow rule, no access is allowed.
    <br>

-   SELinux labels have several contexts: `user`, `role`, `type`, and `sensitivity`.
-   The targeted policy, which is the default policy enabled in Red Hat Enterprise Linux, bases its rules on the third context: the **type context**.
-   Type context names usually end with `_t`.
    ![SELinux_context](./images/selinux_context.png)
    <br>

-   The type context for a **web server** is `httpd_t`.
-   The type context for **files and directories** normally found in `/var/www/html` is `httpd_sys_content_t`.
-   The contexts for **files and directories** normally found in `/tmp` and `/var/tmp` is `tmp_t`.
-   The type context for **web server ports** is `http_port_t`.
    <br>

-   **Apache** has a type context of `httpd_t`.
-   There is a policy rule that permits Apache access to files and directories with the `httpd_sys_content_t` type context.
-   By default files found in `/var/www/html` and other web server directories have the `httpd_sys_content_t` type context.
-   There is no allow rule in the policy for files normally found in `/tmp` and `/var/tmp`, so access is not permitted.
-   With SELinux enabled, a malicious user who had compromised the web server process could not access the `/tmp` directory.
    <br>

-   The **MariaDB server** has a type context of `mysqld_t`.
-   By default, files found in `/data/mysql` have the `mysqld_db_t` type context.
-   This type context allows MariaDB access to those files but disables access by other services, such as the Apache web service.

![SELinux access](./images/access.svg)

-   Many commands that deal with files use the `-Z` option to display or set SELinux contexts.
-   For instance, `ps`, `ls`, `cp`, and `mkdir` all use the `-Z` option to display or set SELinux contexts.
    ```
    $ px axZ
    LABEL                             PID TTY      STAT   TIME COMMAND
    system_u:system_r:init_t:s0         1 ?        Ss     0:09 /usr/lib/systemd/...
    system_u:system_r:kernel_t:s0       2 ?        S      0:00 [kthreadd]
    system_u:system_r:kernel_t:s0       3 ?        S      0:00 [ksoftirqd/0]
    ...output omitted...
    $ systemctl start httpd
    $ ps -ZC httpd
    LABEL                             PID TTY          TIME CMD
    system_u:system_r:httpd_t:s0     1608 ?        00:00:05 httpd
    system_u:system_r:httpd_t:s0     1609 ?        00:00:00 httpd
    ...output omitted...
    $ ls -Z /home
    drwx------. root    root    system_u:object_r:lost_found_t:s0 lost+found
    drwx------. student student unconfined_u:object_r:user_home_dir_t:s0 student
    drwx------. visitor visitor unconfined_u:object_r:user_home_dir_t:s0 visitor
    $ ls -Z /var/www
    drwxr-xr-x. root root system_u:object_r:httpd_sys_script_exec_t:s0 cgi-bin
    drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 error
    drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 html
    drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 icons
    ```

#### Changing the current SELinux mode

-   The SELinux subsystem provides tools to display and change modes.
-   To determine the current SELinux mode, run the `getenforce` command.
-   To set SELinux to a different mode, use the `setenforce` command :
    ```
    $ getenforce
    Enforcing
    $ setenforce
    usage:  setenforce [ Enforcing | Permissive | 1 | 0 ]
    $ setenforce 0
    $ getenforce
    Permissive
    $ setenforce Enforcing
    $ getenforce
    Enforcing
    ```
-   Alternatively, we can set the SELinux mode at boot time by passing a parameter to the kernel : the kernel argument of `enforcing=0` boots the system into **permissive mode**; a value of `enforcing=1` sets **enforcing mode**.
-   We can also **disable SELinux completely** by passing on the kernel parameter `selinux=0`.
-   A value of `selinux=1` **enables SELinux**.

#### Setting the default SELinux mode

-   We can also configure SELinux persistently using the `/etc/selinux/config` file.
-   In the example below (the default configuration), the configuration file sets SELinux to `enforcing`. The comments also show the other valid values : `permissive` and `disabled`.
    ```
    # This file controls the state of SELinux on the system.
    # SELINUX= can take one of these three values:
    #     enforcing - SELinux security policy is enforced.
    #     permissive - SELinux prints warnings instead of enforcing.
    #     disabled - No SELinux policy is loaded.
    SELINUX=enforcing
    # SELINUXTYPE= can take one of these two values:
    #     targeted - Targeted processes are protected,
    #     minimum - Modification of targeted policy. Only selected processes
    #               are protected.
    #     mls - Multi Level Security protection.
    SELINUXTYPE=targeted
    ```
-   The system reads this file at boot time and configures SELinux as shown. Kernel arguments (selinux=0|1 and enforcing=0|1) override this configuration.

### Controlling SELinux File Contexts

#### Initial SELinux Context

-   On systems running SELinux, all processes and files are labeled.
-   The label represents the security relevant information, known as the **SELinux context**.
    <br>

-   New files typically inherit their SELinux context from the parent directory, thus ensuring that they have the proper context.
    <br>

-   But this inheritance procedure can be undermined in two different ways.
-   First, if we create a file in a different location from the ultimate intended location and then move the file, the file still has the SELinux context of the directory where it was created, not the destination directory.
-   Second, if we copy a file preserving the SELinux context, as with the `cp -a` command, the SELinux context reflects the location of the original file.
    <br>

-   The following example demonstrates inheritance and its pitfalls.
-   Consider these two files created in `/tmp`, one moved to `/var/www/html` and the second one copied to the same directory.
-   Note the SELinux contexts on the files. The file that was moved to the `/var/www/html` directory retains the file context for the `/tmp` directory. The file that was copied to the `/var/www/html` directory inherited SELinux context from the `/var/www/html` directory.
    <br>

-   The `ls -Z` command displays the SELinux context of a file. Note the label of the file.
    ```
    $ ls -Z /var/www/html/index.html
    -rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 /var/www/html/index.html
    ```
-   And the `ls -Zd` command displays the SELinux context of a directory :
    ```
    $ ls -Zd /var/www/html
    drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 /var/www/html/
    ```
-   Note that the `/var/www/html/index.html` has the same label as the parent directory `/var/www/html/`. Now, create files outside of the `/var/www/html` directory and note their file context :
    ```
    $ touch /tmp/file1 /tmp/file2
    $ ls -Z /tmp/file*
    unconfined_u:object_r:user_tmp_t:s0 /tmp/file1
    unconfined_u:object_r:user_tmp_t:s0 /tmp/file2
    ```
-   Move one of these files to the `/var/www/html` directory, copy another, and note the label of each :
    ```
    $ mv /tmp/file1 var/www/html/
    $ cp /tmp/file2 var/www/html/
    $ ls -Z /var/www/html/file
    unconfined_u:object_r:user_tmp_t:s0 /var/www/html/file1
    unconfined_u:object_r:httpd_sys_content_t:s0 /var/www/html/file2
    ```
-   The moved file maintains its original label while the copied file inherits the label from the `/var/www/html` directory. `unconfined_u:` is the user, `object_r:` denotes the role, and `s0` is the level. A sensitivity level of 0 is the lowest possible sensitivity level.

#### Changing the SELinux context of a file

-   Commands to change the SELinux context on files include `semanage fcontext`, `restorecon`, and `chcon`.
    <br>

-   The preferred method to set the SELinux context for a file is to declare the default labeling for a file using the `semanage fcontext` command and then applying that context to the file using the `restorecon` command.
-   This ensures that the labeling will be as desired even after a complete relabeling of the file system.
    <br>

-   The `chcon` command changes SELinux contexts.
-   `chcon` sets the security context on the file, stored in the file system. It is useful for testing and experimenting. However, it does not save context changes in the SELinux context database.
-   When a `restorecon` command runs, changes made by the `chcon` command also do not survive. Also, if the entire file system is relabeled, the SELinux context for files changed using `chcon` are reverted.
    <br>

-   The following screen shows a directory being created. The directory has a type value of `default_t`.
    ```
    $ mkdir /virtual
    $ ls -Zd /virtual
    drwxr-xr-x. root root unconfined_u:object_r:default_t:s0 /virtual
    ```
-   The `chcon` command changes the file context of the `/virtual` directory : the type value changes to `httpd_sys_content_t`.
    ```
    $ chcon -t httpd_sys_content_t /virtual
    $ ls -Zd /virtual
    drwxr-xr-x. root root unconfined_u:object_r:httpd_sys_content_t:s0 /virtual
    ```
-   The `restorecon` command runs and the type value returns to the value of `default_t`. Note the `Relabeled` message.
    ```
    $ restorecon -v /virtual
    Relabeled /virtual from unconfined_u:object_r:httpd_sys_content_t:s0 to unconfined_u:object_r:default_t:s0
    $ ls -Zd /virtual
    drwxr-xr-x. root root unconfined_u:object_r:default_t:s0 /virtual
    ```

#### Defining SELinux Default File Context Rules

-   The `semanage fcontext` command displays and modifies the rules that `restorecon` uses to set default file contexts.
-   It uses extended regular expressions to specify the path and file names.
-   The most common extended regular expression used in `fcontext` rules is `(/.*)?`, which means “optionally, match a / followed by any number of characters”. It matches the directory listed before the expression and everything in that directory recursively.

##### Basic File Context Operations

-   The following table is a reference for `semanage fcontext` options to add, remove or list SELinux file contexts.

| option           | description                                  |
| ---------------- | -------------------------------------------- |
| `-a`, `--add`    | Add a record of the specified object type    |
| `-d`, `--delete` | Delete a record of the specified object type |
| `-l`, `--list`   | List records of the specified object type    |

-   To ensure that we have the tools to manage SELinux contexts, install the `policycoreutils` package and the `policycoreutils-python` package if needed. These contain the `restorecon` command and `semanage` command, respectively.
    <br>

-   To ensure that all files in a directory have the correct file context run the `semanage fcontext -l` followed by the `restorecon` command.
-   In the following example, note the file context of each file before and after the `semanage` and `restorecon` commands run.
    ```
    $ ls -Z /var/www/html/file*
    unconfined_u:object_r:user_tmp_t:s0 /var/www/html/file1  unconfined_u:object_r:httpd_sys_content_t:s0 /var/www/html/file2
    $ semanage fcontext -l
    ...output omitted...
    /var/www(/.*)?       all files    system_u:object_r:httpd_sys_content_t:s0
    ...output omitted...
    $ restorecon -Rv /var/www/
    Relabeled /var/www/html/file1 from unconfined_u:object_r:user_tmp_t:s0 to unconfined_u:object_r:httpd_sys_content_t:s0
    [root@host ~]# ls -Z /var/www/html/file*
    unconfined_u:object_r:httpd_sys_content_t:s0 /var/www/html/file1  unconfined_u:object_r:httpd_sys_content_t:s0 /var/www/html/file2
    ```
-   The following example shows how to use `semanage` to add a context for a new directory.

    ```
    $  mkdir /virtual
    $ touch /virtual/index.html
    $ ls -Zd /virtual
    drwxr-xr-x. root root unconfined_u:object_r:default_t:s0 /virtual/

    $ semanage fcontext -a -t httpd_sys_content_t '/virtual/(/.*/)?'
    $ restorecon -RFvv /virtual;
    $ ls -Zd /virtual/
    drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 /virtual/
    $ ls -Z /virtual/
    -rw-r--r--. root root system_u:object_r:httpd_sys_content_t:s0 index.html
    ```

### Adjusting SELinux Policy with Booleans

#### SELinux booleans

-   SELinux booleans are switches that change the behavior of the SELinux policy.
-   SELinux booleans are rules that can be enabled or disabled.
-   They can be used by security administrators to tune the policy to make selective adjustments.
    <br>

-   The SELinux man pages, provided with the selinux-policy-doc package, describe the purpose of the available booleans.
-   The `man -k '_selinux'` command lists these man pages.

-   Commands useful for managing SELinux booleans include `getsebool`, which lists booleans and their state, and `setsebool` which modifies booleans.
-   `setsebool -P` modifies the SELinux policy to make the modification persistent.
-   And `semanage boolean -l` reports on whether or not a boolean is persistent, along with a short description of the boolean.
    <br>

-   Non-privileged users can run the `getsebool` command, but we must be a superuser to run `semanage boolean -l` and `setsebool -P`.

    ```
    $ getsebool -a
    abrt_anon_write --> off
    abrt_handle_event --> off
    abrt_upload_watch_anon_write --> on
    antivirus_can_scan_system --> off
    antivirus_use_jit --> off
    ...output omitted...
    $ getsebool httpd_enable_homedirs
    httpd_enable_homedirs --> off

    $ setsebool httpd_enable_homedirs on
    Could not change active booleans. Please try as root: Permission denied
    $ sudo setsebool httpd_enable_homedirs on
    $ sudo semanage boolean -l | grep httpd_enable_homedirs
    httpd_enable_homedirs          (on   ,  off)  Allow httpd to enable homedirs
    $ getsebool httpd_enable_homedirs
    httpd_enable_homedirs --> on

    # The -P option writes all pending values to the policy, making them persistent across reboots.
    $ setsebool -P httpd_enable_homedirs on
    $ sudo semanage boolean -l | grep httpd_enable_homedirs
    httpd_enable_homedirs          (on   ,   on)  Allow httpd to enable homedirs
    ```

-   To list booleans in which the current state differs from the default state, run `$ semanage boolean -l -C`.

    ```
    $ sudo semanage boolean -l -c
    SELinux boolean                State  Default Description

    cron_can_relabel               (off   ,   on)  Allow cron to can relabel
    ```

### Investigating and Resolving SELinux Issues

#### Troubleshooting SELinux Issues

-   It is important to understand what actions we must take when SELinux prevents access to files on a server that we know should be accessible. Use the following steps as a guide to troubleshooting these issues:

    1. Before thinking of making any adjustments, consider that SELinux may be doing its job correctly by prohibiting the attempted access. If a web server tries to access files in `/home`, this could signal a compromise of the service if web content is not published by users. If access should have been granted, then additional steps need to be taken to solve the problem.

    2. The most common SELinux issue is an incorrect file context. This can occur when a file is created in a location with one file context and moved into a place where a different context is expected. In most cases, running `restorecon` will correct the issue. Correcting issues in this way has a very narrow impact on the security of the rest of the system.

    3. Another remedy for overly restrictive access could be the adjustment of a Boolean. For example, the `ftpd_anon_write` boolean controls whether anonymous FTP users can upload files. We must turn this boolean on to permit anonymous FTP users to upload files to a server. Adjusting booleans requires more care because they can have a broad impact on system security.

    4. It is possible that the SELinux policy has a bug that prevents a legitimate access. Since SELinux has matured, this is a rare occurrence. When it is clear that a policy bug has been identified, contact Red Hat support to report the bug so it can be resolved.

#### Monitoring SELinux Violations

-   Install the **setroubleshoot-server package** to send SELinux messages to `/var/log/messages`.
-   **setroubleshoot-server** listens for audit messages in `/var/log/audit/audit.log` and sends a short summary to `/var/log/messages`.
-   This summary includes unique identifiers (UUID) for SELinux violations that can be used to gather further information.
-   The `sealert -l UUID` command is used to produce a report for a specific incident.
-   Use `sealert -a /var/log/audit/audit.log` to produce reports for all incidents in that file.
    <br>

-   Consider the following sample sequence of commands on a standard Apache web server :
    ```
    $ touch /root/file3
    $ mv /root/file3 var/www/html
    $ systemctl start httpd
    $ curl http://localhost/file3
    <!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
    <html><head>
    <title>403 Forbidden</title>
    </head><body>
    <h1>Forbidden</h1>
    <p>You don't have permission to access /file3
    on this server.</p>
    </body></html>
    ```
-   We expect the web server to deliver the contents of file3 but instead it returns a permission denied error. Inspecting both `/var/log/audit/audit.log` and `/var/log/messages` reveals extra information about this error.
    ```
    $ tail /var/log/audit/audit.log
    ...output omitted...
    type=AVC msg=audit(1392944135.482:429): avc:  denied  { getattr } for
    pid=1609 comm="httpd" path="/var/www/html/file3" dev="vda1" ino=8980981
    scontext=system_u:system_r:httpd_t:s0
    tcontext=unconfined_u:object_r:admin_home_t:s0 tclass=file
    ...output omitted...
    $ tail /var/log/messages
    ...output omitted...
    Feb 20 19:55:42 host setroubleshoot: SELinux is preventing /usr/sbin/httpd
    from getattr access on the file . For complete SELinux messages. run
    sealert -l 613ca624-248d-48a2-a7d9-d28f5bbe2763
    ```
-   Both log files indicate that an SELinux denial is the culprit.
-   The `sealert` command that is part of the output in `/var/log/messages` provides extra information, including a possible fix.

    ```
    $ sealert -l 613ca624-248d-48a2-a7d9-d28f5bbe2763
    SELinux is preventing /usr/sbin/httpd from getattr access on the file .

    *****  Plugin catchall (100. confidence) suggests   **************************

    If you believe that httpd should be allowed getattr access on the
    file by default.
    Then you should report this as a bug.
    You can generate a local policy module to allow this access.
    Do
    allow this access for now by executing:
    # grep httpd /var/log/audit/audit.log | audit2allow -M mypol
    # semodule -i mypol.pp


    Additional Information:
    Source Context                system_u:system_r:httpd_t:s0
    Target Context                unconfined_u:object_r:admin_home_t:s0
    Target Objects                 [ file ]
    Source                        httpd
    Source Path                   /usr/sbin/httpd
    Port                          <Unknown>
    Host                          servera
    Source RPM Packages           httpd-2.4.6-14.el7.x86_64
    Target RPM Packages
    Policy RPM                    selinux-policy-3.12.1-124.el7.noarch
    Selinux Enabled               True
    Policy Type                   targeted
    Enforcing Mode                Enforcing
    Host Name                     servera
    Platform                      Linux servera 3.10.0-84.el7.x86_64 #1
                                SMP Tue Feb 4 16:28:19 EST 2014 x86_64 x86_64
    Alert Count                   2
    First Seen                    2014-02-20 19:55:35 EST
    Last Seen                     2014-02-20 19:55:35 EST
    Local ID                      613ca624-248d-48a2-a7d9-d28f5bbe2763

    Raw Audit Messages
    type=AVC msg=audit(1392944135.482:429): avc:  denied  { getattr } for
    pid=1609 comm="httpd" path="/var/www/html/file3" dev="vda1" ino=8980981
    scontext=system_u:system_r:httpd_t:s0
    tcontext=unconfined_u:object_r:admin_home_t:s0 tclass=file

    type=SYSCALL msg=audit(1392944135.482:429): arch=x86_64 syscall=lstat
    success=no exit=EACCES a0=7f9fed0edea8 a1=7fff7bffc770 a2=7fff7bffc770
    a3=0 items=0 ppid=1608 pid=1609 auid=4294967295 uid=48 gid=48 euid=48
    suid=48 fsuid=48 egid=48 sgid=48 fsgid=48 tty=(none) ses=4294967295
    comm=httpd exe=/usr/sbin/httpd subj=system_u:system_r:httpd_t:s0 key=(null)

    Hash: httpd,httpd_t,admin_home_t,file,getattr
    ```

-   **Note**

    -   The Raw Audit Messages section reveals the target file that is the problem, `/var/www/html/file3`. Also, the target context, `tcontext`, does not look like it belongs with a web server.
    -   Use the `$ restorecon /var/www/html/file3` command to fix the file context.
    -   If there are other files that need to be adjusted, `restorecon` can recursively reset the context : `$restorecon -R /var/www/`.

-   The Raw Audit Messages section of the `sealert` command contains information from `/var/log/audit.log`. To search the `/var/log/audit.log` file use the `ausearch` command. The `-m` searches on the message type. The `-ts` option searches based on time.
    ```
    $ ausearch -m AVC -ts recent
    ----
    time->Tue Apr  9 13:13:07 2019
    type=PROCTITLE msg=audit(1554808387.778:4002): proctitle=2F7573722F7362696E2F6874747064002D44464F524547524F554E44
    type=SYSCALL msg=audit(1554808387.778:4002): arch=c000003e syscall=49 success=no exit=-13 a0=3 a1=55620b8c9280 a2=10 a3=7ffed967661c items=0 ppid=1 pid=9340 auid=4294967295 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0 sgid=0 fsgid=0 tty=(none) ses=4294967295 comm="httpd" exe="/usr/sbin/httpd" subj=system_u:system_r:httpd_t:s0 key=(null)
    type=AVC msg=audit(1554808387.778:4002): avc:  denied  { name_bind } for  pid=9340 comm="httpd" src=82 scontext=system_u:system_r:httpd_t:s0 tcontext=system_u:object_r:reserved_port_t:s0 tclass=tcp_socket permissive=0
    ```
