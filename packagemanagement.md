# Package management

## YUM
___
* By default yum deletes downloaded files when they aren't needed anymore. However by caching we can store downloaded files.

 Temporary files storage:

     /var/cache/yum

To change defualt cache location modify cachedir in:

    /etc/yum.conf

Global YUM configurations options:

     /etc/yum.conf

Reads repo info from:

     /etc/yum.repos.d

Caches latest repo info in:

     /var/cache/yum

To see what repositories you're pulling software from:

    yum repolist



 Searches online repos for updated packs compare to what is installed on the system and upgrades packages:

    yum update

Searches the YUM repos for specific pack:

    yum search 

Lists info about a specific pack:

    yum info  packName

    yum list packName

Displays all installed pack , use | less for ease.

    yum list installed: 

Clean YUM DB and cache for refreshment and curroption removal

    yum clean all: 

To skip the package with the broken dep and update the other software packages:

    yum update --skip-broken

Install a specific pack and all of its deps:

    yum install 

To install a local rpm installation fiel:

    yum localinstall package_name.rpm

Uninstalls a pack, leaves deps:

    yum remove

Like remove but also with deps:

    yum autoremove 

Find out what pack provides a specific file name:

    yum whatprovided

Reinstalls a specific pack

    yum reinstall : 

yum-utils is a package that allows for more utilities such as downloading packs whithout installation. Using yum utils this commands downloads without installation:

    yumdownloader *name of pack* : 

## RPM
___
a more complex packaging to make up shortcommings of simple tarballs,
RPM database located in:

    /var/lib/rpm 

* RPM does not handle deps.

To repair a corrupted RPM DB:

    rpm --builddb 

To find out more information about a certain RPM package:


    rpm -qpi //qurry, particular package, information 

To querry all packages on the system:

    rpm -qa

To install a specific package:

    rpm -ivh //install, verbose, hashmark(progress bar like) 

To upgrade and install package with a newer version.

    rpm -U  

To uninstall an installed package

    rpm -e //erase, 

    rpm -Va : verify all installed packages

rpm2cpio : converts a .rpm file to a cpio archive file

## Source Installation
___
To unzip the file:

    bunzip2 thefile.tar.bz2 :

To untar the file:

    tar xvf thefile.tar : 

To install tools needed:

    sudo apt-get install build-essential git

To install a source file to a destination directory:

    

## Pip

To check if Python is installed:

    python --version

To check if pip is available:

    pip --version

To install the latest version of “SomeProject”:

    pip install "SomeProject"

To install a specific version:

    pip install "SomeProject==1.4"

Upgrade an already installed SomeProject to the latest from PyPI:

    pip install --upgrade SomeProject

## Snapp
___
To install snap daemon:

    yum install snapd

To enable snap:

    sudo systemctl enable --now snapd.socket

To check if snapd is active and enabled:

    sudo systemctl is-active snapd.socket
    sudo systemctl status snapd.socket
    sudo systemctl is-enabled snapd.socket

Enable classic snap support by creating a symbolic link between __/var/lib/snapd/snap__ and __/snap__:

    sudo ln -s /var/lib/snapd/snap /snap

To check version of the snapd and if the snap command line tool is installed on the system:

    snap version

To check snap store for finding snaps in a cetrain category:

    snap find "category"
 
To check both snap store and installed snaps for information about a snap:

    snap info nameOfSnap

To installs a snap: 

    snap install nameOfSnap :

* To install from a different channel, like edge, beta, or candidate, use the --edge, --beta, or --candidate options respectively. Or use the --channel option and specify the channel you wish to install from.

To get a summary of installed snaps:

    snap list 

To updates mailspring snap:

    snap refresh mailspring 

To update all snaps on the local system:
    
    snap refresh

To revert the snap mailspring to the previously used version:

    snap revert mailspring

* Note that the data associated with it will also revert.

To disable the snap mailspring:

    snap disable mailspring

To enable snap

    snap enable mailspring : 

To remove snap completely:

    snap remove mailspring 

## systemd
___
Replaces shell scripts with compiled C code, but still 99% compatible with older System V init scripts.

It uses unit files located at:
    
    /usr/lib/systemd/system 
    
* Do not edit! They can be modified by package updates.  

Safest place to modify

    /etc/systemd/system 
    
Unit files here takes precedence to other unit files.

Runtime unit files: 
    
    /run/systemd/system

systemd still checks:
    
    /sbin/init

To list all system's unit files by different sections:

    systemctl list-unit-files

To print out the contents of the unit file specified:

    systemctl cat name.unit 


## create repo
___
* we can create a .repo file in /etc/yum.repo.d

Install createrepo software pack:

    sudo yum install createrepo

Install yum-utils:

    sudo yum install yum-utils

Create a directory for an HTTP repository using:

    sudo mkdir –p /var/www/html/repos/{base,centosplus,extras,updates}

To create an FTP directory:

    sudo mkdir –p /var/ftp/repos

Download a local copy of the official CentOS repositories to your server. This allows systems on the same network to install updates more efficiently.

To download the repositories:

    sudo reposync -g -l -d -m --repoid=base --newest-only --download-metadata --download_path=/var/www/html/repos/

    sudo reposync -g -l -d -m --repoid=centosplus --newest-only --download-metadata --download_path=/var/www/html/repos/
    
    sudo reposync -g -l -d -m --repoid=extras --newest-only --download-metadata --download_path=/var/www/html/repos/

    sudo reposync -g -l -d -m --repoid=updates --newest-only --download-metadata --download_path=/var/www/html/repos/

    
Options in the previous commands:

    –g: lets you remove or uninstall packages on CentOS that fail a GPG check

    –l: yum plugin support

    –d: lets you delete local packages that no longer exist in the repository

    –m: lets you download comps.xml files, useful for bundling groups of packages by function

    ––repoid: specify repository ID

    ––newest-only: only download the latest package version, helps manage the size of the repository

    ––download-metadata: download non-default metadata

    ––download-path: specifies the location to save the packages

To create a new Repo:

    sudo createrepo /var/www/html //for HTTP

    sudo createrepo /var/ftp //for FTP

To setup a local YUM repository on a client machine:

    sudo vim /etc/yum.repos.d/remote.repo

In the file enter:

<code>

[remote]

name=RHEL Apache

baseurl=http://192.168.1.10 //Replace with IP address of your sever.

enabled=1

gpgcheck=0

</code>

If configuring for FTP use this instead:

<code>

[remote] 

name=RHEL FTP

baseurl=ftp://192.168.1.10 //Replace with IP address of your sever.

enabled=1

gpgcheck=0

</code>