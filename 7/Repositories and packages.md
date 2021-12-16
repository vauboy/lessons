# INSTALLING AND UPDATING SOFTWARE PACKAGES

## Package repository
Repository is a collection of software hosted on a remote server and intended to be used for installing and updating software packages.

Purpose:
- Simplicity of storing and distributing software
- To be one source of updates
- Mirroring external repositories
- Temporary cache for packages

Package manager is a collection of software tools that automates the process of installing, upgrading, configuring, and removing software
YUP (YDL) -> YUM (RHEL) -> DNF (Fedora)

## RPM packages
RPM stands for Red Hat Package Manager.Package manager and fortmat.

RPM was developed by Red Hat and is primarily used on Red Hat-based Linux operating systems (Fedora, CentOS, RHEL, etc.).
An RPM package uses the .rpm extension and is a bundle (a collection) of different files. It can contain the following:
    - Binary files, also known as executables (nmap, stat, xattr, ssh, sshd, and so on)
    - Configuration files (sshd.conf, updatedb.conf, logrotate.conf, etc.)
    - Documentation files (README, TODO, AUTHOR, etc.)

## RPM naming convention
The name of an RPM package follows this format <name>-<version>-<release>.<arch>.rpm
- name is a name describing the packaged software
    ncurses and ncurses-devel
- version is the version of the packaged software
- release is the number of times this version of the software has been packaged. Normally, the rebuilds are due to bugs uncovered after the package has been in use for a while
- architecture is:
    * i386,x86_64 - The Intel x86 family of microprocessors, starting with the 80386.
    * sparc - Sun Microsystems' SPARC series of chips.
    * mips - MIPS Technologies' processors.
    * ppc - The Power PC microprocessor family.
    * noarch - Do not depend on a particular CPU architecture
    * src/nosrc. Both of these strings indicate the file is an RPM source package. The nosrc string means that the file contains only package building files, while the src string means the file contains the necessary package building files and the software's source code.

Packages with -devel appender contain the related development files such as headers, etc.

## Package Information
Package contains the following information:
- The date and time the package was built.
- A description of the package's contents.
- The total size of all the files installed by the package.
- Information that allows the package to be grouped with similar packages.
- A digital "signature" that can be used to verify the authenticity and integrity of the package. The command rpm -K (--checksig is equivalent) verifies a package file.

```bash
[vault@centos ~]$ rpm -qi wget
Name        : wget
Version     : 1.14
Release     : 18.el7_6.1
Architecture: x86_64
Install Date: Mon Dec  6 14:26:48 2021
Group       : Applications/Internet
Size        : 2055573
License     : GPLv3+
Signature   : RSA/SHA256, Thu May 16 11:48:07 2019, Key ID 24c6a8a7f4a80eb5
Source RPM  : wget-1.14-18.el7_6.1.src.rpm
Build Date  : Wed May 15 17:02:02 2019
Build Host  : x86-02.bsys.centos.org
Relocations : (not relocatable)
Packager    : CentOS BuildSystem <http://bugs.centos.org>
Vendor      : CentOS
URL         : http://www.gnu.org/software/wget/
Summary     : A utility for retrieving files using the HTTP or FTP protocols
Description :
GNU Wget is a file retrieval utility which can use either the HTTP or
FTP protocols. Wget features include the ability to work in the
background while you are logged out, recursive retrieval of
directories, file name wildcard matching, remote file timestamp
storage and comparison, use of Rest with FTP servers and Range with
HTTP servers to retrieve files over slow or unstable connections,
support for Proxy servers, and configurability.
```

- List package files:
    * -l - all
    * -c - config files
    * -d - documentation
    * --scripts - shell scripts

```bash
[vault@centos ~]$ rpm -ql wget
/etc/wgetrc
/usr/bin/wget
/usr/share/doc/wget-1.14
/usr/share/doc/wget-1.14/AUTHORS
/usr/share/doc/wget-1.14/COPYING

[vault@centos ~]$ rpm -qc wget
/etc/wgetrc
```

- Query package owning FILE (-f):
```bash
[vault@centos ~]$ rpm -qf /usr/share/info/wget.info.gz
wget-1.14-18.el7_6.1.x86_64
```

- Install package files
```bash
[vault@centos ~]$ sudo rpm -ivh wget-1.14-18.el7_6.1.x86_64.rpm --nodeps
Preparing...                          ################################# [100%]
Updating / installing...
   1:wget-1.14-18.el7_6.1             ################################# [100%]
```

- Remove package
```bash
[vault@centos ~]$ sudo rpm -e wget
[vault@centos ~]$ rpm -qi wget
package wget is not installed
```

- Check gpg sign
```bash
[vault@centos ~]$ rpm -K wget-1.14-18.el7_6.1.x86_64.rpm
wget-1.14-18.el7_6.1.x86_64.rpm: rsa sha1 (md5) pgp md5 OK
```

## YUM package manager
YUM - Yellowdog Updater Modified

YUM  is  an  interactive,  rpm  based,  package manager.

Use the yum utility to manage package operations:
- Searching information about packages
- Installing packages
- Updating packages
- Removing packages
- Checking the list of currently available repositories
- Adding or removing a repository
- Enabling or disabling a repository

Configuration and directories:
- /etc/yum.conf
- /etc/yum.repos.d/
- /var/cache/yum/

## YUM working with packages
General options:
```
-y, --assumeyes
    Assume yes; assume that the answer to any question which would be asked is yes.

-q, --quiet
    Run without output.  Note that you likely also want to use -y.

-v, --verbose
    Run with a lot of debugging output

-C, --cacheonly
    Tells yum to run entirely from system cache; does not download or update metadata

--version
        Reports the yum version number and installed package versions for everything in history_record_packages

--downloadonly
    Don't update, just download

--downloaddir=directory
    Specifies an alternate directory to store packages.
```

Important commands
* list
    - all. List all packages
    - available. List packages available to be installed.
    - updates. List available packages with updates.
    - installed. List installed packages
    - extras. List the packages installed on the system that are not available in any yum repository listed in the config file.

* info
    Is used to list a description and summary information about available packages

* search
    This is used to find packages when you know something about the package but aren't sure of it's name. By default search will try searching just package names and summaries, but if that "fails" it will then try descriptions and url. You can force searching everything by specifying "all" as the first argument

* install
    Is used to install the latest version of a package or group of packages while ensuring that all dependencies are satisfied.
    If no package matches the given package name(s), they are assumed to be a shell glob and any matches are then installed
    If  the  name  is  a  file,  then  install works like localinstall

* update
    If  run  without  any  packages, update will update every currently installed package.
    If one or more packages or package globs are specified, Yum will only update the listed packages
    check-update
            Implemented so you could know if your machine had any updates that needed to be applied without running it interactively. Returns exit value of 100 if  there
            are  packages  available  for an update. Also returns a list of the packages to be updated in list format. Returns 0 if no packages are available for update.
            Returns 1 if an error occurred

* remove or erase
    Are  used  to  remove  the  specified packages from the system as well as removing any packages which depend on the package being removed. remove operates on
    groups, files, provides and filelists just like the "install" command. You can't accidentally remove yum itself.
    autoremove
            With one or more arguments this command works like running the "remove" command with the clean_requirements_on_remove turned on. However you can also specify
            no arguments, at which point it tries to remove any packages that weren't installed explicitly by the user and which aren't required by anything  (so  called
            leaf packages)

* history
    View and use yum transactions
    - yum history list
        List all yum install, update and erase actions
    - yum history info 3
        Show details of yum transaction 3
    - yum history undo 3
        Undo the yum action from transaction 3
    - yum history redo 3
        Redo the undone yum action from transaction 3

* provides or whatprovides
    Is used to find out which package provides some feature or file. Just use a specific name or a file-glob-syntax wildcards to list the packages  available  or installed that provide that feature or file.

* check
    Checks the local rpmdb and produces information on any problems it finds

* check-update
    Implemented so you could know if your machine had any updates that needed to be applied without running it interactively.

## YUM groups
* What is the yum groups?
    On CentOS/RHEL, you can either install packages individually or install multiple packages in a single operation in a group.
    Package group contain packages that perform related tasks such as development tools, web server (for example LAMP or LEMP), desktop (a minimal desktop that can as well be employed as a thin client) and many more
* How to list, install, update, remove groups of packages from yum?
    - yum group list
        is used to list the available groups from all yum repos

    - yum group info
        Before you proceed to install a group of packages, you can view the group ID, a short description of the group and the various packages it contains under different categories

    - yum group install
        is  used  to  install all of the individual packages in a group, of the specified types

    - yum group update
        This will install packages in the group not already installed and upgrade existing packages

    - yum group remove
        is used to remove all of the packages in a group

## Working with yum repository
* Yum repositories info
    yum repolist [all|enabled|disabled] name  Display enabled software repositories
    yum repoinfo [all|enabled|disabled] name  Display information about enabled yum repositories *

* enable and disable red hat repositories
    --enablerepo=extras
        Enables specific repositories by id or glob that have been disabled in the configuration file using the enabled=0 option.

    --disablerepo=extras
        Disables specific repositories by id or glob.

    yum --disablerepo="*" --enablerepo="extras" list available

* adding third-party repository
    Jenkins

    yum-config-manager --add-repo repository_url
    yum-config-manager --enable repository
    yum-config-manager --disable repository

    Yum collects all repository information from .repo files and the [repository] section of the /etc/yum.conf file to create a master list of repositories to use for transactions.

    Yum uses the default directory /etc/yum.repos.d/.

    ```
    [base]
    name=CentOS-$releasever - Base
    mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
    #baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    ```

    - name=repository_name
        Here, repository_name is a human-readable string describing the repository.

    - baseurl=repository_url
        Replace repository_url with a URL to the directory where the repodata directory of a repository is located:
            - If the repository is available over HTTP, use: http://path/to/repo
            - If the repository is available over FTP, use: ftp://path/to/repo
            - If the repository is local to the machine, use: file:///path/to/local/repo
            - If a specific online repository requires basic HTTP authentication, you can specify your user name and password by prepending it to the URL as username:password@link

    - enabled=value
        This is a simple way to tell yum to use or ignore a particular repository, value is one of:
        0 - Do not include this repository as a package source when performing updates and installs.
        1 - Include this repository as a package source.

    - async=value
        Controls parallel downloading of repository packages. Here, value is one of:
        - auto (default) - parallel downloading is used if possible.
        - on - parallel downloading is enabled for the repository.
        - off - parallel downloading is disabled for the repository.

* GPG keys for repositories
    The GNU Privacy Guard is a complete and free implementation of the OpenPGP standard.
    Each stable RPM package that is published by CentOS Project is signed with a GPG signature.
    The Project GPG keys are included in the centos-release package, and are typically found in /etc/pki/rpm-gpg

    - gpgcheck=value
        where value is one of:
        0 — Disable GPG signature-checking on packages in all repositories, including local package installation.
        1 — Enable GPG signature-checking on all packages in all repositories, including local package installation. gpgcheck=1 is the default, and thus all packages' signatures are checked.

    --nogpgcheck
        Run with GPG signature checking disabled

* Yum Variables
    You can use and reference the following built-in variables in yum commands and in all yum configuration files (that is, /etc/yum.conf and all .repo files in the /etc/yum.repos.d/ directory):
    - $releasever
        The release version of Centos.
        Yum obtains the value of $releasever from the distroverpkg=value line in the /etc/yum.conf configuration file.

    - $arch
        The system's CPU architecture as returned when calling Python's os.uname() function.
        Valid values for $arch include: i586, i686 and x86_64.

    - $basearch
        The base architecture of the system. For example, i686 and i586 machines both have a base architecture of i386, and AMD64 and Intel 64 machines have a base architecture of x86_64.

    - $YUM0-9
        These ten variables are each replaced with the value of any shell environment variables with the same name.

* yum log
    - Where to find yum log file?
    - How to reverse yum transaction?

## YUM downloader
* YUM provides possibility for downloading packages:

```bash
yum install --downloadonly --downloaddir=directory package
```

Do not use for "yum groupinstall"
If only the package name is specified, the latest available package is downloaded (such as sshd). Otherwise, you can specify the full package name and version (such as httpd-2.2.3-22.el5).
If you do not use the --downloaddir option, files are saved by default in /var/cache/yum/ in rhel-{arch}-channel/packages
If desired, you can download multiple packages on the same command

* yumdownloader (need to install yum-utils):

```bash
$ yum install yum-utils
$ yumdownloader wget
```

The package is saved in the current working directly by default
Use the --destdir option to specify an alternate location

Be sure to add --resolve if you need to download dependencies.

## YUM cache
Note that these commands only operate on the currently enabled repositories within the current cachedir.  To  clean  specific  repositories, use --enablerepo, --disablerepo or --releasever accordingly.  Note, however, that untracked (no longer configured) repositories cannot be cleaned this way; they have to be removed manually.

* yum clean packages
    Eliminate any cached packages from the system.  Note that packages are not automatically deleted after they are downloaded.

* yum clean rpmdb
    Eliminate any cached data from the local rpmdb.

* yum clean all
    Does  all  of  the above.  As a convenience, if this command does not result in a completely empty cache due to the restrictions outlined at the beginning of this section, a message will be printed, saying how much disk space can be reclaimed by cleaning the remaining repos manually.  For this purpose, a  repo  is considered clean when its disk usage doesn't exceed 64KB (that is to account for directory entries and tiny metadata files such as "productid" that are never cleaned).


---

* [RHEL7 system administrators guide](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-yum#sec-Configuring_Yum_and_Yum_Repositories)
* [RedHat yum cheatsheet](https://access.redhat.com/sites/default/files/attachments/rh_yum_cheatsheet_1214_jcs_print-1.pdf)
* [CentOS repositories](https://wiki.centos.org/AdditionalResources/Repositories)
* [EPEL](https://docs.fedoraproject.org/en-US/epel/)
* [Create local repo](https://wiki.centos.org/HowTos/CreateLocalRepos)
* [RPM Maximum](http://ftp.rpm.org/max-rpm/index.html)
* [RPM Packaging](https://rpm-packaging-guide.github.io/)
* [Create rpm-package](https://www.redhat.com/sysadmin/create-rpm-package)
* [CVE log4j2](https://www.cve.org/CVERecord?id=CVE-2021-44228)
* [CentOS Keys](https://www.centos.org/keys/)