## Mounting Network-Attached Storage with NFS

### Mounting and Unmounting NFS Shares

-   NFS, the **Network File System**, is an internet standard protocol used by Linux, UNIX, and similar operating systems as their native network file system.
-   It is an open standard, still being actively enhanced, which supports native Linux permissions and file-system features.
    <br>

-   The default NFS version in Red Hat Enterprise Linux 8 is 4.2.
-   NFSv4 and NFSv3 major versions are supported.
-   NFSv2 is no longer supported.
-   NFSv4 uses only the TCP protocol to communicate with the server; earlier NFS versions could use either TCP or UDP.
    <br>

-   NFS servers export shares (directories).
-   NFS clients mount an exported share to a local mount point (directory), which must exist.
-   NFS shares can be mounted a number of ways :

    -   Manually, using the `mount` command.

    -   Automatically at boot time using `/etc/fstab` entries.
    -   On demand, using either the `autofs` service or the `systemd.automount` facility.

#### Mounting NFS Shares

-   To mount an NFS share, follow these three steps :

    1. **Identify :** The administrator of the NFS client system can identify available NFS shares in various ways :

        - The administrator for the NFS server may provide export details, including security requirements.

        - Alternatively, the client administrator can identify NFSv4 shares by mounting the root directory of the NFS server and exploring the exported directories.
          Do this as the root user. Access to shares that use Kerberos security will be denied, but the share (directory) name will be visible. Other shared directories will be browsable.

        ```
        $ sudo mkdir /mountpoint
        $ sudo mount serverb:/ /mountpoint
        $ sudo ls mountpoint
        ```

    2. **Mount point :** Use mkdir to create a mount point in a suitable location. <br> `$ mkdir -p mountpoint`
    3. **Mount :** As with file systems on partitions, NFS shares must be mounted to be available. To mount an NFS share, select from the following. In each case, we must run these commands as a superuser either by logging in as root or by using the `sudo` command.

        - **Mount temporarily :** Mount the NFS share using the mount command : <br> `$ sudo mount -t nfs -o rw,sync serverb:/share mountpoint`
          <br>
          The `-t` nfs option is the file-system type for NFS shares (not strictly required but shown for completeness).
          The `-o` sync option tells mount to immediately synchronize write operations with the NFS server (the default is asynchronous).
          <br>
          This command mounts the share immediately but not persistently; the next time the system boots, this NFS share will not be available.
          This is useful for one-time access to data.
          It is also useful for test mounting an NFS share before making the share available persistently.

        - **Mount persistently :** To ensure that the NFS share is mounted at boot time, edit the `/etc/fstab` file to add the mount entry.
            ```
            $ sudo vim /etc/fstab
            ...
            serverb:/share /mountpoint nfs rw,soft 0 0
            ```
            Then, mount the NFS share :<br> `$ sudo mount /mountpoint`

#### Unmounting NFS Shares

-   As root (or using sudo), unmount an NFS share using the umount command.<br> `$ sudo umount /mountpoint`
    <br>
-   **Note**
    -   Unmounting a share does not remove its `/etc/fstab` entry.
    -   Unless we remove or comment out the entry, the NFS share will be remounted either at the next system boot or when the NFS client service is restarted.

### Automounting Network Attached Storage

#### Mounting NFS Shares with the Automounter

-   The automounter is a service (`autofs`) that automatically mounts NFS shares "on-demand," and will automatically unmount NFS shares when they are no longer being used.

##### Automounter Benefits

-   Users do not need to have root privileges to run the `mount` and `umount` commands.

-   NFS shares configured in the automounter are available to all users on the machine, subject to access permissions.

-   NFS shares are not permanently connected like entries in `/etc/fstab`, freeing network and system resources.

-   The automounter is configured on the client side; no server-side configuration is required.

-   The automounter uses the same options as the `mount` command, including security options.

-   The automounter supports both direct and indirect mount-point mapping, for flexibility in mount-point locations.

-   `autofs` creates and removes indirect mount points, eliminating manual management.

-   NFS is the default automounter network file system, but other network file systems can be automatically mounted.

-   `autofs` is a service that is managed like other system services.

##### Create and Automount

-   Configuring an automount is a multiple step process :

    1. Install the `autofs` package.<br> `$ yum install autofs`<br>This package contains everything needed to use the automounter for NFS shares.
    2. Add a master map file to `/etc/auto.master.d`. This file identifies the base directory used for mount points and identifies the mapping file used for creating the automounts.<br> `$ vim /etc/auto.master.d/demo.autofs`
       <br>
       The name of the master map file is arbitrary (although typically meaningful), but it must have an extension of `.autofs` for the subsystem to recognize it.
       We can place multiple entries in a single master map file; alternatively, we can create multiple master map files each with its own entries grouped logically.
       <br>
       Add the master map entry, in this case, for indirectly mapped mounts : <br> `/shares /etc/auto.demo`
       This entry uses the `/shares` directory as the base for indirect automounts.
       The `/etc/auto.demo` file contains the mount details.
       Use an absolute file name.
       The `auto.demo` file needs to be created before starting the `autofs` service.
    3. Create the mapping files. Each mapping file identifies the mount point, mount options, and source location to mount for a set of automounts.<br> `$ sudo vim /etc/auto.demo`
       <br>
       The mapping file-naming convention is `/etc/auto.name`, where `name` reflects the content of the map.<br> `work -rw,sync serverb:/shares/work`
       <br>
       The format of an entry is mount point, mount options, and source location. This example shows a basic indirect mapping entry.
       Direct maps and indirect maps using wildcards are covered later in this section.
       <br>
       Known as the key in the man pages, the mount point is created and removed automatically by the `autofs` service.
       In this case, the fully qualified mount point is `/shares/work` (see the master map file).
       The `/shares` directory and the `/shares/work` directories are created and removed as needed by the `autofs` service.
       <br>
       In this example, the local mount point mirrors the server's directory structure, however this is not required; the local mount point can be named anything.
       The `autofs` service does not enforce a specific naming structure on the client.
       <br>
       Mount options start with a dash character (`-`) and are comma-separated with no white space.
       Mount options available to a manual mounting of a file system are available when automounting.
       In this example, the automounter mounts the share with read/write access (`rw` option), and the server is synchronized immediately during write operations (`sync` option).
       <br>
       Useful automounter-specific options include `-fstype=` and `-strict`.
       Use `fstype` to specify the file system type, for example, `nfs4` or `xfs`, and use `strict` to treat errors when mounting file systems as fatal.
       <br>
       The source location for NFS shares follows the `host:/pathname` pattern; in this example, `serverb:/shares/work`.
       For this automount to succeed, the NFS server, serverb, must export the directory with read/write access and the user requesting access must have standard Linux file permissions on the directory. If serverb exports the directory with read/only access, then the client will get read/only access even though it requested read/write access.

    4. Start and enable the automounter service.<br> `$ systemctl enable --now autofs`

##### Direct Maps

-   Direct maps are used to map an NFS share to an existing absolute path mount point.
    <br>

-   To use directly mapped mount points, the master map file might appear as follows : <br> `/- /etc/auto.direct`
    All direct map entries use `/-` as the base directory. In this case, the mapping file that contains the mount details is `/etc/auto.direct`.
    <br>

-   The content for the `/etc/auto.direct` file might appear as follows :<br> `/mnt/docs -rw,sync serverb:/shares/docs`
    The mount point (or key) is always an absolute path. The rest of the mapping file uses the same structure.
-   In this example the `/mnt` directory exists, and it is not managed by `autofs`. The full directory `/mnt/docs` will be created and removed automatically by the `autofs` service.

##### Indirect Wildcard Maps

-   When an NFS server exports multiple subdirectories within a directory, then the automounter can be configured to access any one of those subdirectories using a single mapping entry.
    <br>

-   Continuing the previous example, if `serverb:/shares` exports two or more subdirectories and they are accessible using the same mount options, then the content for the `/etc/auto.demo` file might appear as follows :<br>`* -rw,sync serverb:/shares/&`<br>
    The mount point (or key) is an asterisk character (`*`), and the subdirectory on the source location is an ampersand character (`&`). Everything else in the entry is the same.
    <br>

-   When a user attempts to access `/shares/work`, the key `*` (which is work in this example) replaces the ampersand in the source location and `serverb:/shares/work` is mounted. As with the indirect example, the work directory is created and removed automatically by `autofs`.
