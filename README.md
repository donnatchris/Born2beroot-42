##### [🇫🇷 Version française](README.fr.md) / [🇬🇧 English version](README.md)

# BORN2BEROOT PROJECT FROM 42
By chdonnat (Christophe Donnat from 42 Perpignan, France)

There is quite a lot of documentation available online for this project. Therefore, I decided to create a guide with the following specifics:
- A guide in English and in French
- That explains the fundamental concepts
- And that addresses the specifics of the project for the ARM 64 architecture (in my case, on a MacBook with an M1 chip)

## AIM OF THE PROJECT
The goal of this project is to create a virtual machine using VirtualBox (or UTM) in order to install a Debian server while adhering to a set of specific rules related to the topic.

## BONUS
Bonus list:
- Properly set up partitions to achieve a structure similar to the example provided in the project description.
- Set up a functional WordPress website with services such as lighttpd, MariaDB, and PHP.
- Set up a service that you find useful (NGINX/Apache2 excluded!). During the defense, you will need to justify your choice.

**Note:** For this project, I only completed the first bonus.

# GUIDE FOR THE BORN TO BE ROOT PROJECT ON A MACBOOK

If you are trying to complete the Born to be root project on a Macbook (or any device with an Apple Silicon chip like M1, M2, etc.) by following the tutorials available online, you will realize that you cannot apply their instructions as they are.

After spending a few days trying to understand what was preventing me from blindly following these tutorials, I have gathered some very useful knowledge that I would like to share with you.

Since the specifics imposed by the M1 chips are not very numerous, this guide, and especially the information it contains, may be useful to you even if you are doing the project on a Windows computer or another machine.

The purpose of this guide is not so much to give you a precise procedure to follow, but to provide you with the knowledge that will allow you to carry out the project on your own by explaining:

- the steps to complete the project
- the particularities due to Macbooks and similar devices
- the necessary knowledge to complete the project
- the linux commands used in the project
- This guide was originally written in French.

<aside>

### Disclaimer

I am myself at the beginning of my curriculum at 42, I may have made mistakes or inaccuracies, do not hesitate to contact me if necessary so that I can correct what needs to be corrected.

</aside>

> **Let's give credit where credit is due...**
This guide was not written from scratch. It is a French version, a development (sprinkled with many explanations and knowledge gleaned from here and there) and an adaptation to the few specifics imposed by the Apple Silicon chips of the guide by **bepoisson (guide** itself based on the guide by **mcombeau).**
> 

### What is a VM for?

The benefits of a virtual machine (VM) are numerous and offer several advantages, including:

1.  **Isolation**: Virtual machines are isolated from each other, which allows for the separation of different applications or operating systems on the same physical machine. This prevents conflicts and security risks between environments.
2.  **Optimized resource utilization**: A physical machine can run multiple VMs at the same time, which allows for full utilization of hardware resources, such as the processor, memory, and storage, and optimizes server usage.
3.  **Flexibility and portability**: VMs are easily transferable from one host to another. For example, a VM can be copied, moved, or backed up, which facilitates system management and migration.
4.  **Security**: The isolation of VMs helps to reduce the impact of vulnerabilities. If a VM is compromised, it generally does not affect other machines or the physical host.
5.  **Testing and development**: VMs allow for the creation of test and development environments that are independent of the main system. It is easy to test applications, updates, or configurations without risking damage to the production environment.
6.  **Snapshots and restoration**: VMs can be "snapshotted," allowing the state of a machine to be saved at a given moment. In case of a problem, it is possible to quickly restore the previous state of the VM.
7.  **Server consolidation**: By consolidating multiple servers on a single physical machine, a company can reduce costs related to purchasing and managing additional hardware.
8.  **Simplified management**: VMs can be managed and automated using virtualization tools, facilitating the creation, deployment, and management of systems.

In summary, virtual machines offer great flexibility, better resource management, and increased isolation, while facilitating backup, testing, and deployment processes.

# Install Virtualbox and Debian

## An Architecture Problem

**Architecture** in computing refers to the structure and design of a system, whether it is hardware (like a processor) or software. It defines how components (CPU, memory, bus, etc.) interact and the instructions that the device can understand.

**AMD64:** The dominant architecture for modern Windows PCs is the **x86-64** architecture (also called **AMD 64**). It supports 64-bit applications, while remaining compatible with older 32-bit (x86) applications.

**ARM64**: The **ARM64** (or **AArch64**) architecture is a 64-bit processor architecture developed by ARM. It is commonly used in smartphones, tablets, IoT devices, and more recently in computers (like Macs with M1/M2 chips).

Some ARM64 processors include a mode called AArch32 that allows running 32-bit instructions for applications designed for the ARM architecture (ARMv7 for example), but **Apple Silicon** (M1, M2) processors, based on ARM64, do **not support 32-bit ARM instructions (AArch32)**, marking a transition to a purely 64-bit environment.
Most popular software has a version for each architecture, but this is not always the case.

## VirtualBox

Downloading and installing VirtualBox is the first step. It is what is called a **hypervisor**:

**Hypervisor:** The function of hypervisors is to **create and manage virtual machines** by allowing multiple operating systems to run simultaneously on a single physical machine, isolating their resources.

**UTM**: The Mac environment has a dedicated hypervisor: **UTM.** However, I decided to use VirtualBox anyway to stick as closely as possible to the tools used by the majority of students.

<aside>

On the website https://www.virtualbox.org/wiki/Downloads download the [**macOS / Apple Silicon hosts**](https://download.virtualbox.org/virtualbox/7.1.4/VirtualBox-7.1.4-165100-macOSArm64.dmg) version and then install it on your machine.

</aside>

## Debian

Downloading the Debian ISO image is the next step.

**Debian** is a very popular **Linux distribution** (or "distro"), known for its stability, flexibility, and wide range of software. It is one of the oldest distributions and serves as a base for many other distributions, including **Ubuntu**, **Linux Mint**, and **Raspbian**.

### **What are the differences between Debian and Rocky?**

Debian and Rocky Linux are two Linux distributions with different goals. **Debian** is community-driven, versatile, and focused on software freedom, with broad architecture support and a vast choice of software. It is suitable for servers, workstations, and personal projects. **Rocky Linux**, on the other hand, is enterprise-oriented, designed as a free and stable alternative to Red Hat Enterprise Linux (RHEL). It focuses on professional environments with long-term support and RHEL compatibility. Debian is ideal for flexible projects, while Rocky is perfect for critical enterprise systems.

**RHEL** (Red Hat Enterprise Linux) is a commercial Linux distribution developed by **Red Hat**, designed for businesses and professional environments. It is renowned for its stability, technical support, and long-term security updates, making it ideal for critical systems like servers, databases, or business applications.

### Linux kernel and Linux distribution:

**Linux Kernel**: This is the central part of the operating system that manages the interactions between hardware and software. It can run on its own, but it does not constitute a complete usable system.

**Linux Distribution**: A distribution like Debian bundles the Linux kernel with all the necessary tools (package management, applications, graphical interface) to make the computer functional and usable by the user.

**An ISO image** (or **ISO file**) is an exact copy of an **optical disc** (like a CD, DVD, or Blu-ray) in the form of a single computer file. The term **ISO** comes from the standard format of the International Organization for Standardization (ISO 9660), which is used to organize files on optical discs. It can be mounted directly on a computer or burned to a physical disc. An ISO image allows for the convenient distribution of operating systems, software, or backups without needing a physical medium.

**CD or DVD?** The main difference between a **CD ISO image** and a **DVD ISO image** of **Debian** lies in the amount of software included. The **CD ISO** contains a minimal version of the Debian system, with only the essential components to start the installation, while the **DVD ISO** includes a more complete installation, with several desktop environments and an extensive range of software. In summary, the CD ISO is ideal for a basic installation requiring an internet connection for additional packages, while the DVD ISO allows for a complete installation without an internet connection. For our project, the CD image is sufficient as long as we have an internet connection to download additional packages.

<aside>

On the Debian website https://www.debian.org/ you should not download the file from the homepage (as it is an ISO image for the AMD 64 architecture); you must go to *Other downloads* then *Download from mirrors* and select a mirror located near you (in France for me), then choose an arm64 ISO image of the latest stable version of Debian (Debian 12.8.0 at the time of writing). The cd version is sufficient, like the one you will find on the last line of this page: https://debian.obspm.fr/debian-cd/current/arm64/iso-cd/

</aside>

## Bios and UEFI

**BIOS** (Basic Input/Output System) and **UEFI** (Unified Extensible Firmware Interface) are two types of **firmware** (software integrated into computer hardware) that are responsible for starting and initializing the operating system on a computer. Although they perform a similar function, there are important differences between them.

-   **BIOS** is an older firmware used to start a computer. The BIOS uses the **MBR** to manage the partition table of a hard drive. It has a text interface and is slower. It is still present on many systems today, but it is limited by its age and capabilities.
-   **UEFI** is a modern firmware that replaces the BIOS. UEFI uses the **GPT** (GUID Partition Table) to manage the partitions of a disk. UEFI allows for a graphical interface and a faster startup, and includes advanced security features like **Secure Boot**. UEFI is compatible with modern systems such as those based on **ARM processors**, **MacBooks with M1 processors**, and **64-bit** architectures.

**M1 Macbooks** and later only support UEFI, unlike modern Windows PCs which are compatible with both BIOS and UEFI.

When you create a virtual machine (VM) on an M1 MacBook, even for a Linux distribution like **Debian ARM64**, it is necessary to check the **"Enable EFI"** option in the VM settings. This allows simulating the startup of a **UEFI** compatible system, which is essential for the VM to function correctly on the ARM architecture of the M1 MacBook.

Without the **"Enable EFI"** option, you might encounter startup problems, as the **BIOS** configuration (the traditional startup mode) is not compatible with the M1 MacBook's architecture.

## Partitioning

**Partitioning** consists of dividing a hard drive or SSD into several distinct **partitions**, each acting as a virtual disk. This allows for organizing data, installing multiple operating systems, or better managing storage space by separating system files, personal data, and backups, for example. Each partition can be formatted with a different file system and have its own partition table (like MBR or GPT).

**A partition table** is a data structure used to define and organize the partitions of a hard drive or SSD. It contains information on how a disk is divided into sections called **partitions**, and each partition can be used for a file system or another type of storage. There are mainly two types of **partitioning tables** used in computer systems:

-   **MBR (Master Boot Record)** is an **old format** used since the 1980s. It is limited to **4 primary partitions** (or 3 primary + 1 extended). It supports **disks up to 2 TB maximum.** It is used mainly with the **BIOS** to start the system. MBR is primarily associated with BIOS for historical reasons (although it can sometimes be used with UEFI).
-   **GPT (GUID Partition Table)** is a **modern format** that replaces MBR. It supports **an unlimited number of partitions** (usually up to 128). It supports **disks larger than 2 TB.** It is used with **UEFI**, offering advanced features like **Secure Boot.** It is more reliable and secure, especially with **redundancy checks**.

### Partition, Encryption and Logical Partition:

**A partition** is a logical division of a hard drive or SSD into several **segments**. Each partition can be treated as a separate disk, with its own file system (like NTFS, ext4, etc.).

**Encryption** is a method of securing data by transforming it into an unreadable form without a **decryption key**. Encryption can be applied to a partition, an entire disk, or even individual files.

**A logical partition** is a **partition internal** to an **extended partition**. In the **MBR** partitioning system, an extended partition is used to bypass the limitation of four primary partitions. Inside this extended partition, one can create several **logical partitions**. With a GPT partition table, logical partitions are useless since there is no limit to the number of partitions.

### LVM

**LVM (Logical Volume Manager)** is a tool used mainly under Linux to manage disks and volumes more flexibly than traditional partitioning methods.

**Logical volumes:** LVM allows managing **logical volumes** (not to be confused with logical partitions) which are abstractions of physical disks. LVM works on top of **physical disks** (or physical partitions) and allows creating more flexible volumes, called **logical volumes**, which can be resized and adjusted dynamically: With LVM, you can easily **adjust the size** of logical volumes, add new disks, or remove them without disrupting data or restarting the system. A logical volume can combine several physical disks or partitions.

**Snapshots:** LVM also allows creating **snapshots**, which are exact copies of the state of a volume at a given moment. This can be very useful for backups or tests, as snapshots allow you to return to a previous state of the volume.

### File systems

When you partition a disk, you must choose a **file system** for each partition. The file system determines how data is organized, stored, and retrieved on that partition. Here are the main types of file systems:

-   **FAT32 (File Allocation Table 32):** Compatible with almost all OSes. Limitation: Maximum file size = 4 GB, maximum partition size = 8 TB. Common use: USB keys, external disks for cross-platform compatibility.
-   **NTFS (New Technology File System):** Used by default on Windows. Supports large files (> 4 GB) and advanced features like encryption and permissions. **Common use**: Partitions for Windows or internal disks.
-   **HFS+ / APFS (Apple File Systems):** Used by macOS (APFS is newer, optimized for SSDs). Not natively compatible with Windows. **Common use**: Partitions for macOS.
-   **ext4 (Fourth Extended File System):** Default file system for Linux. Reliable, fast, and supports large files. **Common use**: Partitions for Linux systems.
-   **Swap (Linux Swap Partition):** Used by Linux to extend the main memory (RAM) when it is insufficient. Not used to store normal files. **Common use**: Partitions reserved for swap under Linux.

Only the last two types of file systems (those for Linux) will be used in the project (in reality FAT32 will also be used by the EFS partition but the file system type for the EFS partition will be applied without your intervention).

## Creating the virtual machine

**To create a new virtual machine** (*Virtual Machine* abbreviated *VM*), just click on the "New" button. You will then have access to 4 tabs.

-   **Name and operating system:** Here you must name your VM (mandatory), then you can change the directory where it will be saved (*folder*) and finally you must select the ISO image (*ISO Image*) of Debian that you downloaded previously. The "Type", "Subtype" and "Version" fields will then be filled automatically. **However, you must check the "Skip Unattended Installation" box** to prevent automated installation.
-   **Unattended Install:** This tab will be grayed out and inaccessible if you have checked the appropriate box.
-   **Hardware:** You must select here the main memory (*RAM*) that you want to allocate to your VM (be careful this will reduce the RAM available for the host machine); 1024 MB seems sufficient for the project. Then the number of CPU cores (*processors*) that you want to allocate to your VM (it works very well with just one). Finally on Macbook you must then check the "Enable EFI" box for the reasons seen above.
-   **Hard Disk:** Select "Create a Virtual Hard Disk Now"; you can then change the destination directory, then you must choose the size of the "hard disk" you want to create (15 GB seems largely sufficient, but if you want to have partition sizes similar to those shown in the subject, take 30GB). Finally you must select VDI (*Virtualbox Disk Image*) as the file type for the virtual hard disk (*Hard Disk File Type And Variant*). Do not check the "preallocate full size" option in order to have dynamic memory allocation.

**VDI:** In VirtualBox, when creating or adding a virtual disk to a virtual machine (VM), you must choose a **virtual hard disk file type**. These files represent the hard disk image for the VM, they emulate a physical hard disk for a VM. VDI is the native format of Virtualbox.

**Dynamic allocation:** Without the "pre-allocate full size" option, the virtual disk file only grows when the VM writes data to it. The internal partition in the VM works the same way, but less space is immediately consumed on the host. With the option checked, the total space is allocated right away, but this does not change anything in the management of partitions inside the VM (this means that a virtual disk file of for example 30 GB will be immediately created on the hard drive of your **host computer**).

You can then click "next" to validate the creation of the VM.

## Installing Debian

**To start the Debian installation** select your VM in Virtualbox and click "start". On launch, in the "view" menu of Virtualbox, select "scale mode" then click "switch" to adjust the display window.

In the VM, then select "installation" to begin the installation of Debian.

Select the language, the time zone, then the keyboard layout ("French" for everything in my case).

**Machine name:** Enter the desired name, this is the *hostname* mentioned in the subject (your login followed by "42").

**Domain:** You can leave this field blank.

**What is a "domain"?**

A domain is part of a computer's network address, usually used in environments where multiple machines communicate (like local networks or the internet). For example: **Hostname**: `myserver` **Domain name**: `example.com` **Full name (FQDN)**: `myserver.example.com`

**When is it useful?**

-   **Local or corporate networks**: If your system is part of a larger network (like a corporate network), the domain helps to locate your machine. For example, in a company, your PC could be: `pc-123.office.local`.
-   **Publicly accessible servers**: If you want your server to be identifiable on the internet with a domain name like `www.mysite.com`.

 If you do not specify a domain, Debian will simply use the **hostname** to identify the machine. This works perfectly for home or off-network use.

Then choose a password for the "root" superuser.

### **What is the root superuser?**

The **`root` superuser** is the account with the highest privileges on a Linux or Unix-based operating system (like Debian). It has total access to the system, which allows it to read, write, execute, and modify all files, including protected ones. This account is used to administer and manage the system, install software, configure devices, and modify system settings. The root account is usually designated by the user ID (UID) 0, which is unique and reserved for this role.

The superuser is responsible for user management, including creating, modifying, or deleting accounts. It also handles system maintenance, such as repairing or reconfiguring in case of a problem. Installing or updating software, as well as modifying critical system files, is also part of its tasks. Finally, it manages the backup and restoration of data or partitions.

When logged in as a standard user, it is possible to log in as root using the `su root` command followed by the superuser's password.

**New user:** You must then choose a full name for the creation of the first user as defined in the subject (your intra42 login), then an identifier for this user (you can leave the same thing) and finally choose a password.

### Partition

Select the "manual" partition mode to prevent the installer from automatically creating the partitions. Then select the disk on which to make the partitions (VBOX HARDISK) to create a new partition table. A line showing the free space on the disk will then appear. You must click on this line to create the first partition.

**EFI system partition:** Create a first partition (512 MB size sufficient), place it at the beginning and in "use as" select "EFI System Partition". This partition does not appear in the examples given in the subject, but it is necessary since we are using UEFI mode to boot the operating system. Then just click on "done setting up the partition".

### **Why do I have an extra EFI partition compared to the subject?**

If you have followed so far, you will have understood that the specifics of the Macbook and its non-support of BIOS (as well as perhaps the ARM64 ISO image of Debian) force you to use UEFI which cannot function without this EFI partition.

**/boot partition:** Create a second partition (512 MB size sufficient), in "use as" select "ext4 journaling file system" (since it is the default file system for Linux), then in "mount point" select "/boot", then "done setting up the partition".

### **Why are free spaces of 1 MB or similar being created?**

The free spaces of **1 MB** (or similar) that appear when creating or modifying partitions are not errors. They are intentional and serve important functions related to compatibility, alignment, and efficiency.

First, these spaces allow partitions to be aligned with the physical blocks of modern disks, such as SSDs, which use blocks of 4 KB or more. This alignment optimizes read and write performance. Second, in the context of the GPT partition table, free spaces are reserved at the beginning and end of the disk to store critical metadata and reduce the risk of conflicts with certain tools.

They also serve to ensure compatibility between BIOS and UEFI systems, by reserving space for the headers necessary for booting or for a potential bootable partition. In addition, partition managers, such as GParted or those integrated into Windows and Linux, sometimes leave these free spaces to anticipate future modifications, such as expanding or merging partitions. Finally, some file systems or specific configurations, such as LVM or RAID, require these margins for their proper functioning.

**Configure encrypted volumes:** Before creating the logical volumes, you must perform the encryption as requested in the subject. To do this, you must go to "Configure encrypted volumes", select the remaining free space on the disk then "done setting up the partition", then "finish" and choose a passphrase.

An **encrypted volume** is a storage unit (like a partition, an entire disk, or a file that acts as a virtual disk) whose content is protected by an **encryption** algorithm. This means that the data stored in this volume can only be read or used with a **decryption key**, such as a password, a cryptographic key, or a key file. **AES encryption** (Advanced Encryption Standard) is a symmetric cryptography algorithm used to secure data. It is one of the most commonly used encryption algorithms today, as it is fast, secure, and standardized.

**Configure the logical volume manager:** Then click on "Configure the logical volume manager", then "Create a volume group" (name it "LVMGroup if you want to name it as in the subject) and select the previously encrypted volume (which should be called "sda3_crypt"). Inside this group, we will then create several logical volumes.

**Why on the subject do partitions SDA3 and SDA4 not exist when there is SDA5?**

**Under a system using MBR (Master Boot Record) partitioning**, which is often associated with BIOS, primary partitions are limited to **four** per disk. **SDA1 to SDA4** (or their equivalents on other disks) are reserved for **primary partitions**. These are the first partitions created on the disk. Logical partitions are numbered from **SDA5** onwards.

With **GPT (GUID Partition Table)**, there is **no longer a strict limitation** on primary partitions, unlike MBR. All partitions created on a GPT disk are equivalent. There is no concept of "primary partition" or "logical partition". Partitions on a GPT disk are numbered as follows: **sda1, sda2, sda3**, etc., for each partition created. There is no limitation to **sda1-sda4** as with MBR.

**Create the logical volumes** as on the subject by clicking on "Create a logical volume", select the "LVMGroup" volume group previously created, name the first volume thus created "root", choose its size (for example 10G as in the subject). Repeat the operation for the logical volumes "swap", "home, "var", "srv", "tmp", "var-log" to have the same ones as the bonus part of the subject. Click "finish" to return to the partition table.

**Assign the different volumes:** Then click on the line of the first volume (the line where its size appears in MB or GB) which should be "home" (if you created them in the same order as me) and in "use as" select "ext4 journaling file system", then in "mount point" select "home", then "done setting up the partition".

Repeat the operation for each of the volumes in the group ("var" should be mounted on "var", "root" on "/", "srv", on "srv" etc) with two exceptions:

-   The swap volume: in "use as" you must choose "swap space" and you will therefore not have to choose a mount point.
-   The var-log volume: After choosing its file system, you must select the mount point "other choice" and manually enter "/var/log".

Once the operation is complete, click on "Finish partitioning and write changes to disk" and accept. Then for "Scan for other operating systems" choose "no".

### **What is a mount point?**

A **mount point** is a directory in a file system where a storage device (like a hard drive, a USB key, or a partition) is "connected" to be accessible and usable by the operating system. Once a device is mounted, its files can be accessed as if they were part of the main file system. When a device is mounted, its content becomes accessible at a specific location in the system's file tree (example: `/home`).

### Configure the package management tool

A **package management tool** is a software or a set of programs used to install, update, configure, and manage software on an operating system. It facilitates the process of managing dependencies and updating applications.

Choose the country of the mirror near you ("France" for me), then select a mirror (the first *deb.debian.org* is very good) and you can leave "HTTP proxy" blank.

For "popularity contest" it's up to you to see if you want to participate in the study or not.

In "software selection" uncheck everything in order to have no desktop environment and to install no additional utilities (the necessary utilities will be installed later manually).

Finally accept the installation of GRUB.

**GRUB** (GNU GRand Unified Bootloader) is a boot loader used to launch an operating system when the computer starts. It allows managing multiple operating systems, offering an interactive menu to choose what to boot, and passing parameters to the kernel. Compatible with MBR and GPT, GRUB supports many file systems and offers troubleshooting tools in case of problems. It is highly customizable and is an essential part of Linux and UNIX systems.

# Configure Debian

In this second part, I will present the tools and commands to know to carry out the configuration of Debian as requested in the subject (excluding bonus).

## Configure sudo

**Sudo (superuser do)** is a command that allows a user to execute actions with the privileges of another user, usually **root**, without logging in directly as that user. It ensures precise control of permissions and improves security by limiting administrator access. To configure sudo, you must log in as the root superuser.

### Add a user <username> to the sudo group

**`su root`** allows you to log in as root

**`apt update`** updates the list of available packages in the repositories configured on the system, without installing or updating any packages

**`apt upgrade`** updates all installed packages on the system to their latest available versions, using the information from the list updated by `apt update`

**`apt install sudo`** Installs sudo

**`sudo usermod -aG sudo <username>`** adds the user <username> to the sudo group without removing them from their other groups

**`exit`** allows you to exit a terminal or close a shell session (for example, allows you to close the root session)

**`groups <username>`** displays all the groups to which the user belongs

### Alternative method: modify the sudoers file

To add a user to the **`sudo`** group via the **`sudoers`** file, first open the file using the **`visudo`** command, as this command checks for syntax errors. Type **`sudo visudo`** in the terminal. Then, add the following line at the end of the file to grant **sudo** privileges to the desired user: **`<username> ALL=(ALL:ALL) ALL`**, replacing **`<username>`** with the user's name. This line allows the user to execute any command with **sudo**. Finally, save and close the file. With **`visudo`**, the syntax will be automatically checked to avoid any errors. This gives the user administrator rights and allows them to use **sudo** to execute commands as a superuser.

**The line `ALL=(ALL:ALL) ALL` in the `sudoers` file allows a user to:**

**`ALL`** (before the equals sign): Access all machines, which means this rule applies to all hosts.

**`(ALL:ALL)`**: Execute commands as any user (the first **`ALL`**) and as any group (the second **`ALL`**).

**`ALL`** (after the equals sign): The user can execute all commands with **sudo**.

In summary, this gives the user full execution rights with **sudo** on all commands, regardless of the user or group.

### Apt and aptitude

**`apt`** (Advanced Package Tool) is a package management system used on Debian-based distributions (like Ubuntu). It allows you to install, update, remove, and manage software on a Linux system using software repositories. `apt` simplifies the installation and management of packages by automatically resolving the dependencies necessary for a software to work.

**`apt`** is a modern and simple tool for managing packages via the command line, mainly used for tasks like installing and updating packages. **`aptitude`**, on the other hand, offers a more interactive interface and provides more advanced dependency management, often able to resolve conflicts more flexibly. In general, **`apt`** is preferred for its simplicity, while **`aptitude`** is used for more complex tasks.

### Configure the sudo group

For this, you need to edit the sudoers file (this file defines the permissions for using `sudo` for users and groups) and integrate the parameters you want. The `sudoers` file is usually located in the `/etc/` directory, but it is better to open it securely with the following command:

**`sudo visudo`** allows you to edit the `sudoers` file securely: `visudo` checks the syntax before saving the changes to avoid errors that could make the system inaccessible.

The parameters to write in the file to respect the subject are:

```bash
Defaults     passwd_tries=3                    
# Limits the number of password attempts to 3 before failing.
Defaults     badpass_message="Wrong password. Try again!" 
 # Message displayed after an unsuccessful password entry attempt.
Defaults     logfile="/var/log/sudo/sudo.log"  
# Specifies the location of the log file where sudo commands are recorded.
Defaults     log_input                         
# Enables logging of inputs (commands executed with sudo).
Defaults     log_output                        
# Enables logging of outputs of commands executed with sudo.
Defaults     requiretty                        
# Requires that sudo be executed in a terminal for increased security (disables calls without a terminal).
```

## Configure the UFW firewall

**UFW (Uncomplicated Firewall)** is a firewall management tool for Linux systems. It simplifies the configuration of network filtering rules, allowing users to easily define which network connections are allowed or blocked on a system. It is designed to be easy to use while being powerful, and is often used on distributions like Ubuntu.

### What is AppArmor?

AppArmor (**Application Armor**) is a security module for Linux that applies mandatory access control (MAC) policies. It limits the actions that applications can perform by defining profiles that specify the files, networks, and resources they can access. This helps to reduce the risks associated with malicious behavior or vulnerabilities.

### What is the difference between AppArmor and UFW?

These are two distinct security tools, but they complement the overall protection of a Linux system.

-   **UFW** focuses on managing network connections by configuring firewall rules to allow or block incoming and outgoing traffic on specific ports.
-   **AppArmor** protects applications by restricting the system resources they can use, such as files, memory, or devices.

Although there is no direct link between them, they can be used together to strengthen security. For example, UFW controls network access while AppArmor restricts the behavior of applications exposed to the network.

**`sudo apt install ufw`** installs ufw

**`sudo ufw enable`** enables ufw

**`sudo ufw status`** checks the status to ensure it is enabled

**`sudo systemctl enable ufw`** configures ufw to start automatically at system startup

### What is systemctl?

The `systemctl` command is a system and service management tool under Linux. It allows you to start, stop, restart, enable, disable, and check the status of system services (or units). It is used to interact with the **systemd** initialization system, which manages system processes and services.

Some examples:

-   `systemctl start <service>`: Starts a service.
-   `systemctl stop <service>`: Stops a service.
-   `systemctl status <service>`: Displays the current status of a service.
-   `systemctl enable <service>`: Enables a service to start at boot.
-   `systemctl disable <service>`: Disables the automatic start of a service at boot.

**`sudo ufw status verbose`** displays the current status of the UFW firewall with detailed information. When executed, it shows the list of active rules, the network interfaces concerned, and other information about the firewall's status. The "verbose" keyword adds additional details, such as open ports, default actions (accepted, rejected, etc.) and the general configuration of the firewall.

### What is a port?

A **port** in computing refers to a **communication point** in a system that allows programs or services to exchange data with other computers or systems. It is associated with a network protocol, such as TCP or UDP, and identifies a specific service on a machine. Ports serve as access points so that applications or services can be accessible via a network.

Ports are identified by numbers between 0 and 65535. They are generally divided into three categories:

-   **Well-known ports** (0-1023): These ports are reserved for standard services, such as port 80 for HTTP, port 443 for HTTPS, or port 22 for SSH.
-   **Registered ports** (1024-49151): These ports can be used by non-standard applications, but which are registered with the IANA (Internet Assigned Numbers Authority) to avoid conflicts.
-   **Dynamic or private ports** (49152-65535): These ports are generally used by applications for temporary connections or for data exchanges with servers.

In summary, a port is an identifier used to direct network traffic to the correct service on a given device. For example, a web server listens on port 80 to receive HTTP requests from clients.

### Why do ports appear twice when I check the ufw status?

The ports appear twice in the output of `ufw status verbose` because the UFW firewall manages both IPv4 and IPv6 connections.

**IPv4** (Internet Protocol version 4) and **IPv6** (Internet Protocol version 6) are two versions of the Internet Protocol used to address and route data on the Internet. IPv6 offers a much larger addressing capacity than IPv4, which allows many more devices to be connected to the Internet. IPv4 addresses are shorter and are written in decimal, while IPv6 addresses are longer and are written in hexadecimal. IPv6 has an automatic address configuration mechanism, which facilitates network management. IPv6 was designed with security improvements over IPv4, including data encryption. Although IPv6 is more modern and offers advantages for the future of the Internet, IPv4 is still widely used, and the transition between the two protocols is ongoing. Both can coexist on the same network.

Commands to manage ports:

**`sudo ufw allow <port>`** allows traffic on port <port>

**`sudo ufw deny <port>`** blocks traffic on port <port>

**`sudo ufw delete allow <port>`** removes the authorization

**`sudo ufw delete deny <port>`** removes the block

## Configure SSH

The first step is to install OpenSSH.

**OpenSSH (Open Secure Shell)** is a set of tools and protocols for securing network communications, primarily via the **SSH (Secure Shell)** command. It is used to establish secure remote connections between computers, which allows you to connect to a server across a network while encrypting the exchanges to ensure the confidentiality and integrity of the data. Here are some key points about OpenSSH:

-   **SSH**: OpenSSH allows the use of the SSH protocol, which is a secure method for connecting to another computer (usually a server) via a command line interface. SSH replaces insecure protocols like Telnet and rlogin.
-   **Encryption**: SSH encrypts all data exchanged between the user and the server, thus ensuring that even if the communication is intercepted, the information remains protected.
-   **Authentication**: OpenSSH allows strong authentication via passwords or cryptographic keys (for example, public and private keys) to ensure that only authorized people can connect.
-   **SFTP and SCP**: OpenSSH includes tools for securely transferring files. **SFTP (Secure File Transfer Protocol)** and **SCP (Secure Copy Protocol)** allow you to copy files between computers in an encrypted manner.

In summary, OpenSSH is a reliable and widely used solution for managing secure remote connections between computers on a network, offering strong authentication and data encryption.

**`sudo apt install openssh-server`** installs OpenSSH

**`sudo systemctl status ssh`** checks the status of SSH

The default port for SSH is port 22, however it is possible to change it (and it is requested in the subject). To change the listening port, you must modify the SSH configuration file located at `/etc/ssh/sshd_config`

**`sudo nano /etc/ssh/sshd_config`** opens the file in question with nano
To change the listening port, you must find the line `#Port 22`, uncomment it and replace the port number with the desired number (4242 in this case)
**`sudo systemctl restart ssh`** allows you to restart the service (necessary after changing the port)

**`sudo ufw allow 4242`** allows traffic on port 4242

### Configure port forwarding in Virtualbox

**Port forwarding** in **VirtualBox** allows the **host** (your physical machine) to communicate with the **virtual machine** (VM). It is particularly useful when you want to access services (like a web server, an SSH server, etc.) that are running in the VM, but you do not want them to be directly exposed to the internet. In other words, port forwarding allows the virtual machine to listen on a specific port while making that port accessible from outside the VM, through a port on the host.

In Virtualbox you must select your VM, click on "Configuration" >> "Network" >> "Port Forwarding" and "add a new forwarding rule" (the small + on the right). fill in the fields as follows: Name: Port 4242, Protocol: TCP, Host Port: 4242, Guest Port: 4242. Then in the VM restart SSH again.

### What is TCP?

The **TCP protocol** (Transmission Control Protocol) is a communication protocol used in computer networks to ensure the reliable and orderly transmission of data between two computers. It is often used in conjunction with the IP protocol (Internet Protocol), thus forming the TCP/IP pair.

TCP is used by applications requiring reliable data transmission, such as the web (HTTP), sending emails (SMTP), or transferring files (FTP).

### Connection from the host terminal

To connect to your VM from the host computer's terminal, you must open a terminal and type the command:

**`ssh <username>@localhost -p 4242`** allows you to connect to a server via SSH (Secure Shell) using the specified user (`<username>`) on the local machine (`localhost`). The `-p 4242` indicates that the SSH connection should be made via port `4242` instead of the default SSH port (which is port `22`).

**Block ssh connections as root superuser:** You must modify the SSH configuration file (`sshd_config`) to comply with this instruction from the statement.

**`sudo nano /etc/ssh/sshd_config`** opens the SSH configuration file

You must then find the line `PermitRootLogin`, uncomment it, and set its value to "no"

```bash
PermitRootLogin no
```

## Password policy

You can configure certain password policy rules directly via the **`/etc/login.defs`** file. This file contains global settings for user authentication, including password management.

Under "password aging control" modify as follows:

```bash
# The password must be changed every 30 days
PASS_MAX_DAYS 30

# There must be a minimum of 2 days between each password change
PASS_MIN_DAYS 2

# The user will be warned 7 days before the password expires
PASS_WARN_AGE 7
```

**The changes do not apply automatically to existing users,** so you must use the **`chage`** command to make the changes on existing users (with sudo).

**`sudo chage -M 30 <username>`** the password must be changed every 30 days

**`sudo chage -m 2 <username>`** there must be a minimum of 2 days between each password change

**`sudo chage -W 7 <username>`** the user will be warned 7 days before expiration

**`chage -l <username>`** see the password settings applied to the user <username>

### The libpwquality library

The **`pwquality.conf`** file is a configuration file used by the **libpwquality** library, which is responsible for managing password quality policies under Linux. This file allows you to apply security rules to passwords to ensure that they meet certain complexity criteria.

When a user tries to change their password, the **libpwquality** library checks that the new password respects the rules defined in **`pwquality.conf`**. If the password does not meet the criteria, it will be rejected and the user will be prompted to choose another one.

On a Debian or similar machine, the **`pwquality.conf`** file is usually located at: **`/etc/security/pwquality.conf`**

Open the file with nano and edit it to look like this:

```bash
# Number of characters in the new password that must not be present in the 
# old password.
difok = 7
# The minimum acceptable size for the new password (plus one if 
# credits are not disabled which is the default)
minlen = 10
# The maximum credit for having digits in the new password. If less than 0 
# it is the minimun number of digits in the new password.
dcredit = -1
# The maximum credit for having uppercase characters in the new password. 
# If less than 0 it is the minimun number of uppercase characters in the new 
# password.
ucredit = -1
# ...
# The maximum number of allowed consecutive same characters in the new password.
# The check is disabled if the value is 0.
maxrepeat = 3
# ...
# Whether to check it it contains the user name in some form.
# The check is disabled if the value is 0.
usercheck = 1
# ...
# Prompt user at most N times before returning with error. The default is 1.
retry = 3
# Enforces pwquality checks on the root user password.
# Enabled if the option is present.
enforce_for_root
# ...

```

### Modify the rules for the root superuser

The root superuser must be subject to the same password rules except for "difok" (the number of characters in the new password that must not be present in the old one).

**You must therefore create a specific file for root:**

 **`/etc/security/pwquality-root.conf`**

and edit it with nano so that it is identical to **`pwquality.conf`** with the exception of the line concerning "difok" since we do not want to impose a rule on this parameter.

```bash
# Number of characters in the new password that must not be present in the 
# old password.
# difok = 0
# The minimum acceptable size for the new password (plus one if 
# credits are not disabled which is the default)
minlen = 10
# The maximum credit for having digits in the new password. If less than 0 
# it is the minimun number of digits in the new password.
dcredit = -1
# The maximum credit for having uppercase characters in the new password. 
# If less than 0 it is the minimun number of uppercase characters in the new 
# password.
ucredit = -1
# ...
# The maximum number of allowed consecutive same characters in the new password.
# The check is disabled if the value is 0.
maxrepeat = 3
# ...
# Whether to check it it contains the user name in some form.
# The check is disabled if the value is 0.
usercheck = 1
# ...
# Prompt user at most N times before returning with error. The default is 1.
retry = 3
# Enforces pwquality checks on the root user password.
# Enabled if the option is present.
enforce_for_root
# ...

```

**Modify the PAM file to distinguish root from other users:**

Edit the PAM file `/etc/pam.d/common-password` (with **`sudo`** to be able to write) and just **above** the line:

```bash
password	requisite			pam_pwquality.so retry=3
```

add the following lines:

```bash
# verify if user is root:
auth [success=1 default=ignore] pam_succeed_if.so uid=0
# if user is root:
password	requisite			pam_pwquality.so retry=3 conf=/etc/security/pwquality-root.conf
```

Thus if the user is **root**, the system will use the rules defined in the file **`/etc/security/pwquality-root.conf`** instead of **`/etc/security/pwquality.conf`**

The password policy configuration is complete, you can change the existing passwords so that they comply with the new password policy with the command: **`sudo passwd <username>`**

### What is PAM?

PAM (**Pluggable Authentication Modules**) is a modular system used on Unix/Linux systems to manage authentication. It allows you to integrate different authentication methods (passwords, biometrics, SSH keys, etc.) and to configure security rules centrally for applications and services.

## Other commands to know

The commands for managing users require the use of **`sudo` in most cases (from the moment the command modifies something).**

### Hostname

**`sudo hostnamectl set-hostname <new_hostname>`** change the hostname to <new_hostname>

**`hostnamectl status`** displays system information

The **`hostnamectl`** command allows you to manage the **hostname** of a machine under Linux. It allows you to **see, set or modify** the hostname, as well as other information related to the system, such as the hardware type or the operating system version.

### Add users

The commands for managing users (add, delete, etc.) need to be executed with **`sudo`:**

`adduser <username>`: Creates a new user with interactive interfaces (creation of the home directory, password, possibility of providing information such as the user's phone number, their room number, etc.)

`useradd <username>`: Creates a user with custom options (less interactive).

Unlike adduser, useradd only creates the user with basic settings and does not automatically initialize configurations such as the home folder, startup scripts, etc., unless specific options are used. Requires adding options or modifying system files manually for specific configurations. Examples:

-   `m`: Creates the home directory.
-   `s`: Sets the default shell.
-   `G`: Adds the user to a group.

### Delete users

`deluser <username>`: Deletes a user while keeping their home directory (optionally).

`userdel -r <username>`: Deletes a user and their home directory.

`userdel` is a basic command for deleting a user, but it does not automatically delete their home directory and associated files, except with the `-r` option. In contrast, `deluser` is a more complete and user-friendly tool, often provided by the `adduser` package, which not only deletes the user, but also their associated files and directories in a cleaner and more secure way.

### Modify and list users

`id <username>`: Displays the UID, GID and groups of the user.

`id -g <username>`: Displays the user's primary GID.

The **primary GID** (Group ID) of a user is the identifier of the main group to which that user belongs. When a user is created, a group with the same name is often created automatically, and this group becomes the user's primary group. The primary GID is used to define file and directory access permissions, and determines the user's default permissions for the files they create.

`usermod -L <username>`: Locks an account (disables the password) without deleting it (*lock*).

`usermod -U <username>`: Unlocks an account (*unlock*).

`usermod` is a command used to modify an existing user on a Linux system. It allows you to adjust the settings of a user account, such as changing the primary group or adding secondary groups, modifying the default shell, renaming a user, locking or unlocking an account, and changing the home directory or moving its content.

`passwd <username>`: Changes a user's password.

`cat /etc/passwd`: Displays all users on the system.

`awk -F: '$3 >= 1000 {print $1}' /etc/passwd`: Displays only "human" users

### What are all these users?

The users you see in the `/etc/passwd` file are not only the users you have created. Indeed, this file also contains **system users** created by the system or by software for specific tasks. These system users often do not have a home directory or a shell and are used by services and processes to operate securely, without needing full access to the user environment. For example: daemon, bin, sys, mail are users associated with system services, such as mail management processes or basic services.

The command `awk -F: '$3 >= 1000 {print $1}' /etc/passwd` filters users whose UID (third field in the /etc/passwd file) is greater than 1000 and only displays the name (first field) because system users generally have UIDs less than 1000 (an exception for example is the system user nobody)

`who`: Lists connected users.

`w`: Gives detailed information about user sessions.

### Manage groups

`groupadd <group>` **create a new group**

`groupdel <group>` deletes a group

`getent group <group>` displays the list of members of a group

`groups <username>` lists the groups to which the user belongs.

`usermod -aG <group> <username>`: Adds a user to a group.

## Monitoring.sh script

The script must be created as the root user and saved in the `/root` folder.

To create this script, you just need to declare variables and store the results of the commands detailed below, then display everything using `echo` and `| wall` so that the message is sent to all connected users.

### Some basics of bash scripting

**A bash script starts with a line called a shebang** (`#!/bin/bash`) which specifies the interpreter to use to execute the script. In this example, it indicates that the script should be executed with the **Bash** interpreter. This allows the system to automatically use the correct program to execute the script, even if it is launched directly.

**In bash you can assign the result of a command to a variable** like this:

```bash
variable=$(command)
```

**To access the value of a variable** we use the following syntax:

```bash
$variable
```

`echo` displays a string of characters or the value of a variable in the terminal.

`wall` sends a message to all users connected to the machine.

`|` allows you to redirect the output of one command to the input of another command.

### Example with the `date` command which returns the date and time:

```bash
#!/bin/bash

message=$(date)
echo $message | wall
# This script will send the date and time returned by the date command
# to all connected users
```

### The commented script

```bash
#!/bin/bash

# Retrieves the system architecture, kernel version, and system type (e.g., x86_64, Linux, GNU)
arch=$(uname -srmo)

# Retrieves the system kernel version
karnel=$(uname -v)

# Number of physical CPUs detected on the system
# This command reads the /proc/cpuinfo file, filters the lines containing "physical id", 
# removes duplicates with sort -u and counts the number of lines with wc -l
pcpu=$(cat /proc/cpuinfo | grep "physical id" | sort -u | wc -l)

# Number of virtual CPUs detected on the system
# This command reads the /proc/cpuinfo file, filters the lines containing "processor", 
# then counts the number of processors with wc -l
vcpu=$(cat /proc/cpuinfo | grep "processor" | sort -u | wc -l)

# Calculation of CPU usage in percentage
# Uses the top command to display process statistics, filters the lines containing 'Cpu', 
# then calculates CPU usage with awk by adding user and system process usage
cpu_used=$(top -bn1 | grep 'Cpu' | xargs | awk '{printf("%.1f%%"), $2 + $4}')

# Retrieves the total memory (RAM) available on the system
mem_total=$(free -h | grep Mem | awk '{print $2}')

# Retrieves the used memory (RAM) on the system
mem_used=$(free -h | grep Mem | awk '{print $3}')

# Calculates the percentage of used memory
mem_perc=$(free -k | grep Mem | awk '{printf("%.2f%%"), $3 / $2 * 100}')

# Retrieves the total disk space available on the system
hdd_total=$(df -h --total | grep "total" | awk '{print $2}')

# Retrieves the used disk space on the system
hdd_used=$(df -h --total | grep "total" | awk '{print $3}')

# Retrieves the disk space usage in percentage
hdd_perc=$(df -h --total | grep "total" | awk '{print $5}')

# Retrieves the last date and time of the system reboot
reboot=$(who -b | awk '{print($3 " " $4)}')

# Retrieves the current date and time in YYYY-MM-DD and HH:MM:SS format
actual_time=$(date +%Y-%m-%d && date +%H:%M:%S)

# Number of active TCP connections
# This command retrieves TCP connection information from /proc/net/sockstat 
# and extracts the number of active connections
tcp=$(grep TCP /proc/net/sockstat | awk '{print $3}')

# Number of users currently connected to the system
# This command uses "who" to list connected users and "wc -l" to count the number of lines
user_conn=$(who | wc -l)

# IPv4 address of the machine
ipv4=$(hostname -I)

# MAC address of the machine
mac=$(ip link show | grep ether | cut -c 16-32)

# Number of commands executed with sudo
# The command searches for lines containing "COMMAND=" in the sudo log file and counts the number of occurrences
sudo=$(sudo grep "COMMAND=" /var/log/sudo/sudo.log | wc -l)

# Checks if LVM (Logical Volume Manager) is enabled on the system
# This command uses lsblk to list devices and check if LVM volumes exist
lvm=$(if [ $(lsblk | grep lvm | wc -l) -eq 0 ]; then
                 echo "Disable"
             else
                 echo "Enable"
             fi)

# Displays the collected system information and sends it to all connected users via the wall command
echo "
	________________________________________________________________________
	MONITORING		
	________________________________________________________________________
	Architecture            	: $arch
	Kernel				            : $karnel
	Number of physical CPU		: $pcpu
	Number of virtual CPU		  : $vcpu
	CPU load			            : $cpu_used
	RAM usage			            : $mem_used/$mem_total ($mem_perc)
	Disk usage		          	: $hdd_used/$hdd_total ($hdd_perc)
	Last boot			            : $reboot
	LVM status		          	: $lvm
	Active TCP connexions		  : $tcp
	Users currently connected	: $user_conn
	IPv4 address			        : $ipv4
	MAC address			          : $mac
	Commands run with sudo		: $sudo
	________________________________________________________________________
" | wall

```

Don't forget to give execution permissions with the command `chmod 755 monitoring.sh`.

## Execute a task automatically

To automate the execution of tasks at specific times, you need a **task scheduler** or **scheduling daemon** like **cron.**

**Cron** is a utility under Linux for executing automated tasks at specific times. These tasks, called **jobs**, can be scheduled to run at regular intervals (like every hour, every day, etc.).
You must first enable **cron** so that it starts automatically at each system startup with the following command: **`systemctl enable cron`**

### The crontab

The crontab is the file that contains the tasks to be executed and the dates and times of the scheduled executions. Here is how it works:

1.  **`crontab -e`**: This command allows you to modify the crontab file of the current user (it opens a text editor to modify the appropriate file).
2.  **Format**: A crontab file contains lines with five time fields (minute, hour, day of the month, month, day of the week) followed by the command to be executed.
3.  Example: `30 14 * * 1 /home/user/backup.sh` will execute the `backup.sh` script every Monday at 2:30 PM.
4.  You can define several values per field (for example `30,40 14 * * 1` for the execution to take place at 2:30 PM and 2:40 PM every Monday) or ranges of values (like for example **`1-5`** which means 1, 2, 3, 4 and 5).
5.  Another way to define several values is for example: `*/10 14 * * 1` which means every time the minutes form a multiple of ten (i.e. every ten minutes), at 2 PM, on Mondays.
6.  The `*` fields mean "any" or "all" (for example * on the month field means that the command will be executed regardless of the month).

You can also see the scheduled jobs with **`crontab -l`** and delete a job with **`crontab -r`**.

### Create the crontab for Born2beroot

Still as the root user, you must create the crontab by adding the following line in the file opened by **`crontab -e`:**

**`*/10 * * * * bash /root/monitoring.sh`**

The word "bash" in the line means that the file must be executed as a bash script, but its presence is not normally necessary if you have put the **shebang** at the beginning of your file.

From there, the monitoring.sh script will be executed every ten minutes (for example at 2:10 PM, then 2:20 PM, then 2:30 PM, etc.). However, the subject specifies that it must be executed every ten minutes from the system startup. We will have to create a small script to calculate the delay between the system startup time and the nearest ten minutes to apply this delay to the execution of our script.

### Create the sleep.sh script

Still in the root folder and as the root user, create a sleep.h file containing the following script which uses the sleep command.

The **`sleep` command suspends the execution of a script for a specified time.**

```bash
#!bin/bash

# Get boot minutes and seconds
boot_min=$(uptime -s | cut -c 15-16)
boot_sec=$(uptime -s | cut -c 18-19)

# We calculate the number of minutes that separates us from the nearest 10
# For 18:42:23
# 42%10 = 2 so 2 minutes of difference with the nearest 10 which is 40
# 2*60 = 120 to convert to seconds
# 120+23 = 143 seconds between the nearest 10
delay=$(bc <<< $boot_min%10*60+$boot_sec)

#we pass the seconds to sleep to have our delay
sleep $delay
```

Then modify the previously created line in the crontab as follows:

**`*/10 * * * * bash /root/sleep.sh && bash /root/monitoring.sh`**

The logical AND operator **`&&`** allows the next command to be executed **only if the previous command was successful** (i.e. the previous command terminated with an exit code of 0, indicating success).

This line in the crontab file will execute two scripts at regular intervals. Here is what it does in detail:

-   **`/10 * * * *`**: This means that the command must be executed every 10 minutes (every minute that is a multiple of 10).
-   **`bash /root/sleep.sh`**: The `/root/sleep.sh` script will be executed first.
-   **`&&`**: If the first script (`sleep.sh`) executes correctly (without error), then the second script will be executed.
-   **`bash /root/monitoring.sh`**: The `/root/monitoring.sh` script will be executed **only if** the first script has terminated successfully.

In summary, this line will launch the `sleep.sh` script every 10 minutes, and if this script succeeds (after applying the delay imposed by the sleep command), it will launch `monitoring.sh`.

## The signature.txt file

**To create the file** you just need to go to the directory where the VM is stored and execute **`shasum` on your VM's .vdi file.**

### What is shasum?

**Shasum** is a command used to calculate and verify the cryptographic fingerprints (or "hashes") of files, using SHA (Secure Hash Algorithm) algorithms such as SHA-1, SHA-256, etc. It is mainly used to verify the integrity and authenticity of files. It creates a hash, which is a unique digital fingerprint generated from data, used to verify integrity or authenticity.

What is a VDI file?

A **VDI** (Virtual Disk Image) file is a file format used by **VirtualBox** to represent a virtual hard disk. This file contains all the data of the guest system (like a real hard disk) and allows the virtual machine to access a storage space for its operating system and its files.

**`~/Virtualbox VMS`** is the default destination directory for VMs on Mac. Your VM will therefore be stored in the **`~/Virtualbox VMS/NameOfYourVM`** directory

**`shasum NameOfYourVm.vdi > signature.txt`** is the command to execute to create the fingerprint of your file and store it in the signature.txt file (which will be created by executing this command). The output of shasum stored in signature.txt will be the hash (an alphanumeric string) followed by the name of the analyzed file (if the execution of the command takes a little time, don't panic, it's normal). For example:

a933ad77674444661a25fea08af33154550ddef7  Born2beroot.vdi

**To compare the file** with its signature, you will just have to execute **`shasum`** with the **`-c`** option followed by the name of the signature file (**`signature.txt`** in this case).

The command **`shasum -c signature.txt`** thus allows you to verify the integrity of a file by comparing its hash with the one stored in a signature file. During verification, if the hash matches, the command will display **`file.txt: OK`**, otherwise an error will indicate that the file has been modified.
