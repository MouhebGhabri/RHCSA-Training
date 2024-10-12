


## **Basic Shell Skills**

The shell is the default working environment for a Linux administrator. Bash is the most common shell. The shell allows users and administrators to execute commands, manipulate files, and manage processes.

### **Understanding Commands**

A command typically consists of:
- The **command** itself (e.g., `ls` to list files).
- **Options** that modify the behavior of the command (e.g., `ls -l` shows long listing).
- **Arguments** that target specific files or directories (e.g., `ls -l /etc`).

Command example:
```bash
ls -l /etc
```

This command has two parts:
- `-l`: An option that modifies the command.
- `/etc`: The target directory where the command operates.

### **Executing Commands**

There are three types of commands:
- **Aliases**: Custom shortcuts for commands. Example:
  ```bash
  alias newcommand='oldcommand'
  ```
  Check existing aliases:
  ```bash
  alias
  ```
- **Internal commands**: Built into the shell (e.g., `cd`, `echo`).
- **External commands**: Executable files on disk (e.g., `ls`, `grep`).

Find command types:
```bash
type commandname
which commandname
```

Example:
```bash
type time
which ls
```

### **$PATH Variable**

The shell uses the `$PATH` variable to locate executables. To view the current `$PATH`:
```bash
echo $PATH
```

To run a command from the current directory:
```bash
./scriptname
```

### **I/O Redirection**

Commands have standard input (STDIN), standard output (STDOUT), and standard error (STDERR). Redirect output or errors using:
- Redirect output: 
  ```bash
  command > file
  ```
- Redirect error:
  ```bash
  command 2> errorfile
  ```

Example:
```bash
ls > outputfile
ls wrongfile 2> errorfile
```

To discard output:
```bash
command > /dev/null
```

### **Using Pipes**

Pipes (`|`) connect the output of one command to the input of another. Example:
```bash
ls | less
```

### **Bash History**

Bash saves command history for easy repetition. Useful commands:
- View history:
  ```bash
  history
  ```
- Repeat a command:
  ```bash
  !commandnumber
  !command
  ```
- Search through history:
  ```bash
  Ctrl + r
  ```

To clear history:
```bash
history -c
```

### **Bash Completion**

Bash automatically completes commands, variables, and filenames. Press the `Tab` key after typing part of a command for auto-completion.

```bash
ls <Tab>
```

### **Exercises**

#### **Exercise 1: Using Internal and External Commands**
1. Authenticate on the server and open a terminal.
2. Run the following commands:
   ```bash
   time ls
   which time
   type time
   echo $PATH
   /usr/bin/time ls
   ```

#### **Exercise 2: Using I/O Redirection and Pipes**
1. Run the following:
   ```bash
   ls > /dev/null
   ls wrongfile 2> /dev/null
   ls /etc 2> /dev/null > output
   cat output
   echo hello > output
   ls >> output
   ls -R / | less
   ```

#### **Exercise 3: Working with History**
1. Run the following:
   ```bash
   history
   Ctrl + r (search for commands)
   history | grep cat
   !number
   history -c
   ```

#### **Exercise 4: Bash Completion**
1. Test Bash completion with commands:
   ```bash
   ls <Tab>
   ```

## **Editing Files with `vim`**

Linux system administration often requires editing configuration files, and the most widely available text editor is `vi` or its improved version, `vim`. While `vim` offers many enhancements, such as syntax highlighting, its basic commands are identical to `vi`.

### **`vim` Modes**
`vim` operates in two essential modes:
- **Command Mode**: For issuing commands.
- **Input Mode**: For editing the file's contents.

Switch between these modes as needed for your tasks.

### **Essential `vim` Commands**
The following commands are essential when using `vim`:
- **i**: Enter Input Mode.
- **Esc**: Exit Input Mode and return to Command Mode.
- **:w**: Write (save) the file.
- **:q**: Quit `vim`.
- **dd**: Delete the current line.
- **u**: Undo the last action.
- **:%s/old/new/g**: Replace `old` with `new` throughout the file.

To practice these commands, follow **Exercise 2-5** where you will create and edit a file using `vim`.

## **Understanding the Shell Environment**

A shell environment consists of variables that define the user environment. Examples include:
- `$PATH`: Defines directories to search for executable files.
- `$LANG`: Specifies language settings.

To view the current environment variables, use the `env` command. To modify a variable temporarily, you can assign it a new value, e.g., `LANG=es_ES.UTF-8`.

### **Exercise: Shell Environment Practice**
1. Open a shell and type `echo $LANG` to display your current language setting.
2. Temporarily change the language by typing `LANG=es_ES.UTF-8`.
3. Open `.bashrc` and add a new variable (e.g., `COLOR=red`).

## **Finding Help on Linux**

Hundreds of commands are available on Linux, but it's impractical to memorize them all. Instead, focus on how to find the help you need:
- **--help**: Display a usage summary for a command.
- **man**: Show detailed manual pages for commands.
- **apropos** or `man -k`: Search for commands based on a keyword.
- **info**: Provides hierarchical documentation for commands.

### **Exercise: Using `man` Pages**
1. Search for `man -k partition` to find commands related to partitions.
2. Use `grep` to filter results by specific sections, e.g., `man -k partition | grep 8`.

### **Tip**
To ensure up-to-date `man` pages, periodically run `mandb` to update the `mandb` database.


# Working with the File System Hierarchy

To manage a Linux system, you should be familiar with the default directories that exist on almost all Linux systems. This section describes these directories and explains how mounts are used to compose the file system hierarchy.

## Defining the File System Hierarchy

The file system on most Linux systems is organized in a similar way. The layout of the Linux file system is defined in the Filesystem Hierarchy Standard (FHS), and this file system hierarchy is described in `man 7 file-hierarchy`. 

### Table 3-2 FHS Overview

Understanding **Mounts**

To understand the organization of the Linux file system, you need to understand the important concept of mounting. A Linux file system is presented as one hierarchy, with the root directory (`/`) as its starting point. This hierarchy may be distributed over different devices and even computer systems that are mounted into the root directory.

In the process of mounting, a device is connected to a specific directory, such that after a successful mount, this directory gives access to the device contents.

Mounting devices makes it possible to organize the Linux file system in a flexible way. There are several disadvantages to storing all files in just one file system, which gives several good reasons to work with multiple mounts:

- High activity in one area may fill up the entire file system, which will negatively impact services running on the server.
- If all files are on the same device, it is difficult to secure access and distinguish between different areas of the file system with different security needs. By mounting a separate file system, you can add mount options to meet specific security needs.
- If a one-device file system is completely filled, it may be difficult to make additional storage space available.

To avoid these pitfalls, it is common to organize Linux file systems in different devices (and even shares on other computer systems), such as disk partitions and logical volumes, and mount these devices into the file system hierarchy. By configuring a device as a dedicated mount, you also are able to use specific mount options that can restrict access to the device.

Some directories are commonly mounted on dedicated devices:

- **/boot**: This directory is often mounted on a separate device because it requires essential information your computer needs to boot. Because the root directory (`/`) is often on a Logical Volume Manager (LVM) logical volume, from which Linux cannot boot by default, the kernel and associated files need to be stored separately on a dedicated `/boot` device.
- **/boot/EFI**: If a system uses Extended Firmware Interface (EFI) for booting, a dedicated mount is required, giving access to all files required in the earliest stage of the boot procedure.
- **/var**: This directory is often on a dedicated device because it grows in a dynamic and uncontrolled way (for example, because of the log files that are written to `/var/log`). By putting it on a dedicated device, you can ensure that it will not fill up all storage on your server.
- **/home**: This directory often is on a dedicated device for security reasons. By putting it on a dedicated device, you can mount it with specific options, such as `noexec` and `nodev`, to enhance the security of the server. When you are reinstalling the operating system, it is an advantage to have home directories in a separate file system. The home directories can then survive the system reinstall.
- **/usr**: This directory contains operating system files only, to which normal users normally do not need any write access. Putting this directory on a dedicated device allows administrators to configure it as a read-only mount.

Apart from these directories, you may find servers that have other directories that are mounted on dedicated partitions or volumes also. After all, it is up to the discretion of the administrator to decide which directories get their own dedicated devices.

To get an overview of all devices and their mount points, you can use different commands:

- The `mount` command gives an overview of all mounted devices. To get this information, the `/proc/mounts` file is read, where the kernel keeps information about all current mounts. It shows kernel interfaces also, which may lead to a long list of mounted devices being displayed. 

**Example 3-1 Partial mount Command Output**
```bash
[root@server1 ~]# mount 
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec)
proc on /proc type proc (rw,nosuid,nodev,noexec)
devtmpfs on /dev type devtmpfs (rw,nosuid,seclabe)
securityfs on /sys/kernel/security type securityf
tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev,sec)
devpts on /dev/pts type devpts (rw,nosuid,noexec)
tmpfs on /run type tmpfs (rw,nosuid,nodev,seclabe)
tmpfs on /sys/fs/cgroup type tmpfs (ro,nosuid,nod)
…
/dev/nvme0n1p1 on /boot type xfs (rw,relatime,sec)
sunrpc on /var/lib/nfs/rpc_pipefs type rpc_pipefs
tmpfs on /run/user/42 type tmpfs (rw,nosuid,nodev)
tmpfs on /run/user/1000 type tmpfs (rw,nosuid,nod)
gvfsd-fuse on /run/user/1000/gvfs type fuse.gvfsd
/dev/sr0 on /run/media/student/RHEL-8-0-BaseOS-x8
tmpfs on /run/user/0 type tmpfs (rw,nosuid,nodev,
```

- The `df -Th` command was designed to show available disk space on mounted devices; it includes most of the system mounts. Because it will look on all mounted file systems, it is a convenient command to get an overview of current system mounts. The `-h` option summarizes the output of the command in a human-readable way, and the `-T` option shows which file system type is used on the different mounts.
- The `findmnt` command shows mounts and the relationship that exists between the different mounts. Because the output of the mount command is a bit overwhelming, you may like the output of `findmnt`. 

**Example 3-2 Sample findmnt Command Output**
```bash
[root@server1 ~]# findmnt 
TARGET                               SOURCE      
/                                      /dev/mappe
/sys                                  sysfs      
/sys/kernel/security                    security
/sys/fs/cgroup                       tmpfs      
… 
/proc                                  proc     
/proc/sys/fs/binfmt_misc              systemd-1 
/dev                                  devtmpfs   
/dev/shm                              tmpfs     
/dev/pts                              devpts    
/dev/mqueue                               mqueue
/dev/hugepages                       hugetlbfs   
/run                                 tmpfs       
/run/user/0                             tmpfs   
/run/user/42                             tmpfs  
/run/user/1000                       tmpfs      
/run/user/1000/gvfs                   gvfsd-fuse 
/run/media/student/RHEL-8-0-BaseOS-x86_64 
                                          /dev/sr
/boot                                 /dev/nvme0n
                                                x
/var/lib/nfs/rpc_pipefs                    sunrp
```

### Exercise 3-1 Getting an Overview of Current Mounts
1. Log in as the student user and type `mount`. Notice that the output of the command is quite overwhelming. If you read carefully, though, you’ll see a few directories from the Linux directory structure and their corresponding mounts.
2. Now type `df -hT`. Notice that a lot fewer devices are shown. An example of the output of this command is shown in **Example 3-3**.

**Example 3-3 df -hT Sample Output**
```bash
[root@server1 ~]# df -hT 
   Filesystem        Type        Size  Used Avail
   /dev/mapper/centos-root xfs     5.9G  3.9G  2
   devtmpfs         devtmpfs    908M     0     90
   tmpfs            tmpfs     918M  144K  917M   
   tmpfs            tmpfs     918M   21M  897M   
   tmpfs            tmpfs     918M     0      918
   /dev/sda1           xfs          197M  131M   
```

Now that you have entered the `mount` and `df` commands, let’s have a closer look at the output of the `df -hT` command in Example 3-3. 

The output of `df` is shown in seven columns:
- **Filesystem**: The name of the device file that interacts with the disk device that is used. The real devices in the output start with `/dev` (which refers to the directory that is used to store device files). You can also see a couple of `tmpfs` devices. These are kernel devices that are used to create a temporary file system in RAM.
- **Type**: The type of file system that was used.
- **Size**: The size of the mounted device.
- **Used**: The amount of disk space the device has in use.
- **Avail**: The amount of unused disk space.
- **Use%**: The percentage of the device that currently is in use.
- **Mounted on**: The directory the device currently is mounted on.

Note that when you use the `df` command, the sizes are reported in kibibytes. The option `-m` will display these in mebibytes, and using `-h` will display a human-readable format in KiB

, MiB, or GiB.

## Mounting File Systems Manually

To add a device to the Linux file system hierarchy, you need to mount the file system to an empty directory. If the empty directory does not exist, create it. The steps to mount a file system manually are:

1. Identify the device name and the type of file system on the device you want to mount. You can view the current file systems by using `lsblk`, `blkid`, or `fdisk -l`.
2. Create an empty mount point in your file system hierarchy where you want to mount the device. You can do this by using `mkdir` command:
```bash
mkdir /mnt/mydisk
```
3. Finally, use the `mount` command to mount the device to the directory you created:
```bash
mount /dev/sdX /mnt/mydisk
```

This way, the contents of the `/dev/sdX` device are accessible under the `/mnt/mydisk` directory.

To unmount a file system, use the `umount` command followed by the mount point or the device file:
```bash
umount /mnt/mydisk
```



# Managing Files

As an administrator, you need to be able to perform common file management tasks. These tasks include the following:

- Working with wildcards
- Managing and working with directories
- Working with absolute and relative pathnames
- Listing files and directories
- Copying files and directories
- Moving files and directories
- Deleting files and directories

The following subsections explain how to perform these tasks.

## Working with Wildcards

When you’re working with files, using wildcards can make your work a lot easier. A wildcard is a shell feature that helps you refer to multiple files in an easy way. 

**Table 3-3: Wildcard Overview**

## Managing and Working with Directories

To organize files, Linux works with directories (also referred to as folders). You have already read about some default directories as defined by the FHS. When users start creating files and storing them on a server, it makes sense to provide a directory structure as well. As an administrator, you have to be able to walk through the directory structure.

### Exercise 3-2: Working with Directories

1. Open a shell as the student user. Type `cd`. Next, type `pwd`, which stands for print working directory. You’ll see that you are currently in your home directory; that is, name `/home/<username>`.
2. Type `touch file1`. This command creates an empty file with the name `file1` on your server. Because you currently are in your home directory, you can create any file you want to.
3. Type `cd /`. This changes the current directory to the root (`/`) directory. Type `touch file2`. You’ll see a “permission denied” message. Ordinary users can create files only in directories where they have the permissions needed for this.
4. Type `cd /tmp`. This brings you to the `/tmp` directory, where all users have write permissions. Again, type `touch file2`. You’ll see that you can create items in the `/tmp` directory (unless there is already a `file2` that is owned by somebody else).
5. Type `cd` without any arguments. This command brings you back to your home directory.
6. Type `mkdir files`. This creates a directory with the name `files` in the current directory. The `mkdir` command uses the name of the directory that needs to be created as a relative pathname; it is relative to the position you are currently in.
7. Type `mkdir /home/$USER/files`. In this command, you are using the variable `$USER`, which is substituted with your current username. The complete argument of `mkdir` is an absolute filename to the directory `files` you are trying to create. Because this directory already exists, you’ll get a “file exists” error message.
8. Type `rmdir files` to remove the directory `files` you have just created. The `rmdir` command enables you to remove directories, but it works only if the directory is empty and does not contain any files.

## Working with Absolute and Relative Pathnames

In the previous section, you worked with the commands `cd` and `mkdir`. You used these commands to browse through the directory structure. You also worked with a relative filename and an absolute filename.

An **absolute filename**, or absolute pathname, is a complete path reference to the file or directory you want to work with. This pathname starts with the root directory, followed by all subdirectories up to the actual filename. No matter what your current directory is, absolute filenames will always work. An example of an absolute filename is `/home/lisa/file1`.

A **relative filename** is relative to the current directory as shown with the `pwd` command. It contains only the elements that are required to get from the current directory up to the item you need. Suppose that your current directory is `/home` (as shown by the `pwd` command). When you refer to the relative filename `lisa/file1`, you are referring to the absolute filename `/home/lisa/file1`.

When working with relative filenames, it is sometimes useful to move up one level in the hierarchy. Imagine you are logged in as root and you want to copy the file `/home/lisa/file1` to the directory `/home/lara`. A few solutions would work:

- Use `cp /home/lisa/file1 /home/lara`. Because in this command you are using absolute pathnames, this command will work at all times.
- Make sure your current directory is `/home` and use `cp lisa/file1 lara`. Notice that both the source file and the destination file are referred to as relative filenames and for that reason do not start with a `/`. There is a risk though: if the directory `lara` in this example doesn’t exist, the `cp` command creates a file with the name `lara`. If you want to make sure it copies to a directory, and generates an error message if the directory doesn’t exist, better use `cp lisa/file1 lara/`.
- If the current directory is set to `/home/lisa`, you could also use `cp file1 ../lara`. In this command, the name of the target file uses `..`, which means go up one level. The `..` is followed by `/lara`, so the total name of the target file would be interpreted as “go up one level” (so you would be in `/home`), and from there, look for the `/lara` subdirectory.

**Tip:** If you are new to working with Linux, understanding relative filenames is not always easy. There is an easy workaround, though. Just make sure that you always work with absolute pathnames. Using absolute pathnames involves more typing, but it is easier so you’ll make fewer mistakes.

In Chapter 2, “Using Essential Tools,” you learned how you can use Bash completion via the Tab key to complete commands. Using Bash completion makes it a lot easier to work with long commands. Bash completion works on filenames, too. If you have a long filename, like `my-long-file-name`, try typing `my-` and press the Tab key. If in the current directory, just one file has a name starting with `my-`, the filename will automatically be completed. If there are more files that have a name starting with `my-`, you have to press the Tab key twice to see a list of all available filenames.

## Listing Files and Directories

While working with files and directories, it is useful to show the contents of the current directory. For this purpose, you can use the `ls` command. If used without arguments, `ls` shows the contents of the current directory. Some common arguments make working with `ls` easier. 

**Table 3-4: ls Common Command-Line Options**

**Tip:** A hidden file on Linux is a file that has a name that starts with a dot. Try the following: `touch .hidden`. Next, type `ls`. You will not see the file. Then type `ls -a`. You’ll see it.

When using `ls` and `ls -l`, you’ll see that files are color-coded. The different colors that are used for different file types make it easier to distinguish between different kinds of files. Do not focus too much on them, though, because the colors that are used are the result of a variable setting that might be different in other Linux shells or on other Linux servers.

## Copying Files and Directories

To organize files on your server, you’ll often copy files. The `cp` command helps you do so. Copying a single file is not difficult: just use `cp /path/to/file /path/to/destination`. To copy the file `/etc/hosts` to the directory `/tmp`, for instance, use `cp /etc/hosts /tmp`. This results in the file `hosts` being written to `/tmp`.

**Tip:** If you copy a file to a directory, but the target directory does not exist, a file will be created with the name of the alleged target directory. In many cases, that’s not the best solution and it would be better to just get an error message instead. You can accomplish this by placing a `/` after the directory name, so use `cp /etc/hosts /tmp/` and not `cp /etc/hosts /tmp`.

With the `cp` command, you can also copy an entire subdirectory, with its contents and everything beneath it. To do so, use the option `-R`, which stands for recursive. (You’ll see the option `-R` with many other Linux commands also.) For example, to copy the directory `/etc` and everything in it to the directory `/tmp`, you would use the command `cp -R /etc /tmp`.

While using the `cp` command, you need to consider permissions and other properties of the files. Without extra options, you risk these properties not being copied. If you want to make sure that you keep the current permissions, use the `-a` option, which has `cp` work in archive mode. This option ensures that permissions and all other file properties will be kept while copying. So, to copy an exact state of your home directory and everything within it to the `/tmp` directory, use `cp -a ~ /tmp`.

A special case when working with `cp` is hidden files. By default, hidden files are not copied over. There are three solutions to copy hidden files as well:

- `cp /somedir/.* /tmp` This copies all files that have a name starting with a dot (the hidden files, that is) to `/tmp`. It gives an error message for directories whose name starts with a dot in `/somedir`, because the `-R` option was not used.
- `cp -a /somedir

/.* /tmp` This will do the same as the previous command but keep the attributes of the files (including the date). Again, you’ll get an error message if there are directories whose name starts with a dot.
- You could also simply copy everything from `/somedir` to `/tmp` with `cp -a /somedir /tmp`, because this keeps the attributes and also copies hidden files.

## Moving Files and Directories

To move files or directories, use the `mv` command. The `mv` command works the same way as the `cp` command: use it with the names of the file or directory you want to move, followed by the name of the target file or directory you want to move to. For example, to move a file named `file1` to the directory `/tmp`, you can run the command `mv file1 /tmp`. You can also rename a file with `mv`. To rename `file1` to `file2`, you can use `mv file1 file2`.

**Tip:** To rename multiple files, you can use a for loop to automate the process, like so:
```bash
for f in *.txt; do mv "$f" "${f%.txt}.bak"; done
```

## Deleting Files and Directories

To delete a file or directory, use the `rm` command. To delete a single file, you can use the command `rm file1`. To delete a directory, you need to use the option `-r`, which stands for recursive. This means that the directory and all its contents are removed. 

For example, you can delete the directory `/tmp/mydir` with the command `rm -r /tmp/mydir`. **Use this command with caution**, as it can remove many files at once.

### Exercise 3-3: Removing Files and Directories

1. Open a terminal and create a directory named `files` in your home directory by using `mkdir files`.
2. Change into the directory with `cd files` and create a file named `file1` using `touch file1`.
3. Delete the file you created by using `rm file1`.
4. Go back to the previous directory with `cd ..`.
5. Delete the `files` directory by using `rmdir files`. This will only work if the directory is empty.
6. To remove a non-empty directory, you can use `rm -r files`. Be sure to confirm that the directory contains no important files before doing this.

## Understanding Hard Links

Linux stores administrative data about files in **inodes**. An inode contains crucial information such as:

- The data block where the file contents are stored
- Creation, access, and modification dates
- Permissions
- File ownership

One important piece of information not stored in the inode is the **filename**. Filenames are stored in directories, and each filename links to its corresponding inode to access further file information. 

### Hard Links

- Each file starts with one hard link (its name).
- Multiple hard links can be created for a file, allowing access from different locations.
- All hard links point to the same data blocks; thus, changes to one link are reflected in all.

**Restrictions:**
- Hard links must exist on the same device (partition, logical volume, etc.).
- You cannot create hard links to directories.
- When the last hard link is removed, access to the file’s data is also removed.

The distinction between hard links is that there is no "primary" link; all hard links are equal. The Linux operating system often utilizes links to enhance file accessibility.

## Understanding Symbolic Links

A **symbolic link** (or soft link) does not link directly to the inode but to the filename. This provides flexibility, as symbolic links can link to files on different devices and directories. However, they have a major drawback: if the original file is removed, the symbolic link becomes invalid.

### Creating Links

To create links, use the `ln` command. The syntax is similar to `cp` and `mv`:

- To create a hard link:
  ```bash
  ln source target
  ```
- To create a symbolic link:
  ```bash
  ln -s source target
  ```

**Note:** To create hard links, you must be the owner of the source item.

### Identifying Links

Use `ls -l` to check if a file is a link:

- The first character is `l` for symbolic links.
- For hard links, the output shows the link counter (the number of names associated with the inode).

#### Example Output
```bash
[root@localhost tmp]# ls -l
total 3 
lrwxrwxrwx. 1 root root 5 Jan 19 04:38 home -> /h
-rw-r--r--. 3 root root 158 Jun 7 2013 hosts
```

## Removing Links

Removing links requires caution:

1. Create a directory named `test` in your home directory:
   ```bash
   mkdir ~/test
   ```
2. Copy specific files from `/etc` to `~/test`:
   ```bash
   cp /etc/[a-e]* ~/test
   ```
3. Verify contents:
   ```bash
   ls -l ~/test/
   ```
4. Create a symbolic link:
   ```bash
   ln -s test link
   ```
5. Remove the symbolic link:
   ```bash
   rm link
   ```
6. Recreate the link and remove it with `-rf`:
   ```bash
   ln -s test link
   rm -rf link/
   ```

### Exercise 3-4: Working with Links
1. Open a shell as the student user.
2. Create a hard link to `/etc/passwd` (will fail due to permissions):
   ```bash
   ln /etc/passwd .
   ```
3. Create a symbolic link:
   ```bash
   ln -s /etc/passwd .
   ```
4. Create another symbolic link to `/etc/hosts`:
   ```bash
   ln -s /etc/hosts
   ```
5. Create a new file and a hard link to it:
   ```bash
   touch newfile
   ln newfile linkedfile
   ```
6. Verify link counters with:
   ```bash
   ls -l
   ```
7. Create a symbolic link to `newfile`:
   ```bash
   ln -s newfile symlinkfile
   ```
8. Remove `newfile` and check the behavior of `symlinkfile` and `linkedfile`:
   ```bash
   rm newfile
   cat symlinkfile  # Will fail
   cat linkedfile   # Should work
   ```
9. Restore the situation:
   ```bash
   ln linkedfile newfile
   ```

## Working with Archives and Compressed Files

### Managing Archives with `tar`

The **Tape ARchiver (tar)** utility is used for archiving files. It allows you to perform several tasks, including:

- **Creating an archive**
- **Listing contents**
- **Extracting files**
- **Compressing/uncompressing archives**

### Creating Archives

To create an archive:
```bash
tar -cf archivename.tar /files-to-archive
```
To see the progress, add the `-v` (verbose) option.

### Adding and Updating Files

- To add a file to an existing archive:
  ```bash
  tar -rvf archivename.tar /path/to/file
  ```
- To update an existing archive:
  ```bash
  tar -uvf archivename.tar /path/to/file
  ```

### Extracting Archives

Before extracting, list contents:
```bash
tar -tvf archivename.tar
```
To extract:
```bash
tar -xvf archivename.tar
```
To specify a target directory:
```bash
tar -xvf archivename.tar -C /targetdir
```

### Using Compression

To compress archives during creation, use:

- `-z` for gzip
- `-j` for bzip2
- `-J` for xz

For example, to create a gzipped archive:
```bash
tar -czvf archivename.tar.gz /files-to-archive
```

### Exercise 3-5: Using `tar`
1. Create a tar archive:
   ```bash
   tar cvf etc.tar /etc
   ```
2. Compress the archive:
   ```bash
   gzip etc.tar
   ```
3. List contents of the compressed archive:
   ```bash
   tar tvf etc.tar.gz
   ```
4. Extract a specific file:
   ```bash
   tar xvf etc.tar.gz etc/hosts
   ```
5. Verify extraction:
   ```bash
   ls -R
   ```
6. Decompress the archive:
   ```bash
   gunzip etc.tar.gz
   ```
7. Extract to a specific directory:
   ```bash
   tar xvf etc.tar -C /tmp etc/passwd
   ```
# Using Common Text File–Related Tools

Before we start talking about the best possible way to find text files containing specific text, let’s take a look at how you can display text files in an efficient way. Table 4-2 provides an overview of some common commands often used for this purpose.

## Table 4-2 Essential Tools for Managing Text File Contents

Apart from their use on a text file, these commands may also prove very useful when used with pipes. You can use the command `less /etc/passwd`, for example, to open the contents of the `/etc/passwd` file in the less pager, but you can also use the command `ps aux | less`, which sends the output of the command `ps aux` to the less pager to allow for easy reading.

## Doing More with less

In many cases, as a Linux administrator, you’ll need to read the contents of text files. The `less` utility offers a convenient way to do so. To open the contents of a text file in less, just type `less` followed by the name of the file you want to see, as in `less /etc/passwd`.

From less, you can use the Page Up and Page Down keys on your keyboard to browse through the file contents. Seen enough? Then you can press `q` to quit less. Also very useful is that you can easily search for specific contents in less using `/sometext` for a forward search and `?sometext` for a backward search. Repeat the last search by using `n`.

If you think this sounds familiar, it should. You have seen similar behavior in `vim` and `man`. The reason is that all of these commands are based on the same code.

**Note**  
Once upon a time, less was developed because it offered more features than the classical UNIX tool `more` that was developed to go through file contents page by page. So, the idea was to do more with less. Developers did not like that, so they enhanced the features of the `more` command as well. The result is that both `more` and `less` offer many features that are similar, and which tool you use doesn’t really matter that much anymore. 

There is one significant difference, though, and that is the `more` utility ends if the end of the file is reached. To prevent this behavior, you can start more with the `-p` option. Another difference is that the `more` tool is a standard part of any Linux and UNIX installation. This is not the case for `less`, which may have to be installed separately.

## Exercise 4-1: Applying Basic less Skills

1. From a terminal, type `less /etc/passwd`. This opens the `/etc/passwd` file in the less pager.
2. Type `/root` to look for the text `root`. You’ll see that all occurrences of the text `root` are highlighted.
3. Press `G` to go to the last line in the file.
4. Press `q` to quit less.
5. Type `ps aux | less`. This sends the output of the `ps aux` command (which shows a listing of all processes) to less. Browse through the list.
6. Press `q` to quit less.

## Showing File Contents with cat

The `less` utility is useful to read long text files. If a text file is not that long, you are probably better off using `cat`. This tool just dumps the contents of the text file on the terminal it was started from. This is convenient if the text file is short. If the text file is long, however, you’ll see all contents scrolling by on the screen, and only the lines that fit on the terminal screen are displayed.

Using `cat` is simple. Just type `cat` followed by the name of the file you want to see. For instance, use `cat /etc/passwd` to show the contents of this file on your computer screen.

**Tip**  
The `cat` utility dumps the contents of a file to the screen from the beginning to the end, which means that for a long file you’ll see the last lines of the file only. If you are interested in the first lines, you can use the `tac` utility, which gives the inversed result of `cat`.

## Displaying the First or Last Lines of a File with head and tail

If a text file contains much information, it can be useful to filter the output a bit. You can use the `head` and `tail` utilities to do that. Using `head` on a text file will show by default the first ten lines of that file. Using `tail` on a text file shows the last ten lines by default. You can adjust the number of lines that are shown by adding `-n` followed by the number you want to see. So, `tail -n 5 /etc/passwd` shows the last five lines of the `/etc/passwd` file.

**Tip**  
With older versions of `head` and `tail`, you had to use the `-n` option to specify the number of lines you wanted to see. With current versions of both utilities, you may also omit the `-n` option. So, using either `tail -5 /etc/passwd` or `tail -n 5 /etc/passwd` gives you the exact same results.

Another useful option that you can use with `tail` is `-f`. This option starts by showing you the last ten lines of the file you’ve specified, but it refreshes the display as new lines are added to the file. This is convenient for monitoring log files. The command `tail -f /var/log/messages` (which has to be run as the root user) is a common command to show in real time messages that are written to the main log file `/var/log/messages`. To end this command, press `Ctrl-C`.

By combining `tail` and `head`, you can do smart things as well. Suppose, for instance, that you want to see line number 11 of the `/etc/passwd` file. To do that, use `head -n 11 /etc/passwd | tail -n 1`. The command before the pipe shows the first 11 lines from the file. The result is sent to the pipe, and on that result, `tail -n 1` is used, which leads to only line number 11 being displayed.

## Exercise 4-2: Using Basic head and tail Operations

1. From a root shell, type `tail -f /var/log/messages`. You’ll see the last lines of `/var/log/messages` being displayed. The file doesn’t close automatically.
2. Press `Ctrl-C` to quit the previous command.
3. Type `head -n 5 /etc/passwd` to show the first five lines in `/etc/passwd`.
4. Type `tail -n 2 /etc/passwd` to show the last two lines of `/etc/passwd`.
5. Type `head -n 5 /etc/passwd | tail -n 1` to show only line number 5 of the `/etc/passwd` file.

# Filtering Specific Columns with cut

When you’re working with text files, it can be useful to filter out specific fields. Imagine that you need to see a list of all users in the `/etc/passwd` file. In this file, several fields are defined, of which the first contains the name of the users who are defined. To filter out a specific field, the `cut` command is useful. To do this, use the `-d` option to specify the field delimiter followed by `-f` with the number of the specific field you want to filter out. So, the complete command is:

```bash
cut -d : -f 1 /etc/passwd
```

if you want to filter out the first field of the `/etc/passwd` file. You can see the result in **Example 4-1**.

## Example 4-1 Filtering Specific Fields with cut

```bash
[root@localhost ~]# cut -d : -f 1 /etc/passwd
root
bin
daemon
adm
lp
sync
shutdown
halt
...
```

# Sorting File Contents and Output with sort

Another very useful command to use on text files is `sort`. As you can probably guess, this command sorts text. If you type:

```bash
sort /etc/passwd
```

for instance, the content of the `/etc/passwd` file is sorted in byte order. You can use the `sort` command on the output of a command also, as in:

```bash
cut -f 1 -d : /etc/passwd | sort
```

which sorts the contents of the first column in the `/etc/passwd` file.

By default, the `sort` command sorts in byte order, which is the order that the characters appear in the ASCII text table. Notice that this looks like alphabetical order, but it is not, as all capital letters are shown before lowercase letters. So `Zoo` would be listed before `apple`. In some cases, that is not convenient because the content that needs sorting may be numeric or in another format. The `sort` command offers different options to help sort these specific types of data. Type, for instance:

```bash
cut -f 3 -d : /etc/passwd | sort -n
```

to sort the third field of the `/etc/passwd` file in numeric order. It can be useful also to sort in reverse order; if you use the command:

```bash
du -h | sort -rn
```

you get a list of files sorted with the biggest file in that directory listed first.

You can also use the `sort` command and specify which column you want to sort. To do this, use:

```bash
sort -k3 -t : /etc/passwd
```

for instance, which uses the field separator `:` to sort the third column of the `/etc/passwd` file. Add `-n` to the command to sort in a numeric order, and not in an alphabetic order.

Another example is shown below, where the output of the `ps aux` command is sorted. This command gives an overview of processes running on a Linux system. The fourth column indicates memory usage, and by applying a numeric sort to the output of the command you can see that the processes are sorted by memory usage, such that the process that consumes the most memory is listed last.

## Example 4-2 Using ps aux to Find the Busiest Processes on a Linux Server

```bash
[root@localhost ~]# ps aux | sort -k 4 -n
root         897  0.3  1.1 348584 42200 ?
student     2657  1.0  1.1 2936188 45200 ?
student     2465  0.3  1.3 143976 52644 ?
student     2660  1.9  1.4 780200 53412 ?
root        2480  2.1  1.6 379000 61568 ?
student     2368  0.9  1.6 1057048 61096 ?
root        1536  0.6  1.8 555908 69916 ?
student     2518  0.6  1.8 789408 70336 ?
student     2540  0.5  1.8 641720 68828 ?
student     2381  4.7  1.9 1393476 74756 ?
student     2000 16.0  7.8 3926096 295276 ?
```

# Counting Lines, Words, and Characters with wc

When working with text files, you sometimes get a large amount of output. Before deciding which approach to handling the large amount of output works best in a specific case, you might want to have an idea about the amount of text you are dealing with. In that case, the `wc` command is useful. In its output, this command gives three different results: the number of lines, the number of words, and the number of characters.

Consider, for example, the `ps aux` command. When executed as root, this command gives a list of all processes running on a server. One solution to count how many processes there are exactly is to pipe the output of `ps aux` through `wc`, as in:

```bash
ps aux | wc
```

You can see the result of the command in **Example 4-3**, which shows that the total number of lines is 90 and that there are 1,045 words and 7,583 characters in the command output.

## Example 4-3 Counting the Number of Lines, Words, and Characters with wc

```bash
[root@localhost ~]# ps aux | wc
    90      1045   7583
```

# A Primer to Using Regular Expressions

Working with text files is an important skill for a Linux administrator. You must know not only how to create and modify existing text files, but also how to find the text file that contains specific text.

It will be clear sometimes which specific text you are looking for. Other times, it might not. For example, are you looking for `color` or `colour`? Both spellings might give a match. This is just one example of why using flexible patterns while looking for text can prove useful. These flexible patterns are known as regular expressions, often also referred to as regex in Linux.

To understand regular expressions a bit better, let’s take a look at a text file example, shown in **Example 4-4**. This file contains the last six lines from the `/etc/passwd` file. (This file is used for Chapter 6, “User and Group Management,” for more details.)

## Example 4-4 Sample Lines from /etc/passwd

```bash
[root@localhost ~]# tail -n 6 /etc/passwd
anna:x:1000:1000::/home/anna:/bin/bash
rihanna:x:1001:1001::/home/rihanna:/bin/bash
annabel:x:1002:1002::/home/annabel:/bin/bash
anand:x:1003:1003::/home/anand:/bin/bash
joanna:x:1004:1004::/home/joanna:/bin/bash
joana:x:1005:1005::/home/joana:/bin/bash
```

Now suppose that you are looking for the user `anna`. In that case, you could use the general regular expression parser `grep` to look for that specific string in the file `/etc/passwd` by using the command:

```bash
grep anna /etc/passwd
```

**Example 4-5** shows the results of that command, and as you can see, way too many results are shown.

## Example 4-5 Example of Why You Need to Know About Regular Expressions

```bash
grep anna /etc/passwd 
[root@localhost ~]# 
anna:x:1000:1000::/home/anna:/bin/bash 
rihanna:x:1001:1001::/home/rihanna:/bin/bash 
annabel:x:1002:1002::/home/annabel:/bin/bash 
joanna:x:1004:1004::/home/joanna:/bin/bash
```

A regular expression is a search pattern that allows you to look for specific text in an advanced and flexible way.

## Using Line Anchors

In **Example 4-5**, suppose that you wanted to specify that you are looking for lines that start with the text `anna`. The type of regular expression that specifies where in a line of output the result is expected is known as a line anchor.

To show only lines that start with the text you are looking for, you can use the regular expression `^` (in this case, to indicate that you are looking only for lines where `anna` is at the beginning of the line; see **Example 4-6**).

## Example 4-6 Looking for Lines Starting with a Specific Pattern

```bash
grep ^anna /etc/passwd 
[root@localhost ~]# 
anna:x:1000:1000::/home/anna:/bin/bash 
annabel:x:1002:1002::/home/annabel:/bin/bash
```

Another regular expression that relates to the position of specific text in a specific line is `$`, which states that the line ends with some text. For instance, the command:

```bash


grep anna$ /etc/passwd 
```

returns lines that end with the string `anna`. For example, the last line from **Example 4-4** has the string `joanna`, and running the command results in the following:

## Example 4-7 Example of a Line Ending with Specific Text

```bash
grep joanna$ /etc/passwd 
[root@localhost ~]# 
joanna:x:1004:1004::/home/joanna:/bin/bash 
```

# Wildcards and Regular Expressions

To find text matching a certain pattern, you can use wildcards in regular expressions, which will make your search more flexible.

For instance, if you want to match the first two letters of the first user, `anna`, you can use the `.` character (which means to match any character). So if you type:

```bash
grep an.. /etc/passwd 
```

the command results in:

## Example 4-8 Example of Using Wildcards

```bash
grep an.. /etc/passwd 
[root@localhost ~]# 
anna:x:1000:1000::/home/anna:/bin/bash 
annabel:x:1002:1002::/home/annabel:/bin/bash 
```

In this example, both matches are returned.

Another example is if you want to match any line that contains a user whose name starts with `an`, followed by any character, followed by the letter `d`, you would write the command as follows:

```bash
grep an.d /etc/passwd 
```

In this case, this command does not match any lines from **Example 4-4** because there is no match for that pattern.

If you want to match all users starting with `an`, you would use the wildcard `*`, which matches any number of characters. In this case, you can type:

```bash
grep an* /etc/passwd 
```

**Example 4-9** shows that this command matches every user whose name starts with `an`.

## Example 4-9 Example of Matching Users

```bash
grep an* /etc/passwd 
[root@localhost ~]# 
anna:x:1000:1000::/home/anna:/bin/bash 
annabel:x:1002:1002::/home/annabel:/bin/bash 
anand:x:1003:1003::/home/anand:/bin/bash 
```

# Combining Regular Expressions

You can also combine regular expressions to search for multiple patterns within a single line. Consider the command:

```bash
grep -e an -e ri /etc/passwd 
```

This command gives you all lines that contain `an` or `ri`:

## Example 4-10 Example of Matching Multiple Regular Expressions

```bash
 
[root@localhost ~]# grep -e an -e ri /etc/passwd
anna:x:1000:1000::/home/anna:/bin/bash 
rihanna:x:1001:1001::/home/rihanna:/bin/bash 
annabel:x:1002:1002::/home/annabel:/bin/bash 
anand:x:1003:1003::/home/anand:/bin/bash 
joanna:x:1004:1004::/home/joanna:/bin/bash 
```



## Using Escaping in Regular Expressions
When using regular expressions, it's advisable to escape special characters to prevent the Bash shell from interpreting them. This is because characters like `*`, `$`, and `?` have specific meanings in the shell. Although escaping isn’t always necessary, it's a good practice to place regular expressions in quotes. For example, instead of running:

```bash
grep ^anna /etc/passwd
```

you should use:

```bash
grep '^anna' /etc/passwd
```

## Using Wildcards and Multipliers
Wildcards and multipliers help when you're unsure of the exact text format you’re searching for. The dot (`.`) serves as a wildcard for one specific character. For example, `r.t` matches `rat`, `rot`, and `rut`. To specify a range of characters, you can use brackets, e.g., `r[aou]t` matches `rat`, `rot`, and `rut`, but not `rit` or `ret`.

The multiplier `*` matches zero or more of the previous character. If you want to specify an exact number of occurrences, use `{n}`. For instance, `re{2}d` matches `reed`, but not `red`. The `?` operator matches zero or one occurrence of the previous character.

### Table 4-3: Most Significant Regular Expressions

| Pattern | Description                          |
|---------|--------------------------------------|
| `.`     | Matches any single character         |
| `[aou]` | Matches any one character in the set |
| `*`     | Matches zero or more of the previous character |
| `{n}`   | Matches exactly n occurrences        |
| `?`     | Matches zero or one of the previous character |

## Using Extended Regular Expressions
Regular expressions have two sets: basic and extended. Basic regular expressions are supported by tools like `grep`, while extended ones require the `-E` option. For example, the `+` operator (in extended regex) looks for one or more occurrences of the preceding character, while `?` looks for zero or one occurrence.

### Example
To illustrate an extended regex, consider the expression `"/web(/.*)?"` from the `semanage-fcontext` man page. Here, `/web` can be followed by `(/.*)?`, where `?` indicates the preceding group may appear zero or one time. Thus, it matches just `/web`, `/web/`, or `/web/somefile`.

### Practical Example
1. Create a file named `regex.txt` with the following contents:
   ```
   bat
   boot
   boat
   bt
   ```
2. Use the command to find lines starting with `b` and ending with `t`:
   ```bash
   grep 'b.*t' regex.txt
   ```
3. Attempt using the extended regex without options:
   ```bash
   grep 'b.+t' regex.txt
   ```
   This won’t return results as `grep` doesn't recognize the extended regex by default.
4. Use the `-E` option for extended regex:
   ```bash
   grep -E 'b.+t' regex.txt
   ```
   Now it functions as expected.
