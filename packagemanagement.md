
## Package management

#### yum
By default yum deletes downloaded files when they aren't needed anymore. but by caching we can store downloaded files.
temp files storage: /var/cache/yum/$basearch/$releasever/ $base architecture  $releaseversion of redhat linux
to change defualt cache location modify cachedir in /etc/yum.conf
Global yum conf options /etc/yum.conf
reads repo info /etc/yum.repos.d
caches latest repo info in /var/cache/yum
dnf to replace yum in fedora and uses same syntax,

yum update : searches online repos for updated packs compare to what is installed on the system and upgrades packages.

yum search : searches the yup repos for specific pack

yum info : lists info about a specific pack

yum list installed : displays all installed pack , use | less for ease.

yum clean all : clean yum db and cache for fresh and curroption removal

yum install : install a specific pack and all of its dep's

yum remove : uninstalls a pack, leaves deps

yum autoremove : like remove but also deps

yum whatprovided : find out what pack provides a specific file name

yum reinstall : reinstalls a specific pack

yum-utils is package that allows for more utilities such as downloading packs whithout installation.

using above: yumdownloader *name of pack* : downloads without installation

yum makecache : 


#### rpm
a more complex packaging to make up shortcommings of simple tarballs,
rpm database located in /var/lib/rpm and uses "rpm --rebuilddb" command to repair a corrupted rpm database.
does not handle deps.

rpm --builddb : repairs a currupted rpm db.

rpm -qpi : qurry, particular package, information, find out more information about a certain rpm package

rpm -qa : querry all packages on the system

sudo rpm -ivh : install, verbose, hashmark(progress bar like) , installs a specific package

sudo rpm -U : upgrades and installed package with a newer version.

sudo rpm -e : erase, uninstalls an installed package

rpm -Va : verify all installed packages

rpm2cpio : converts a .rpm file to a cpio archive file

#### Source Installation

bunzip2 thefile.tar.bz2 :unzips the file.

tar xvf thefile.tar : untars the file.

sudo apt-get install build-essential git : tools needed.

configure : probes the system to findout information needed about installation.




#### Pip

#### Snapp
snapd daemon is the background service that manages and maintains the snap environment on a linux system.Also provides the snap command.
yum install snapd

sudo systemctl enable --now snapd.socket

to check if snapd is active and enabled :
sudo systemctl is-active snapd.socket
sudo systemctl status snapd.socket
sudo systemctl is-enabled snapd.socket

enable classic snap support by creating a symbolic link between /var/lib/snapd/snap and /snap 
sudo ln -s /var/lib/snapd/snap /snap

snap version : to check version of the snapd and snap command line tool installed on the system.

snap find "category" : to check snap store for finding snaps in a cetrain category.

snap info nameOfSnap : checks both snap store and installed snaps for information about the snap.

sudo snap install nameOfSnap : installs the snap 

To install from a different channel: edge, beta, or candidate, use the --edge, --beta, or --candidate options respectively. Or use the --channel option and specify the channel you wish to install from.

snap list : summary of installed snaps

sudo snap refresh mailspring : updates mailspring snap

sudo snap refresh	:update all snaps on the local system

sudo snap revert mailspring : reverts the snap mailspring to the previously used version. Note that the data associated with it will also revert.

sudo snap disable mailspring : desables the snap mailspring

sudo snap enable mailspring : enables the specified snap

sudo snap remove mailspring : removes the snap completely.



#### bin, lib, conf

##### bin
/bin : Essential user command binaries (for use by all users)
Purpose
/bin contains commands that may be used by both the system administrator and by users, but which are required when no other filesystems are mounted (e.g. in single user mode). It may also contain commands which are used indirectly by scripts.

##### /usr/bin
/usr/bin : Most user commands
Purpose
This is the primary directory of executable commands on the system.

##### lib
/lib : Essential shared libraries and kernel modules
Purpose
The /lib directory contains those shared library images needed to boot the system and run the commands in the root filesystem, ie. by binaries in /bin and /sbin.

##### /usr/lib : Libraries for programming and packages
Purpose
/usr/lib includes object files, libraries, and internal binaries that are not intended to be executed directly by users or shell scripts.

Applications may use a single subdirectory under /usr/lib. If an application uses a subdirectory, all architecture-dependent data exclusively used by the application must be placed within that subdirectory.

##### /usr/src : 
Source code (optional)
Purpose
Source code may be placed in this subdirectory, only for reference purposes. 

/etc : Host-specific system configuration
Purpose
The /etc hierarchy contains configuration files. A "configuration file" is a local file used to control the operation of a program; it must be static and cannot be an executable binary.

Requirements
No binaries may be located under /etc.

The following directories, or symbolic links to directories are required in /etc:

Directory	Description
opt	Configuration for /opt
X11	Configuration for the X Window system (optional)
sgml	Configuration for SGML (optional)
xml	Configuration for XML (optional)
Specific Options
The following directories, or symbolic links to directories must be in /etc, if the corresponding subsystem is installed:

Directory	Description
opt	Configuration for /opt
The following files, or symbolic links to files, must be in /etc if the corresponding subsystem is installed:

File	Description
csh.login	Systemwide initialization file for C shell logins (optional)
exports	NFS filesystem access control list (optional)
fstab	Static information about filesystems (optional)
ftpusers	FTP daemon user access control list (optional)
gateways	File which lists gateways for routed (optional)
gettydefs	Speed and terminal settings used by getty (optional)
group	User group file (optional)
host.conf	Resolver configuration file (optional)
hosts	Static information about host names (optional)
hosts.allow	Host access file for TCP wrappers (optional)
hosts.deny	Host access file for TCP wrappers (optional)
hosts.equiv	List of trusted hosts for rlogin, rsh, rcp (optional)
hosts.lpd	List of trusted hosts for lpd (optional)
inetd.conf	Configuration file for inetd (optional)
inittab	Configuration file for init (optional)
issue	Pre-login message and identification file (optional)
ld.so.conf	List of extra directories to search for shared libraries (optional)
motd	Post-login message of the day file (optional)
mtab	Dynamic information about filesystems (optional)
mtools.conf	Configuration file for mtools (optional)
networks	Static information about network names (optional)
passwd	The password file (optional)
printcap	The lpd printer capability database (optional)
profile	Systemwide initialization file for sh shell logins (optional)
protocols	IP protocol listing (optional)
resolv.conf	Resolver configuration file (optional)
rpc	RPC protocol listing (optional)
securetty	TTY access control for root login (optional)
services	Port names for network services (optional)
shells	Pathnames of valid login shells (optional)
syslog.conf	Configuration file for syslogd (optional)
mtab does not fit the static nature of /etc: it is excepted for historical reasons.
___
#### systemd
Replaces shell scripts with compiled C code, but still 99% compatible with older System V init scripts.

It uses unit files located at: /usr/lib/systemd/system (Do not edit! They can be modified by package updates.)  
                              /etc/systemd/system (unit files here takes precedence to other unit files. Safest place to modify.)
                               Runtime unit files: /run/systemd/system

systemd still checks /sbin/init

systemctl list-unit-files : lists all system's unit files by different sections.

systemctl cat name.unit : This will print out the contents of the unit file specified.


#### add repo
/etc/yum.repos.d contains extra repository information.
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm will install the epel repository or any other repository that the address for is give.


#### local repo

#### create repo
we can create a .repo file in /etc/yum.repo.d

#### clean up
