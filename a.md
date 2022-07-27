## Controlling the Boot Process

### Selecting the Boot Target

#### Describing the Red Hat Enterprise Linux 8 Boot Process

-   Modern computer systems are complex combinations of hardware and software.
-   Starting from an undefined, powered-down state to a running system with a login prompt requires a large number of pieces of hardware and software to work together.
-   The following list gives a high-level overview of the tasks involved for a physical `x86_64` system booting Red Hat Enterprise Linux 8. The list for `x86_64` virtual machines is roughly the same, but the hypervisor handles some of the hardware-specific steps in software.

    -   The machine is powered on. The system firmware, either modern **UEFI** or older **BIOS**, runs a **Power On Self Test (POST)** and starts to initialize some of the hardware.
        <br>
        Configured using the system BIOS or UEFI configuration screens that we typically reach by pressing a specific key combination, such as `F2`, early during the boot process.
        <br>
    -   The system firmware searches for a bootable device, either configured in the UEFI boot firmware or by searching for a **Master Boot Record (MBR)** on all disks, in the order configured in the BIOS.
        <br>
        Configured using the system BIOS or UEFI configuration screens that we typically reach by pressing a specific key combination, such as `F2`, early during the boot process.
        <br>
    -   The system firmware reads a boot loader from disk and then passes control of the system to the boot loader. On a Red Hat Enterprise Linux 8 system, the boot loader is the **GRand Unified Bootloader version 2 (GRUB2)**.
        <br>
        Configured using the `grub2-install` command, which installs GRUB2 as the boot loader on the disk.
        <br>
    -   GRUB2 loads its configuration from the `/boot/grub2/grub.cfg` file and displays a menu where we can select which kernel to boot.
        <br>
    -   Configured using the `/etc/grub.d/` directory, the `/etc/default/grub` file, and the `$ grub2-mkconfig` command to generate the `/boot/grub2/grub.cfg` file.
        <br>
    -   After we select a kernel, or the timeout expires, the boot loader loads the kernel and initramfs from disk and places them in memory. An **initramfs** is an archive containing the kernel modules for all the hardware required at boot, initialization scripts, and more. On Red Hat Enterprise Linux 8, the initramfs contains an entire usable system by itself.
        <br>
        Configured using the `/etc/dracut.conf.d/` directory, the `dracut` command, and the `lsinitrd` command to inspect the `initramfs` file.
        <br>
    -   The boot loader hands control over to the kernel, passing in any options specified on the kernel command line in the boot loader, and the location of the `initramfs` in memory.
        <br>
        Configured using the `/etc/grub.d/` directory, the `/etc/default/grub` file, and the `$ grub2-mkconfig` command to generate the `/boot/grub2/grub.cfg` file.
        <br>
    -   The kernel initializes all hardware for which it can find a driver in the `initramfs`, then executes `/sbin/init` from the initramfs as PID 1. On Red Hat Enterprise Linux 8, `/sbin/init` is a link to `systemd`.
        <br>
        Configured using the `init=` command-line parameter.
        <br>
    -   The `systemd` instance from the `initramfs` executes all units for the `initrd.target` target. This includes mounting the root file system on disk on to the `/sysroot` directory.
        <br>
        Configured using `/etc/fstab`
        <br>
    -   The kernel switches (pivots) the root file system from `initramfs` to the root file system in `/sysroot`. `systemd` then re-executes itself using the copy of `systemd` installed on the disk.
        <br>
    -   `systemd` looks for a default target, either passed in from the kernel command line or configured on the system, then starts (and stops) units to comply with the configuration for that target, solving dependencies between units automatically. In essence, a `systemd` target is a set of units that the system should activate to reach the desired state. These targets typically start a text-based login or a graphical login screen.
        <br>
        Configured using `/etc/systemd/system/default.target` and `/etc/systemd/system/`.

#### Rebooting and Shutting Down

-   To power off or reboot a running system from the command line, we can use the `systemctl` command.
    <br>

-   `$ systemctl poweroff` stops all running services, unmounts all file systems (or remounts them read-only when they cannot be unmounted), and then powers down the system.
    <br>

-   `$ systemctl reboot` stops all running services, unmounts all file systems, and then reboots the system.
    <br>

-   We can also use the shorter version of these commands, `$ poweroff` and `$ reboot`, which are symbolic links to their `systemctl` equivalents.
    <br>

-   **Note**
    -   `$ systemctl halt` and `$ halt` are also available to stop the system, but unlike `$ poweroff`, these commands do not power off the system; they bring a system down to a point where it is safe to power it off manually.

##### Selecting a Systemd Target

-   A `systemd` target is a set of `systemd` units that the system should start to reach a desired state. The following table lists the most important targets.

    | Target              | Purpose                                                                                   |
    | ------------------- | ----------------------------------------------------------------------------------------- |
    | `graphical.target`  | System supports multiple users, graphical- and text-based logins.                         |
    | `multi-user.target` | System supports multiple users, text-based logins only.                                   |
    | `rescue.target`     | **sulogin** prompt, basic system initialization completed.                                |
    | `emergency.target`  | **sulogin** prompt, `initramfs` pivot complete, and system root mounted on `/` read only. |

-   A target can be a part of another target. For example, the `graphical.target` includes `multi-user.target`, which in turn depends on `basic.target` and others.
-   We can view these dependencies with the following command.
    ```
    $ systemctl list-dependencies graphical.target | grep target
    graphical.target
    * └─multi-user.target
    *   ├─basic.target
    *   │ ├─paths.target
    *   │ ├─slices.target
    *   │ ├─sockets.target
    *   │ ├─sysinit.target
    *   │ │ ├─cryptsetup.target
    *   │ │ ├─local-fs.target
    *   │ │ └─swap.target
    ...output omitted..
    ```
-   To list the available targets, use the following command.
    ```
    $ systemctl list-units --type=target --all
    UNIT                      LOAD      ACTIVE   SUB    DESCRIPTION
    ---------------------------------------------------------------------------
    basic.target              loaded    active   active Basic System
    cryptsetup.target         loaded    active   active Local Encrypted Volumes
    emergency.target          loaded    inactive dead   Emergency Mode
    getty-pre.target          loaded    inactive dead   Login Prompts (Pre)
    getty.target              loaded    active   active Login Prompts
    graphical.target          loaded    inactive dead   Graphical Interface
    ...output omitted...
    ```

###### Selecting Target at Runtime

-   On a running system, administrators can switch to a different target using the `systemctl isolate` command. <br> `systemctl isolate multi-user.target`
    <br>
    Isolating a target stops all services not required by that target (and its dependencies), and starts any required services not yet started.

-   Not all targets can be isolated. We can only isolate targets that have `AllowIsolate=yes` set in their unit files. For example, we can isolate the graphical target, but not the cryptsetup target.

    ```
    $ systemctl cat graphical.target
    # /usr/lib/systemd/system/graphical.target
    ...output omitted...
    [Unit]
    Description=Graphical Interface
    Documentation=man:systemd.special(7)
    Requires=multi-user.target
    Wants=display-manager.service
    Conflicts=rescue.service rescue.target
    After=multi-user.target rescue.service rescue.target display-manager.service
    AllowIsolate=yes

    $ systemctl cat cryptsetup.target
    # /usr/lib/systemd/system/cryptsetup.target
    ...output omitted...
    [Unit]
    Description=Local Encrypted Volumes
    Documentation=man:systemd.special(7)
    ```

-   When the system starts, `systemd` activates the `default.target` target.
-   Normally the default target in `/etc/systemd/system/` is a symbolic link to either `graphical.target` or `multi-user.target`.
-   Instead of editing this symbolic link by hand, the `systemctl` command provides two subcommands to manage this link : `get-default` and `set-default`.
    ```
    $ systemctl get-default
    multi-user.target
    $ systemctl set-default graphical.target
    Removed /etc/systemd/system/default.target.
    Created symlink /etc/systemd/system/default.target -> /usr/lib/systemd/system/graphical.target.
    $ systemctl get-default
    graphical.target
    ```

###### Selecting a different Target at Boot Time

-   To select a different target at boot time, append the `systemd.unit=target.target` option to the kernel command line from the boot loader.
    <br>

-   For example, to boot the system into a rescue shell where we can change the system configuration with almost no services running, append the following option to the kernel command line from the boot loader.
    ```
    systemd.unit=rescue.target
    ```
    This configuration change only affects a single boot, making it a useful tool for troubleshooting the boot process.
    <br>
    To use this method of selecting a different target, use the following procedure :
    -   Boot or reboot the system.
    -   Interrupt the boot loader menu countdown by pressing any key (except Enter which would initiate a normal boot).
    -   Move the cursor to the kernel entry that we want to start.
    -   Press `e` to edit the current entry.
    -   Move the cursor to the line that starts with linux. This is the kernel command line.
    -   Append `systemd.unit=target.target`. For example, `systemd.unit=emergency.target`.
    -   Press `Ctrl+x` to boot with these changes.
