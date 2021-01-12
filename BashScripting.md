# Bash Scripting
## Index
[File Globbing](https://github.com/hassani0393/cheatsheets/blob/main/BashScripting.md#file-globbing)<br>
[Linking Files](https://github.com/hassani0393/cheatsheets/blob/main/BashScripting.md#linking-files)<br>
["tree" command](https://github.com/hassani0393/cheatsheets/blob/main/BashScripting.md#tree-command)<br>
[Viewing File Contents](https://github.com/hassani0393/cheatsheets/blob/main/BashScripting.md#viewing-file-contents)<br>
[Processes](https://github.com/hassani0393/cheatsheets/blob/main/BashScripting.md#processes)<br>
[Disk Space](https://github.com/hassani0393/cheatsheets/blob/main/BashScripting.md#disk-space)<br>
[Working with Data Files](http://example.com/ "Title")<br>
[Subshell](http://example.com/ "Title")<br>
[Putting a process list in the background](http://example.com/ "Title")<br>
[Co-processing](http://example.com/ "Title")<br>
[Built-in commands](http://example.com/ "Title")<br>
[Environment Variables](http://example.com/ "Title")<br>
[Locating System Environment Variables](http://example.com/ "Title")<br>

[Basic Script Building](http://example.com/ "Title")<br>



## File Globbing

To represent one character:
  
    ls -l my_scr?pt

To represent any number of characters:

    ls -l my_s*t // Returns myscript.

To 'or' in a search:

    ls -l my_sr[ai]pt

To specify a range such as alphabetic:

    ls -l f[a-i]ll 

To 'not' in a search:

    ls -l f[!a]ll // won't return fall.
___

## Linking Files

### Symbolic Link

To create a symbolic link to a file:

    ln -s data_file s1_data_file // Created the symbolic link s1_data_file

<p>
The two above file's inode numbers are different to. The inode number of a file or directory is a unique identification number that the kernel assigns to each object in the filesystem.
</p>

To view a file or directory's inode:

    ls -i data_file

### Hard Link

To create a hard link to a file:

    ln data_file h1_data_file

To check to which file a link is linked to:

    file s1_data_file

<p>
A hard link creates a separate virtual file that contains information about the original file and where to locate it, However, unlike the soft link, they are physically the same file and have the same inode number and file size. By referencing the hard link file, it's just like referencing the original file. 
</p>

<p>
The hard link can only be created between files on the same physical medium. To create a link between files under separate physical mediums, symbolic link must be used.
</p>

<p>
Try to create multiple soft links to the original file instead of creating a chain of softlinks that point to eachother and can be broken and cause problems.
</p>

___
## "tree" command

To display directories, subdirectories and their files:

    tree /director-name
___
## Viewing File Contents

To check file type:

    file name_of_file

To display all the data inside a text file:

    cat file1

"cat" parameters:

    -n: Numbers all the lines.
    -b: Numbers the lines that have text in them.
    -T: Replaces any tabs with ^I.

To display text with limited navigation:

    more textfile

To display text with full scrolling and searching capabilities:

    less textfile

To view the last 10 lines of a file:

    tail log_file

"tail" parameters:

    -20: Shows the last 20 lines
    -f: Displays the file real-time.

To display first 10 lines of the file:

    head log_file

___
## Processes

To examine all processes:

    ps -ef
    ps -efl // long format

For real-time process monitoring:

    top

"top" interactive commands:

    f: select the sorting field.
    d: change the polling interval.

<p>
Processes communicate with eachother using <strong>signals</strong>. It's a predefined message that processes recognize and choose to act on or ignore.
</p>

<center>

| Signal | Name | Description |
| :---: | :---: | :---: |
| 1 | HUP | Hangs up |
| 2 | INT | Interrupts |
| 3 | QUIT | Stops running |
| 9 | KILL | Unconditionally terminates |
| 11 | SEGV | Produces segment violation |
| 15 | TERM | Terminates if possible |
| 17 | STOP | Stops unconditionally, but doesn't terminate|
| 18 | TSTP | Stops or pauses, but continues to run in background |
| 19 | CONT | Resumes execution after STOP or TSTP |

</center>

</br>
To kill a process:

    sudo kill 3940 // Use PID, Use -s to get forceful.

To kill processes by using their names and/or wildcard:

    killall http*

___
## Disk Space

<p>
To use a new media disk on the system, it must be placed in the virtual directory or in other words, be <strong>mounted</strong>.
</p>

To list currently mounted media devices:
    
    mount

To manually mount a media device:

    mount -t type device directory

To mount a USB memory stick at device __/dev/sdb1__ at location __/media/disk__:

    mount -t vfat /dev/sdb1 /media/disk

<p>
After mounting the root user has full access to the device, but access by other users is restricted, which can be changed using directory permissions.
</p>

To unmount a device:

    umount directory
or:

    umount device

To see the status of all mounted disks:

    df -h

To see disk usage for a specific directory or by default, the current directory:

    du -s // -c produces a grand totall of all files listed and -s summarizes each argument.

___
## Working with Data Files

To sort the data in a file:

    sort file1 // By default does a char sort.

"sort" parameters:

    -n: Sorts numerically.
    -M: Month sort
    -b: Ignores leading blanks
    -d: Considers only blanks and alphanumerics not special characters.
    -k  POS1[,POS2]: Sorts based on POS1, and ends at POS2 if specified.
    -o = file: Writes results to file specified.
    -r: reverses the sort order.
    -t  'SEP': Specifies field separator char.

To search for a data in the middle of a file:

    grep pattern file

"grep" parameters:

    -v: Outputs lines that don't match the pattern.
    -n: Shows the line numbers.
    -c: Shows only the count of the lines that contain the pattern.
    -e: To specify more than one matching pattern (or).

To compress a file with gzip package:

    gzip file*

To display contents of a compressed text file:

    gzcat file1

To uncompress a file:

    gunzip file1

To create a tar archive file called test.tar containing the contents of test and test2 directories:

    tar -cvf test.tar test/ test2/

To list the contents of the tar file test.tar:

    tar -tf test.tar

To extract the contents of the tar file:

    tar -xvf test.tar

<p>If the tar file was created from a dir structure, the entire dir structure is recreated starting at the current directory.

"tar" parameters:

    -A: Appends a tar file to another tar file.
    -c: Creates a new tar file.
    -d: Checks the differences between a tar file and the filesystem.
    --delete: Deletes from a tar file.
    -r: Appends files to the end of a tar file.
    -u: Appends files to an existing tar archive file that are newer than a file with the same name in the existing archive.
    -C dir: Changes to the specified dir.
    -f file1: Outputs results to file (or device) file1.
    -j: Redirects output to the bzip2 command for compression.
    -p: Preserves all file permissions.
    -v: Lists files as they are processed.
    -z: Redirects the output to the gzip command for compression.

<p>The files ending in <strong>.tgz</strong> are gzipped tar files.</p>

To extract .tgz files:

    tar -zxvf filename.tgz

___

## Subshell

To create a child shell: 

    /bin/bash

To check for child shells:

    ps -f // Using parent process ID (PPID) we can find the parent shell process.

To check subshells tree structure:

    ps --forest

To exit a (child) shell:

    exit

___
## Putting a process list in the background
To have a list of commands,in a single line, run one after another, use ";" :

    pwd ; ls ; cd /etc ; pwd ; cd ; pwd

To have a process list, use parentheses:

    (pwd ; ls ; cd /etc ; pwd ; cd ; pwd)

<p>In a process list, a subshell is spawned to execute the commands.</p>

<p>
A process list is a <strong>command grouping type</strong>. Another command grouping type is { command; } which will not create a subshell like the process list does.
</p>

To check if a subshell is spawned we can use the __Environmental variable $BASH_SUBSHELL__:

    echo $BASH_SUBSHELL

To put a command into background mode we add __&__ at the end of it:

    sleep 1000&

To display background job information:

    jobs -l

To use a processes list in background mode & can be used:

    (sleep 2; echo $BASH_SUBSHELL ; sleep)&

<p>
This way a large amount of processing within a subshell can be done without tying up the subshell's I/O.
</p>

Another example using tar:

    (tar -cf file.tar /home/tars ; tar -cf My.tar /home/mostafa)&

___
## Co-processing

We can spawn a subshell in background mode and execute a command in that subshell, This is called co-processing:

    coproc sleep 10

To see co-processing status:

    jobs

To give the process a name:

    coproc My_Job { sleep 10; } // A space must be put before and after the openning and closing curly brackets and a ; after the command. 


<p>
An <strong>external command</strong>, sometimes called a filesystem command, is a program that exists outside of the bash shell. They are typically located in <strong>/bin</strong>, <strong>/usr/bin</strong>, <strong>/sbin</strong>, or <strong>/usr/sbin</strong>.
</p>

___
## Built-in commands
<p>
Whenever an external command is executed, a child process is created. This action is called <strong>forking</strong>.
</p>

To check if a command is built-in:

    type cd

To see parent and forked child processes:

    ps -f

To see flavors of a command:

    type -a pwd

The __PATH__ environment variable defines the directory that the shell searches looking for external commands:

    echo $PATH

To add a new search directory:

    PATH=$PATH:/home/userName/scripts

A trick is going to the directory and:

    PATH=$PATH:.

<p> This is only valid until the next reboot.</p>
<p>
Remember to export the variable to global environment to be able to use it in the subshells.
</p>

To see a history of used commands:

    history

To recall and reuse the last command in the history list:

    !!

To see a list of active aliases:

    alias -p

To create an alias:

    alias li='ls -li'

<p>
Since command aliases are built-in commands, an alias is valid only for the shell process in which it is defined. Store personal alias settings in the <strong>$HOME/.bashrc</strong> startup file to make them permanent.
</p>

___
## Environment Variables

Two types of the environment variables:
* __Global variables__: Visible from the shell session and any spawned child subshells.
* __Local variables__: Only available in the shell that creates them.

To view global environment variables:

    printenv
or

    env

To display an individual environment variable's value:

    Printenv HOME

To pass the variable as acommand variable:

    echo $HOME

To see all local, global and user-defined variables for a specific process:

    set

To create a local user-defined variable, visible within the shell process:

    my_variable="Hello World"

The created variable can be referenced by $my_variable.

<p>
Be careful if you set a local variable in a child process, after you leave the child process, the local variable is no longer available.
</p>

<p>
To create a global environment variable a local variable must be created first and then exported to the global environment.
</p>

    export my_variable

<p>
Changing a global environment variable within a child shell does not affect the variable's value in the parent shell.
</p>

To remove an environment variable:

    unset my_variable

To add a string to an environment variable:

    PATH=$PATH:/home/userName/scripts

or

    PATH=$PATH:.

___
## Locating System Environment Variables

<p>
<strong>Startup files</strong> or <strong>environment files</strong> are files that by default are checked by the bash when somenone starts a bash shell. The startup files that bash processes depend on the method that we start a bash shell.
</p>

There are three ways that we can start a bash shell:

* As a default login shell at login time 
* As an interactive shell that is started by spawning a subshell
* As a non-interactive shell to run script

<p>
When you login to a linux system, the bash shell starts as a login shell. The login shell typically looks for five different startup files to process commands from:
</p>

* /etc/profile
<p>
<strong>/etc/profile</strong> has a for statement that iterates through any files in the <strong>/etc/profile.d</strong> directory. Any application-specific startup files that are executed by the shell when we log in are placed in the <strong>profile.d</strong>.
</p>

<p>
The first file found in the following ordered list is run, and the rest are ignored:
</p>

* $HOME/.bash_profile
* $HOME/.bashrc
* $HOME/.bash_login
* $HOME/.profile

<p>
Each user can edit the files and add his or her own environment variables that are active for every bash shell session they start.
</p>

If bash is started as an interactive shell, it won't process the /etc/profile, and it will only check for the file <strong>.bashrc</strong>.

The .bashrc file does two things:

1. It checks for a common bashrc file in the __/etc directory__.
2. It's a place for the user to enter __personal command aliases__ and __private script functions__.

<p>
For non-interactive subshells, we might want the system to run specific commands each time we start a script on the system. For that purpose we use the <strong>BASH_ENV</strong> environment variable.
</p>

</p>
To make it short, To create a persistent global environment variable or modify an existing one, create a file ending with .sh in the <strong>/etc/profile.d</strong>. Each user's persistent bash shell variable should be stored in the $HOME/.bashrc.
</p>

___
## Linux User and File Permission

<p>
User info is saved in /etc/passwd file. System accounts use UIDs below 500.
</p>

<p>
Passwords are held in the /etc/shadow file and only special programs are allowd to access it.
</p>

<p>
System defaults for definition of a new user is in <strong>/etc/sbin/useradd</strong>.
</p>

To see default values:

    /usr/sbin/useradd -D

Standard startup files for the bash shell environment from the template <strong>/etc/skel</strong> are automatically copied into every user's HOME dir that is created.

To remove a user along with his home dir:

    /usr/sbin/userdel -r testUser

To modify user accounts we use the following utilities:

<center>

| Command | Description |
| :-: | :-: |
| usermod | Edits user account fields, as well as specifying primary and secondary group |
| membership | passwd Changes the password for an existing user |
| chpasswd | Reads a file of login name and password pairs, and updates the passwords |
| chage | Changes the password’s expiration date |
| chfn | Changes the user account’s comment information |
| chsh | Changes the user account’s default shell |

</center>
<br>

To lock a user's account:

    usermod -L testUser

To unlock the account:

    usermod -U test User

To change password for a user:

    passwd testUser

To force a password change on the next login:

    passwd -e testUser

To do a mass password change from a file:

    chpasswd < users.txt # userid:password

To change a user's shell:

    chsh -s /bin/csh testUser

To find info ablut people on the system:

    finger name

To change finger information:

    chfn name

<p>
All the finger info is stored in the <strong>/etc/passwd</strong> file.
</p>

<p>
To manage password aging process for user accounts we use chage command according to the following table:
</p>

<center>


| Parameter | Description |
| :-: | :-: |
| -d | Sets the number of days since the password was last changed |
| -E | Sets the date the password expires |
| -I | Sets the number of days of inactivity after the password expires to lock the
account |
| -m | Sets the minimum number of days between password changes |
| -W | Sets the number of days before the password expires that a warning message appears |


</center>

## Using Linux Groups

Group information is stored in:

    /etc/group

To create a group:

    /usr/sbin/groupadd groupName

To add users to a group:

    /usr/sbin/usermod -G groupName userName

To change a groups name:

    /usr/sbin/groupmod -n newName oldName

To change a group's GID:

    /usr/sbin/groupmod -g NewGID groupName

To delete a group:

    groupdel groupName


## File Permissions

To set the default umask:
    
    umask 026

To change the security settings:

    chmod 760 file

or

    [ugoa...][[+-=][rwxXstugo...]

The first group of characters defines to whom the new permissions apply:

* u for the user
* g for the group
* o for others (everyone else)
* a for all of the above

Symbol is used to indicate whether you want to add, subtract or set the permission to the value (=).

The third symbol is the permission used for the setting.

* X assigns execute permissions only if the object is a directory or if it already had
execute permissions.
* s sets the UID or GID on execution.
* t saves program text.
* u sets the permissions to the owner’s permissions.
* g sets the permissions to the group’s permissions.
* o sets the permissions to the other’s permissions.

An example:

    chmod o+r newfile

The o+r entry adds the read permission to whatever permissions the everyone security
level already had.

To change the owner of the file:

    chown userName fileName

To change both the user and the group of a file:

    chown userName.groupName fileName

To change the default group for a file:

    chown .defaultGroup fileName

<p>
We can use the chown command recursively through subdirectories and files using -R parameter, and we can also apply the changes to any files that are symbolically linked to the file using -h parameter.
<p>

To just change the default group for a file or directory:

    chgrp groupName fileName

<p>
Three additional bits of information that Linux stores for each file and directory:
</p>

* The set user id (SUID): When a file is executed by a user, the program runs under
the permissions of the file owner.

* The set group id (SGID): For a file, the program runs under the permissions of the
file group. For a directory, new files created in the directory use the directory group
as the default group.

* The sticky bit: The file remains(sticks) in memory after the process ends.

<p>
To create a shared dir that always sets the directory group for all new files, we use the SGID.
</p>

## Managing Filesystems

A note over filesystems:

<p>
"ext" was a mimick of the Unix filesystem. Used virtual directories to handle physical devices. It uses inodes system to track info about files stored in the virtual directory. Created a separate table on each physical device, called the inode table. File size was limited to 2GB. 
</p>

<p>
"ext2" was developed to expand the basic abilities of ext and added <strong>created, modified and last accessed time</strong> values for files. File size was increased to 2TB and in later versions to 32TB. "ext2" also reduced data blocks fragmentation by allocating disk blocks in groups. Up to ext2 it was notoriously easy for the filesystem to become corrupted if anything was going to happen to the system while the inode table entry wasn't complete.
</p> 

<p>
To fix this problem, <strong>journaling filesystems</strong> were introduced. File changes were written to a journal file before being applied and the journal entry was deleted afterwards. The three methods of journalling from safest to fastest are <strong>Data mode</strong>, <strong>Ordered mode</strong> and <strong>Writeback mode</strong>.
</p>

<p>
"ext3" filesystem created a journal file to each storage device. However it didn't support recovery from accidental deletion of files, built-in data compression and file encryption.
</p>

<p>
The "ext4" filesystem supported compression and encryption and also a feature called extents which allocated space on a storage device in blocks and only stored the starting block location in the inode table saving space in the inode table. It also incorporated <strong>block preallocation</strong>.
</p>

<p>
<strong>ReiserFS</strong> allowed resizing of an active fs and uses a technique called <strong>tailpacking</strong>, which stuffs data form one file into empty space in a data block from another file. "Reiser4" has several improvements, including extremly efficient handling of small files.
</p>

<p>
Mostly found in IBM linux offerings, The <strong>JFS</strong> filesystem is a compromise between the speed of the Reiser4 and the integrity of the data mode journaling.
</p>

<p>
"XFS" journalling fs is the default on the RHEL. It uses the writeback mode of journaling and allows online resizing of the filesystem similar to the Reiser4, except it can only be expanded and not shrunk.
</p>

<p>
Copy-on-write(COW) is an alternative to to the journaling. For modifying data, a clone or writable <strong>snapshot</strong> is used. Instead of writing modified data over current data, it's put in a new fs location and even when data modification is completed, the old data is never overwritten.
</p>

</p>
The two Most popular COW file systems are ZFS and Btrfs.
</p>

To enter fdisk utility:

    fdisk /dev/sdb

"fdisk" commands:

p: Show details of the storage device.

n: Create a new partition. There can only be 4 partitions on a single storage device. We can extend that by creating multiple extended partitions and the creating primary partitions inside the extended partitions.

l: Lists the different partitions available.

w: Saves the changes to the storage device.
</br>

To inform the OS of partition table changes:

    partprobe

After creating a partition, it must be formated with a filesystem so Linux can use it. Different programs are used for formating with different filesystems.

To check if the utility is available:

    type mkfs.ext4

To format with ext4 filesystem:

    sudo mkfs.ext4 /dev/sdb1

To mount the new filesystem in the virtual directory:

    sudo mount -t ext4 /dev/sdb1 /mnt/my_partition

<p>
This way the filesystem is only termorarily mounted until the next reboot. To force Linux to automatically mount the new fs at boot time, the new filesystem must be added to the <strong>/etc/fstab</strong> file.
</p>

For ease, we use "fsck" utility which automatically uses proper utilities to check and repair most linux file systems:

    fsck options filesystem

## Managing Logical Volumes


<p>
Hard drives are called physical volumes (PV). Each PV maps to a specific physical partition created on a hard drive. Multiple PV elements are pooled together to create a volume group (VG). Logical volumes (LV) creates the partition environment for linux to create a fs, much like a physical partition. Each LV can be formatted as an ext4 filesystem and then mounted to a specific location in the virtual directory.
</p>

<p>
LVM allows us to take snapshots of an actvie logical volume without locking the files and then copy it to another device.
</p>

<p>
<strong>LVM striping</strong> allows us to write to a single file which is defined across multiple hard drives in a single logical volume, which improves disk performance but can lead to loss of a LV in case of a single drive failure.
</p>

<p>
LVM can also create a complete copy of a LV that's updated real time. This is called an <strong>LVM mirror</strong>.
</p>

To convert the physical partitions of the hard drive into th physical volume extents used by LVM using fdisk:

    fdisk ... 
    t
    8e
    w

Use the new partition to create the PV:

    sudo pvcreate /dev/sdb1

See the list of physical volumes created:

    sudo pvdisplay /dev/sdb1

Create a new volume group:

    sudo vgcreate Vol1 /dev/sdb1

To see details about the VG:

    sudo vgdisplay vol1

To create a logical volume:

    sudo lvcreate -l 100%FREE -n lvName VGname

To see the details of the logical volume:

    sudo lvdisplay VGname

To format a filesystem on the logical volume:

    sudo mkfs.ext4 /dev/VGname/lvName

To mount the volume in the virtual directory:

    sudo mount /dev/VGname/lvName /mnt/my_partition


The Linux LVM Commands:

<center>

| Command | Function |
|:---: | :---: |
| vgchange | Activates and deactivates a volume group |
| vgremove | Removes a volume group |
| vgextend | Adds physical volumes to a volume group |
| vgreduce | Removes physical volumes from a volume group |
| lvextend | Increases the size of a logical volume |
| lvreduce | Decreases the size of a logical volume |

</center>

___

## Shell Scripting Basics

To give execute permission to the user:

    chmod u+x test1

In the first line we must specify the shell we are using:

    #!/bin/bash

To tap into environment variables within the scripts:

    echo "User info for userid: $USER"
    echo UID: $UID
    echo HOME: $HOME

To assign values to user vars:

    var1=10
    var2=-57
    var3=testing
    var4="still more testing"

<p>
These user values are deleted when the shell script is completed.
</p>

To reference user values:

    days=5
    guest="Mike"
    echo "$guest checked in $days days ago"

To assign the output of a shell command to a variable:

    testVar=`date`

or 

    testVar=$(date)

To redirect(overwrite) the output from a command to a file:

    command > outputfile

To redirect(append) the output from a command to a file:

    command >> outputfile

To redirect content of a file to the input of a command:

    command < inputfile

To redirect input inline:

    wc << EOF
    string 1
    string 2
    string 3
    EOF

To use piping:

    rpm -qa | sort | less

or to save to a file:

    rpm -qa | sort > rpm.list

To do integer math operations:

    var1=$[1 + 5]

<p>
To do floating point operations we use "bc" application.
</p>

To use bc in a script:

    var1=$(echo "scale=4; 3.44 /5" | bc)

or

    var3=$(echo "scale=4; $var1 / $var2" | bc)

or for more complicated calculations:

    var5=$(bc <<
    scale = 4
    a1 = ( $var1 * $var2)
    b1 = ( $var3 * $var4)
    a1 + b1
    EOF
    )

<p>
"$?" is a special variable that holds the exit status value from the last command that executed.
</p>

Linux Exit Status Codes:

<center>

| Code | Description |
| :---: | :---: |
| 0 | Successful completion of the command |
| 1 | General unknown error |
| 2 | Misuse of shell command |
| 126 | The command can’t execute |
| 127 | Command not found |
| 128 | Invalid exit argument |
| 128+x | Fatal error with Linux signal x |
| 130 | Command terminated with Ctrl+C |
| 255 | Exit status out of range |

</center>

<p>
By default the shell script exits with the exit status of the last command in the script:
</p>

To specify an exit code:

    exit 5

or

    exit $var

___

## Using Structured Commands

### "if-then" statement

Format of the if-then statement:

    if command
    then
        commands
    fi

<p>
Instead of having a boolean variable, it checkes the exit status of the command infront of the if. If it's anything other than 1, it skips the commands after.
</p>

Alternative format of the if-then statement:

    if command;then
        commands
    fi

An example:

    if grep $testuser /etc/passwd
    then
    echo "This is my first command"
    echo "This is my second command"
    echo "I can even put in other commands besides echo:"
    ls -a /home/$testuser/.b*
    fi

### "if-then-else" statement

The format:

    if command
    then
        commands
    else
        commands
    fi

An example:

    testuser=NoSuchUser

    if grep $testuser /etc/passwd
    then
        echo "The bash files for user $testuser are:"
        ls -a /home/$testuser/.b*
        echo
    else
        echo "The user $testuser does not exist on this system."
        echo
    fi

An example for nesting ifs:

    testuser=NoSuchUser

    if grep $testuser /etc/passwd
    then
        echo "The user $testuser exists on this system."
    else
        echo "The user $testuser does not exist on this system."
        if ls -d /home/$testuser/
        then
            echo "However, $testuser has a directory."
         fi
    fi

A better structure than nested ifs is elif:

    if command1
    then
        commands
    elif command2
    then
        more commands
    fi

The same script using elif:

    testuser=NoSuchUser

    if grep $testuser /etc/passwd
    then
        echo "The user $testuser exists on this system."

    elif ls -d /home/$testuser
    then
        echo "The user $testuser does not exist on this system."
        echo "However, $testuser has a directory."
    fi

Extended elif:

    if command1
    then
        command set 1
    elif command2
    then
        command set 2
    elif command3
    then
        command set 3
    elif command4
    then
        command set 4
    fi  

We can use the test command to convert a condition into status code.

    test condition

To use test in an if-then statement:

    if test condition
    then
        commands
    fi

If test is left empty it returns non-zero exit status code:

    if test
    then
        echo "blah"
    else
        echo "This will be printed"
    fi

If test is used on an empty variable, it returns non-zero.

    if test $my_variable
    then
        echo "it will show this message if it has content"
    else
        echo " you will see this message if it doesn't has a value "

    Another way to test a condition:

        if [ condition ] # The spaces are important.
        then
            commands
        fi

### Comparisons in "test"
Example for numeric comparison:

    if [ $value1 -eq $value2 ]
    then
        echo "The values are equal"
    else
        echo "The values are different"
    fi

or

    if [ $value1 -gt 5 ]
    then
        echo "The test value $value1 is greater than 5"
    fi

<p>
When using String comparison in "test", greater than and less than symbols must be escaped or the shell uses them as redirection symbols.
</p>

Example for string comparison:

    if [ $val1 \> $val2 ]
    then
        echo "$val1 is greater than $val2"
    else
        echo "$val1 is less than $val2"
    fi

To check if the variable is non-zero in length:

    if [ -n $val1 ]

To check if the variable is zero in length:

    if [ -z $val2 ]

File comparisons:

<center>

| Comparison | Description |
| :---: | :---: |
| -d file | Checks if file exists and is a directory |
| -e file | Checks if file exists |
| -f file | Checks if file exists and is a file and not a directory |
| -r file | Checks if file exists and is readable |
| -s file | Checks if file exists and is not empty |
| -w file | Checks if file exists and is writable |
| -x file | Checks if file exists and is executable |
| -O file | Checks if file exists and is owned by the current user |
| -G file | Checks if file exists and the default group is the same as the current user " |
| file1 -nt file2 | Checks if file1 is newer than file2, Check if the file exists before using|
| file1 -ot file2 | Checks if file1 is older than file2, Check if the file exists before using |
</br>

</center>

To look before you leap:

    jump_directory=/home/arthur

    if [ -d $jump_directory ]
    then
        echo "The $jump_directory directory exists"
        cd $jump_directory
        ls
    else
        echo "The $jump_directory directory does not exist"
    fi

To combine conditions:

    [ condition1 ] && [ condition2 ]
    [ condition1 ] || [ condition2 ]

Example:

    if [ -d $HOME ] && [ -w $HOME/testing]
    then
        echo "The file exists and you can write to it"
    else
        echo "I cannot write to the file"
    fi

### Advanced if-then Features

To use advanced math formulas:

    (( expression ))

To use advanced string comparisons:

    [[ expression ]]

Example:

    if [[ $USER == r* ]]

### Case Command

Format:

    case variable in
    pattern1 | pattern2) commands1;;
    pattern3) commands2;;
    *) default commands;;
    esac

Example:

    !/bin/bash

### Using the case command

    case $USER in
    rich | barbara)
        echo "Welcome, $USER"
        echo "Please enjoy your visit";;
    testing)
        echo "Special testing account";;
    jessica)
        echo "Do not forget to log off when you're done";;
    *)
        echo "Sorry, you are not allowed here";;
    esac

___

## More Structured Commands

### "for" Command

Format:

    for var in list
    do
        commands
    done

Example:

    for test in Alabama Alaska Arizona Arkansas California Colorado
    do
        echo The next state is $test
    done
    
Reading a list from a variable:

    list="Alabama Alaska Arizona Arkansas Colorado"
    list=$list" Connecticut"

    for state in $list
    do
        echo "Have you ever been to $state?"
    done

Reading values from the output of a command:

    file="states"

    for state in $(cat $file)
    do
        echo "Come visit $state"
    done

Default IFS:

    space, tab, newline

To temporarily change the IFS environment variable:

    IFS.OLD=$IFS
    IFS=$'\n'
    //Do what you gotta do
    IFS=$IFS.OLD

Reading a Directory Using Wildcards:

    for file in /home/rich/test/.b*
    do
        if [ -d "$file" ]
        then
            echo "$file is a directory"
        elif [ -f "$file" ]
        then
            echo "$file is a file"
        fi
    done

### "while" Command

Format:

    while test command
    do
        other commands
    done

Example:

    var1=10
    while [ $var1 -gt 0 ]
    do
        echo $var1
        var1=$[ $var1 -1 ]
    done

### "until" Command

Format:

    until test commands
    do
        other commands
    done

Example:

    until echo $var1
        [ $var1 -eq 0 ]
    do
        echo inside the loop: $var1
        var1=$[ $var1 - 25 ]
    done

### Controlling the Loop

To breaks out of the loop that's currently processing ( The most inner loop ):

    break

To break out of outer loop, where n is the level starting from inside (1 is current loop):

    break n

To stop a loop iteration but not the loop itself:

    continue

To stop a loop iteration at a higher level:

    continue n

### Processing the output of a loop

To redirect the results of the loop to a file:

    done > output.txt

To redirect the results of the loop to a command:

    done > sort
___

## Handling User Input

### Passing parameters:

Positional Parameters:

    $0 # the name of the script
    $1 # first parameter up to $9
    ${10} # for more than 9 parameters
    $# # number of command line parameters included when running the cummand line.
    The last param is $params
    The last param is ${!#}
    $* all params as a single word
    $@ all params as a string of separate words
    shift # shifts command line parameters in their relative positions
    
### Working with Options
#### extracting command line options as parameters
    
    #!/bin/bash
    #
    echo
    while [ -n "$1" ]
    do
        case "$1" in
            -a) echo "Found the -a option" ;;
            -b) echo "Found the -b option" ;;
            -c) echo "Found the -c option" ;;
             *) echo "$1 is not an option" ;;
        esac
        shift
    done

___

## Input and Output

### "read" basics

At it's simplest:
    
    read VAR

Or to directly specify a prompt in read:

read -p "Please enter your age: " age

To assign data to multiple values inline:

    read -p "Enter Your Name: "  first, last

To specify a timer for user input:

    read -t 5 -p "Please enter your name: " name

To specify number of digits to enter and proceed:

    read -n1 -p "Do you want to continue [Y/N]? " answer

To prevent data being entered being displayed by matching the font color and bg color:

    read -s -p "Enter your password: " pass

### Reading from a file:

    count=1
    cat test | while read line
    do
        echo "Line $count: $line"
        count=$[ $count + 1]
    done

### To redirect STDIN, STDOUT and STDERR:

To redirect STDIN to a command:

    cat < testfile

To redirect STDOUT to a file:

    ls -l > testfile

To append:

    ls -l >> testfile

To redirect errors only:

    ls -al badfile 2> test

To redirect both:

    ls -al test badtest 2> errfile 1> outfile

To redirect both to the same file:

    ls -al test &> file

To redirect output to a specific file permanently:

    exec 1>testout

To redirect input in a script:

    exec 0< testfiel

To create an alternative file descriptor as well as STDOUT:

    exec 3>test13out
    echo "This displays on the monitor"
    echo "And this will be stored in the file" >&3
    
To append to the file instead:

    exec 3>>test13out
    echo "This displays on the monitor"
    echo "And this will appended to the file" >&3

To create a read/write file descriptor:

    exec 3<> testfile

To close a file descriptor:

    exec 3>&-

To list all file descriptors:

    /usr/sbin/lsof  -a -p $$ -d 0,1,2 # $$ means current PID

To suppress output:

    ls -al > /dev/null

To suppress error messages:

    ls -al badfile test16 2> /dev/null

To create a temporary file in a local directory:

    mktemp testing.XXXXXX   # at least 3 X

To create a temp file in the temporary directory:

    mktemp -t test.XXXXXX

To send output to both the monitor and to a file for logging:

    command | tee filename # overwrites the file
    command | tee -a filename # appends to the file

___

## Script Control

To kill a process by sending a SIGKILL:

    kill -9 PID

To trap a signal:

    trap "echo ' Ctrl-C is trapped' " SIGINT

To trap when the shell script exits:

    trap "echo Goodbye..." EXIT # This will do the commands as the shell finishes it's job

To remove traps:

    trap -- SIGINT

### Background Mode

To run a script in background mode:

    ./script &

To make the scripts keep running in bg mode even after logging off the console, we use "nohup" command.

    nohup ./test1.sh & # STDOUT and STDERR are redirected to file "nohup.out"

### Job Control

To view current jobs being handled by the shell:

    jobs -l 

Usefull parameters for jobs:

<center>

| Parameter | Description |
| :---: | :---: |
| -l | lists the PID as well |
| -n | lists only jobs that have changed stats since their last notification from the shell |
| -p | lists only the PIDs of the jobs |
| -r | lists only the running jobs |
| -s | lists only stopped jobs |

</center>

To restart a stopped job in background mode:

    bg 2 # if no PID is provided, the default job is restarted

To restart a job in foreground mode:

    fg 2

### Using "nice"

To set priority for CPU time:

    nice -n 10 ./test4.sh > test4.out &

To set priority for running processes:

    renice -n 10 -p 5055

### Using "cron"

The format of cron table:

    min hour dayofmonth month dayofweek command

Example:

    15 10 * * * command # 10:15 everyday

    15 16 * * 1 command # 4:15 pm every Monday

    15 10 * * * /home/user/test.sh > testout

<p>
We can also copy the script to one of the following directories and the cron will execute it accordingly:

    /etc/cron.daily
    /etc/cron.hourly
    /etc/cron.monthly
    /etc/cron.weekly
</p>

## Functions

To define a function:

    function fname {

    }

or

    fname() {
    commands
    }

To call functions:

    fname # use after the location where the function is defined

To make the function return a value from 0 to 255:

    return value
    return $[ $value * 2 ]
    return 2

The output of a function can be assigned to a variable:

    result='fname' # cathes output of for example echo command.

We can pass parameteres to a function:

    func1 10 $param

To create a local variable inside a function:

    local temp

To use arrays in functions:

    function testit {
        local newarrey
        newarray=(;'echo "$@"')
        echo "The new array value is : ${newarray[*]}"
    }
    myarray=(1 2 3 4 5)
    testit ${myarray[*]}

To create a function on the command line:

    function divem { echo $[ $1 / $2 ]; }

or

    function doubleit { read -p "Enter value: " value; echo $[ $value * 2 ]; }

or

    function multem {
    echo $[ $1 * $2 ]
    }

    To have the function reload by the shell each time we start a new shell we place them in the .bashrc file either by writing them directly or by sourcing from an existing library file to .bashrc:

        . /home/usr/libraries/myfuncs

## "sed" Stream Editor

Format:

    sed options script file

To replace "This" with "That" in a text file and output: 

    sed 's/This/That/' file.txt

To execute more than one editor command from the sed command line, we use the -e option:

    sed -e 's/this/that/; s/dog/cat/' data1.txt

To read sed commands from a file we use -f:

    sed -f script1.sed data1.txt

## "gawk" program

Format:

    gawk options program file

The gawk options:

-F fs: Specifies a file separator for deliminating data fields in a line
-f file: Specifies a file name to read the program from
-v var=value: defines a var and default value used in the gawk program
-mf N: Specifies the maximum number of fields to process in the data file
-mr N: Specifies the maximum record size in the data file
-W keyword: Specify the compatibility mode or warning level for gawk



Example:

    gawk '{print "Hello World!"}'

To generate and End-of-File (EOF) character in bash:

    CTRL + D

To print the first data field for each line of text:

    gawk '{print $1}' data2.txt

To specify another field separator:

    gawk -F: '{print $1}' /etc/passwd

To issue multiple command use colon:

    echo "My name is Mostafa" | gawk '{$4="Hassan"; print $0}'

To read the program from a file:

    gawk -F: -f script.gawk /etc/passwd

<p>
The gawk script runs with the BEGIN keyword and END keyword allows to specify a program script that gawk executes after reading the data.
</p>

## Regex

<p>
Different utilities have different Regex. Sed and Gawk included.
Special characters in Regex: .*[]^${}\+?|()
Anchor characters:
^   beginning of a line
$   looks at the end of a line
.   match any single character exept /n
[]  characters to match
[^] not one of these
</p>

