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

p110
