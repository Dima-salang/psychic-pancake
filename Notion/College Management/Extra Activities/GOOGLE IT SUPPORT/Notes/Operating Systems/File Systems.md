**File Systems: A Quick Refresher**

- **Definition**: A file system is a method of organizing and storing computer files and data on storage devices like disks, USB drives, or hard drives. It enables the operating system to manage and access files efficiently.
- **Importance**: Without a file system, the operating system would be unable to organize and interact with files effectively.
- **Initial Setup**: When using a new storage device, such as a disk or USB drive, a file system needs to be added to it for proper functioning.
- **Default File Systems**:
    - Windows: NTFS (New Technology File System)
    - Linux: ext4 (Fourth Extended Filesystem)
- **Compatibility**:
    - File systems may have varying degrees of compatibility with different operating systems (OS).
    - Cross-OS support is generally limited. For example:
        - NTFS is compatible with both Windows and Linux (Ubuntu).
        - ext4 is primarily recommended for Linux and has limited support on Windows without third-party tools.
- **Common Scenario**:
    - If you have a USB drive with important files:
        - Using an ext4 USB drive may not work on Windows without third-party tools.
        - To access files on multiple OS (Windows, Linux, macOS), you may need to reformat the USB drive.
- **Universal File System**:
    - FAT32 (File Allocation Table 32) is a file system that supports reading and writing data on Windows, Linux, and macOS.
    - Limitations of FAT32:
        
        - Maximum file size: 4 gigabytes.
        - Maximum file system size: 32 gigabytes.
        
          
        

  

**Components of a Disk for File Storage**

1. **Partitions**:
    - Definition: Divisions or sections of a storage disk.
    - Purpose: To manage and organize disk space, allowing multiple logical drives on a single physical disk.
    - Usage Example: Creating separate partitions for different operating systems on a single machine.
2. **Partition Table**:
    - Definition: A data structure that describes how the disk is divided into partitions.
    - Purpose: Informs the operating system about the layout and characteristics of each partition on the disk.
    - Types:
        - **MBR (Master Boot Record)**:
            - Traditional partition table.
            - Commonly used in Windows OS.
            - Limits volume size to two terabytes or less.
            - Allows up to four primary partitions; additional partitions created as extended partitions with logical partitions inside.
        - **GPT (GUID Partition Table)**:
            - Modern partition table.
            - Volume size can exceed two terabytes.
            - Supports unlimited partitions on a disk.
            - Compatible with UEFI (Unified Extensible Firmware Interface) booting.

**Partitioning and Formatting Disks**

- **Partitioning**:
    - The process of dividing a disk into partitions, each acting as an independent logical unit.
    - Allows for the organization of data and multiple operating systems on a single physical disk.
- **Formatting**:
    - When a file system is added to a partition, it becomes a volume.
    - Formatting is the process of preparing a partition (volume) to store files by creating a file system on it.
- **Operating System Compatibility**:
    - Different operating systems may require specific partition table schemes (MBR or GPT) and file systems (e.g., NTFS for Windows, ext4 for Linux).
- **UEFI and GPT**:
    
    - UEFI is the default BIOS standard for modern systems.
    - To use UEFI booting, the disk must use the GPT partition table scheme.
    
      
    

You can format a disk using the disk management utility tool and the diskpart command-line utility tool.

  

  

DiskPart is a tool that lets you manage your storage from a command line interface and is useful for a multitude of actions including creating, deleting, merging, and repairing drives.

- The three main divisions of storage that you will find on a drive are cluster, volume, and partition.
- To use DiskPart you will need to use specific commands to select and manage the parts of your drive you need to access.
- Cluster size is the smallest division of storage possible in a drive. Cluster size is important because a file will take up the entire size of the cluster regardless of how much space it actually requires in the cluster.

  

**Mounting a File System in Windows**

- **Definition of Mounting**:
    - In IT, "mounting" refers to making a file system or storage device accessible to the computer's operating system.
- **Automated Mounting in Windows**:
    - Windows automatically mounts file systems when you insert a USB drive.
    - When you plug in a USB drive, it appears as an accessible drive in the list of drives in Windows Explorer.
    - You can start using the drive right away for reading and writing data.
- **Unmounting or Safely Ejecting**:
    
    - When you're finished using a mounted drive, it's essential to safely eject or unmount it.
    - To safely eject a drive:
        - Right-click on the drive in Windows Explorer.
        - Select "Eject."
    - Safely ejecting the drive ensures that all pending write operations are completed, preventing data loss or corruption.
    
      
    

  

**Partitioning and Formatting a Disk in Linux Using Parted**

1. **Viewing Connected Disks in Command Line**:
    - To see the connected disks, use the `**parted -l**` command.
    - Example: `**sudo parted -l**`
    - The output lists disks with details such as partition table type (e.g., GPT), partition numbers, start and end points, sizes, file system, and flags.
2. **Selecting the Disk for Partitioning**:
    - Use the interactive mode of Parted to select the desired disk for partitioning.
    - Example: `**sudo parted /dev/sdb**`
3. **Setting Disk Label**:
    - If the disk doesn't have a label, create one using the `**mklabel**` command.
    - Example: `**mklabel gpt**` (for GPT partition table).
4. **View Disk Information**:
    - Confirm the disk information using the `**print**` command.
    - Example: `**print**`
    - Verify that the partition table type is now GPT.
5. **Creating Partitions**:
    - Use the `**mkpart**` command to create partitions.
    - Provide partition type (e.g., primary), file system type (e.g., ext4), start, and end points.
    - Example: `**mkpart primary ext4 1MiB 5GiB**`
    - Note: Measuring in mebibytes (MiB) and gibibytes (GiB) ensures precise data measurements.
6. **Formatting Partitions**:
    - After creating a partition, format it with a file system using `**mkfs**`.
    - Example: `**sudo mkfs -t ext4 /dev/sdb1**`
7. **Unmounted, Usable Disk**:
    - The disk is partitioned and formatted but not yet accessible for file operations.
    - To use it, you must mount the file system to a directory.
8. **Mounting the File System**:
    
    - Mounting is the process of making the file system accessible.
    - Use the `**mount**` command to mount the partition to a directory.
    - Example: `**sudo mount /dev/sdb1 /mnt/mydrive**`
    - `**/mnt/mydrive**` is the mount point where you can access the partition.
    
      
    

**Mounting and Unmounting File Systems in Linux**

- **Mounting File Systems**:
    - To interact with a file system on a disk, you must mount it to a directory.
    - Example: If the desired partition is `**/dev/sdb1**` and you have created a directory called `**myusb**`, you can mount it using:
        
        ```Shell
        bashCopy code
        sudo mount /dev/sdb1 /myusb
        
        ```
        
    - The mount point `**/myusb**` is where you can access the partition.
- **Automatic Mounting**:
    - Most operating systems automatically mount file systems when devices like USB drives are plugged in.
    - File systems must be mounted to tell the OS how to interact with the device.
- **Unmounting File Systems**:
    - Unmounting is the process of detaching a file system from a directory.
    - Use the `**umount**` command to unmount a file system.
    - Example: To unmount `**/myusb**`, you can use either:or
        
        ```Shell
        bashCopy code
        sudo umount /myusb
        
        ```
        
        ```Shell
        bashCopy code
        sudo umount /dev/sdb1
        
        ```
        
    - Always unmount a file system before physically disconnecting the drive to avoid potential file system errors.
- **Permanent Mounting**:
    
    - If you want a disk to be automatically mounted at boot, you can configure it in the `**/etc/fstab**` file.
    - Add an entry with the following information:
        - UUID (Universally Unique Identifier) of the disk.
        - Mount point (directory).
        - File system type.
        - Additional mount options (optional).
    - Obtain the UUID using:
        
        ```Plain
        Copy code
        sudo blkid
        
        ```
        
    - Edit `**/etc/fstab**` and add an entry similar to existing ones.
    
      
    
      
    

**Virtual Memory and Swap Space**

- **Virtual Memory**:
    - Virtual memory is a concept used by operating systems to manage physical memory (like RAM) and provide a consistent memory abstraction to applications.
    - It creates a mapping of virtual addresses used by programs to physical addresses in RAM.
    - Virtual memory allows programs to access memory without worrying about other programs' memory usage and eliminates the need to track physical memory locations.
    - It enables the use of more memory than physically available by using a portion of the hard drive as storage space for data blocks called "pages."
    - Pages not actively used are "swapped out" to the hard drive, and the most frequently accessed pages remain in RAM.
- **Paging Mechanism**:
    - Almost all operating systems employ virtual memory management schemes and paging mechanisms.
    - Pages of memory are read from and written to the hard drive as needed.
    - The goal is to keep frequently accessed data in RAM for faster access, moving less frequently used data to the slower hard drive storage.
- **Windows Virtual Memory**:
    - Windows uses a Memory Manager to handle virtual memory.
    - Pages saved to disk are stored in a hidden file on the root partition called "pagefile.sys."
    - Windows automatically creates and manages page files.
    - The size, number, and location of paging files can be modified through the "System Properties" control panel applet.
- **Configuring Paging Files in Windows**:
    
    - Access "System Properties" through Control Panel: "System and Security" -> "System" -> "Advanced system settings" -> "Advanced" tab -> "Settings" button in the "Performance" section.
    - Navigate to the "Advanced" tab and locate the "Virtual memory" section, which displays the paging file size.
    - Click the "Change" button to adjust paging file settings.
    - Microsoft provides guidelines for setting the paging file size, with recommended minimums based on system RAM.
    - Generally, letting Windows manage the paging file size is suitable unless specific requirements dictate otherwise.
    
      
    

  

**Creating Swap Space in Linux**

- **Swap Space**:
    - Swap space is a dedicated area on a hard drive used for virtual memory management.
    - It allows the operating system to use part of the hard drive as an extension of physical RAM.
    - Swap space is essential for systems with limited physical memory and helps prevent out-of-memory issues.
    - Typically, swap space is created on main storage devices like hard drives and SSDs.
- **Determining Swap Space Size**:
    - A common guideline is to follow recommended partitioning schemes for your system.
    - Swap space size depends on factors such as system RAM and usage patterns.
    - In practice, swap space is often created alongside other partitions on main storage devices.
- **Creating Swap Space**:
    - To create swap space, you can use disk partitioning tools like `**parted**`.
    - Example: To create a swap partition on `**/dev/sdb**` with a size of 5 GiB (Gibibytes), you can use the following commands:
        
        ```Shell
        bashCopy code
        sudo parted /dev/sdb
        (parted) mkpart primary linux-swap 5GiB 100%
        
        ```
        
    - The `**100%**` endpoint indicates that you want to use the remaining free space on the drive.
- **Formatting as Swap**:
    - Swap space is not a file system, so it requires a specific format.
    - To format the partition as swap, use the `**mkswap**` command.
    - Example: To format `**/dev/sdb2**` as swap, run:
        
        ```Shell
        bashCopy code
        sudo mkswap /dev/sdb2
        
        ```
        
- **Enabling Swap**:
    - After formatting, enable swap space using the `**swapon**` command.
    - Example: To enable swap on `**/dev/sdb2**`, run:
        
        ```Shell
        bashCopy code
        sudo swapon /dev/sdb2
        
        ```
        
- **Automatic Mounting**:
    
    - To have swap space automatically mounted at boot, add a swap entry to the `**/etc/fstab**` file, similar to what was done earlier.
    
      
    
      
    

  

**File Data and File Metadata**:

- File Data: This refers to the actual content or information stored within a file, such as the text within a text document, the pixels in an image, or the bytes in a program.
- File Metadata: This encompasses all other information associated with a file, including attributes like the owner of the file, file permissions, file size, file location on the hard drive, creation and modification timestamps, and more.

**Master File Table (MFT)**:

- NTFS uses a structure called the Master File Table (MFT) to manage files, their data, and their associated metadata.
- Every file on an NTFS volume has at least one entry in the MFT, including the MFT itself.
- Typically, there is a one-to-one correspondence between files and MFT records, but for files with extensive attributes, multiple MFT records may be used to represent them.
- Attributes in the MFT include file names, timestamps, file attributes (e.g., read-only, compressed), file data location, and various other properties.
- When you create files on an NTFS file system, entries are added to the MFT. When files are deleted, their MFT entries are marked as free for potential reuse.

**File Record Number (FRN)**:

- Each file's entry in the MFT is identified by a unique identifier called the File Record Number (FRN).
- The FRN serves as an index to locate the file's entry within the MFT.

**Shortcut Files**:

- In Windows, shortcut files are a special type of file that serves as references to other files or destinations.
- These shortcut files have their own entries in the MFT, complete with their metadata.
- When you open a shortcut, it redirects you to the target file or location.

**Symbolic Links**:

- Symbolic links (symlinks) are similar to shortcuts but function at the file system level.
- When you create a symbolic link, it adds an entry in the MFT that points to the name of another entry or file.
- The operating system treats symbolic links like substitutes for the linked file in most scenarios.

**Hard Links**:

- Hard links are references to existing files at the file system level.
- Unlike symbolic links, hard links point directly to the File Record Number (FRN) of the target file, not its name.
- Modifications made to the content of the file through one hard link affect all other hard links to the same file.
- Renaming the target file does not impact hard links because they rely on the FRN.

**Creating Symbolic and Hard Links**:

- To create a symbolic link, you can use the `**mklink**` command with the `**/D**` option for directories or the `**/H**` option for hard links.
- Example for creating a symbolic link: `**mklink /D [link_name] [target]**`
- Example for creating a hard link: `**mklink /H [link_name] [target]**`

**Usage Differences**:

- Symbolic links are flexible and versatile, behaving like the original file in most respects.
- Hard links are rigid and are often used to create multiple references to the same file with different names.

  

  

  

In the Linux file system, particularly in ext2, ext3, ext4, and other Unix-like file systems, files and their associated metadata are managed using a structure called an Inode. Inodes are somewhat analogous to Windows NTFS MFT (Master File Table) records and play a crucial role in organizing and tracking files. Here are some key concepts related to Inodes, soft links (symbolic links), and hard links in Linux:

**Inodes**:

- Inodes are data structures that store metadata about files, such as ownership, permissions, timestamps (creation, modification, and access), and other attributes.
- While Inodes contain a wealth of information about a file, they do not store the actual file data or the file name itself.
- Each file and directory on a Linux file system has a corresponding Inode entry.
- Inodes are organized into an Inode table within the file system.

**Soft Links (Symbolic Links)**:

- Soft links, often referred to as symbolic links or symlinks, are similar to shortcuts in Windows.
- They provide a way to create references or pointers to other files or directories.
- Symbolic links point to the target file or directory using a file name. Unlike hard links, they do not directly reference an Inode or physical location.
- Symlinks are flexible and can point to files or directories regardless of their location on the file system.
- They are created using the `**ln**` command with the `**s**` flag: `**ln -s [target] [link_name]**`.

**Hard Links**:

- Hard links also provide a way to create references to files, but they work differently from symbolic links.
- When you create a hard link, it points directly to the Inode of the target file, essentially linking to the same data on disk.
- Multiple hard links can exist for the same file, all of which share the same Inode and data blocks.
- Changes made to the content of one hard-linked file are reflected in all other hard links because they all point to the same Inode.
- Hard links are indicated by a count in the third field when you use the `**ls -l**` command to list files.
- To create a hard link, you use the `**ln**` command without the `**s**` flag: `**ln [target] [link_name]**`.

**Usage and Benefits**:

- Soft links are versatile and can point to files or directories using relative or absolute paths. They are particularly useful for creating shortcuts or referencing files that may be relocated.
- Hard links are efficient in terms of storage because they share data blocks with the original file. Deleting one hard link does not affect other hard links as long as at least one reference remains.
- Hard links are commonly used for creating multiple references to the same file without duplicating data, which can be helpful for versioning or backup purposes.

  

  

1. **Using Disk Management**:
    
    - Open the "Computer Management" utility by right-clicking on "This PC" (or "My Computer" in older versions of Windows) and selecting "Manage."
    - In the Computer Management window, expand "Storage" and select "Disk Management."
    - Right-click on the partition or disk you want to check and select "Properties."
    - In the "General" tab, you'll find information about the used and free space on the selected drive.
    
    This method provides a graphical representation of disk usage.
    
2. **Using Command Line (DU Utility)**:
    
    - Windows offers a command-line utility called DU (Disk Usage) as part of the Sysinternals suite of tools.
    - DU can provide detailed information about disk usage and the number of files on a given disk.
    - It's particularly useful for scripting and text-based output.
    
    You can download the Sysinternals Suite, which includes the DU tool, from the official Microsoft website or trusted sources.
    
3. **Disk Cleanup**:
    - Windows provides a built-in tool called "Disk Cleanup" (cleanmgr.exe) that helps you free up disk space by removing unnecessary files.
    - To run Disk Cleanup, press the Windows key, type "Disk Cleanup," and select the tool from the search results.
    - You'll be prompted to choose a drive to clean up. Select the drive you want to clean and click "OK."
    - Disk Cleanup will analyze the selected drive and present a list of file categories that can be deleted.
    - Check the categories you want to clean, and then click "OK" to initiate the cleanup process.
    - This process can include deleting temporary files, compressing old files, cleaning up logs, and emptying the recycle bin.
4. **Disk Defragmentation**:
    
    - Disk defragmentation is the process of reorganizing files on your hard drive to improve read and write performance, particularly on traditional spinning hard drives (HDDs).
    - It's less beneficial for solid-state drives (SSDs) because they don't have moving parts.
    - Windows schedules automatic defragmentation tasks, but you can manually initiate defragmentation if needed.
    - To manually defragment a drive, type "Disk Defragmenter" in the Windows search bar and select the tool.
    - Select the drive you want to defragment and click "Analyze" to check whether defragmentation is needed.
    - If analysis suggests that defragmentation will be beneficial, click "Optimize" to start the process.
    
      
    
      
    

**For Disk Usage in Linux**:

- Use the `**du -h**` command to check disk usage in a specific directory.
- The `**h**` flag provides human-readable data sizes.

**For Free Space in Linux**:

- Use the `**df -h**` command to check free space on the entire system.
- The `**h**` flag provides human-readable data sizes.

  

  

In Lesson: "The Importance of Safely Ejecting USB Drives and Handling Data Corruption in Windows NTFS," we delve into the significance of safely ejecting USB devices and addressing data corruption in Windows NTFS file systems. Here's a detailed breakdown:

**Introduction:**

- Unplugging USB devices without proper ejection can lead to data corruption.
- Error messages prompt us to safely eject these devices.
- We use data buffers or caches in RAM for data transfer because RAM is faster than hard drives.
- If we don't allow data buffers to complete data transfer, data corruption can occur.
- Data corruption can result from various factors, including power outages, system crashes, or software bugs.

**Windows NTFS Features to Prevent and Recover from Data Corruption:**

- Journaling: NTFS logs changes to file metadata in the NTFS Log.
    - This log maintains a history of system actions, allowing recovery to a consistent state.
- Self-healing: NTFS automatically addresses minor issues and corruption in the background.
    - It operates without requiring a system reboot.
- NTFS Check Disk Utility: Used in severe cases of corruption, like bad disk sectors or disk failures.
    - It can be run manually with options to fix problems (`**chkdsk /F**`) and specify the drive.

**Checking Self-Healing Status in Windows:**

- You can check the status of self-healing using the `**fsutil**` tool.
    - Example command: `**fsutil repair query C:**`

**Automatic Repair During Boot:**

- Windows detects corruption and sets a bit in metadata if issues arise.
- During boot, the check disk utility checks this bit.
- If set, it initiates repair by reconstructing the file system from the NTFS Log.

**Summary:**

- Windows NTFS employs journaling, self-healing, and check disk utilities to recover from data corruption.
- These features ensure robust measures for data protection in NTFS file systems.