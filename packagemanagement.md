
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
