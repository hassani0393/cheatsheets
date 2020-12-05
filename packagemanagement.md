
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

#### bin

#### lib

#### conf

#### systemd


#### add repo
/etc/yum.repos.d contains extra repository information.
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm will install the epel repository or any other repository that the address for is give.


#### local repo

#### create repo
we can create a .repo file in /etc/yum.repo.d

#### clean up
