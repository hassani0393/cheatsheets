
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

#### Source Installation

#### Pip

#### Snapp

#### bin

#### lib

#### conf

#### systemd

#### add repo

#### local repo

#### create repo

#### clean up