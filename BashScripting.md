# Bash Scripting

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
## Processses

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

To see a history of used commands:

    history

To recall and reuse the last command in the history list:

    !!

To see a list of active aliases:

    alias -p

To create an alias:

    alias li='ls -li'

<p>
Since command aliases are built-in commands, an alias is valid only for the shell process in which it is defined.
</p>

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

