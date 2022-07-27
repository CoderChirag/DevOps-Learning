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
