## Repositories and Packages

- Use rpm for the following tasks:
1. Download sysstat package.
2. Get information from downloaded sysstat package file.
3. Install sysstat package and get information about files installed by this package.

- Add NGINX repository (need to find repository config on https://www.nginx.com/) and complete the following tasks using yum:
1. Check if NGINX repository enabled or not.
2. Install NGINX.
3. Check yum history and undo NGINX installation.
4. Disable NGINX repository.
5. Remove sysstat package installed in the first task.
6. Install EPEL repository and get information about it.
7. Find how much packages provided exactly by EPEL repository.
8. Install ncdu package from EPEL repo.

*Extra task:
    Need to create an rpm package consists of a shell script and a text file. The script should output words count stored in file.

## Work with files

1. Find all regular files below 100 bytes inside your home directory.
2. Find an inode number and a hard links count for the root directory. The hard link count should be about 17. Why?
3. Check what inode numbers have "/" and "/boot" directory. Why?
4. Check the root directory space usage by du command. Compare it with an information from df. If you find differences, try to find out why it happens.
5. Check disk space usage of /var/log directory using ncdu