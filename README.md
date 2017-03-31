[![Travis branch](https://img.shields.io/travis/ccztux/glsysbackup/master.svg?label=build%20%28master%29)](https://travis-ci.org/ccztux/glsysbackup)
[![Travis branch](https://img.shields.io/travis/ccztux/glsysbackup/devel.svg?label=build%20%28devel%29)](https://travis-ci.org/ccztux/glsysbackup)
[![Latest Release](https://img.shields.io/github/release/ccztux/glsysbackup.svg?label=latest%20release)](https://github.com/ccztux/glsysbackup/releases/latest)
[![Latest Pre-release](https://img.shields.io/badge/latest%20pre--release-v1.0.1--beta1-orange.svg)](https://github.com/ccztux/glsysbackup/releases/tag/1.0.1-beta1)
[![GitHub license](https://img.shields.io/badge/license-AGPL-blue.svg)](https://github.com/ccztux/glsysbackup/blob/master/LICENSE)



# glsysbackup
(**g**eneric **l**inux **sys**tem **backup**) is an advanced backup tool written in bash.



## Example help output:
```
20:10:41 [root@localhost]:~$ ./glsysbackup -h
[2017-03-28 20:10:41] glsysbackup: [8641] glsysbackup 1.0.1-beta1 starting... (PID=8641)
[2017-03-28 20:10:41] glsysbackup: [8641] Getting options...

Usage: glsysbackup OPTIONS

Author:			Christian Zettel (ccztux)
Last modification:	2017-03-28
Version:		1.0.1-beta1

Description:		glsysbackup (Generic Linux System Backup) is an advanced backup tool written in bash.

OPTIONS:
   -h		Shows this help.
   -o		Override lock in case glsysbackup was terminated abnormally.
   -v		Shows detailed version information.

[2017-03-28 20:10:41] glsysbackup: [8641] Caught: 'EXIT', exiting script...
[2017-03-28 20:10:41] glsysbackup: [8641] We hope you are informed better now. :P This was a lazy job. :)
[2017-03-28 20:10:41] glsysbackup: [8641] Script was running: '0' seconds.
[2017-03-28 20:10:41] glsysbackup: [8641] Bye, bye...
```



## Configuration variables:
```bash
#-------------------------
# Configuration variables:
#-------------------------

#---------
# Logging:
#---------

# enable log to file. (possible values: 1|0)
log_to_file="0"

# log directory
log_directory="/var/log/"

# filename of logfile
log_filename="${script_name}.log"

# enable log to stdout (possible values: 1|0)
log_to_stdout="1"

# enable log to system logfile (possible values: 1|0)
log_to_syslog="1"

# timestamp format for log messages.
log_timestamp_format="%Y-%m-%d %H:%M:%S"



#------------
# Privileges:
#------------

# enable this to ensure glsysbackup is running with root privileges (possible values: 1|0)
root_privileges_required="1"



#--------
# Renice:
#--------

# enable reniceing of glsysbackup and child procs (possible values: 1|0)
re_nice_required="0"

# set renice priority (HINT: have a look at: 'man renice')
re_nice_priority="19"



#-----------
# Re-ionice:
#-----------

# enable re-ioniceing of glsysbackup and child procs (possible values: 1|0)
re_ionice_required="0"

# set re-ionice scheduling class (HINT: have a look at: 'man ionice')
re_ionice_scheduling_class="2"

# set re-ionice priority (HINT: have a look at: 'man ionice')
re_ionice_priority="7"



#----------
# Rotation:
#----------

# enable backup rotation (possible values: 1|0)
backup_rotation_required="1"

# number of backup files to keep
backup_rotation_files_to_keep="5"



#--------------------
# Installed packages:
#--------------------

# enable the creation of installed packages file (possible values: 1|0)
installed_packages_required="1"

# force this package manager to create installed packages file, if you have more than one package manager installed (possible values: rpm|dpkg|pacman|equery|pkgutil)
installed_packages_forced_manager=""

# file name of installed packages
installed_packages_filename="/root/installed_syspackages.txt"



#--------
# Backup:
#--------

# enable backup compression (possible values: 1|0)
backup_compression_enabled="1"

# backup compression type (possible values: gzip|bzip2|xz|lzip|lzma|lzop)
backup_compression_type="gzip"

# enable backup verbose mode
backup_verbose_mode_enabled="1"

# set backup destination path
backup_destination_path="/var/backups/"

# set backup filename
backup_filename="${script_name}.${script_hostname}.tar.gz"

# files and folders you want to backup
backup_items=(
"/etc/"
"/home/"
"/root/"
"/var/lib/mpd/"
"/usr/local/bin/"
"/boot/config.txt"
)

# exclude this items from backup (HINT: have a look at: 'man tar')
backup_exlude_items=(
"/old.backups"
"/old.mysqldumps/"
)



#------------
# Encryption:
#------------

# enable backup encryption with openssl (possible values: 1|0)
backup_encryption_required="1"

# set password for encryption
backup_encryption_password="test1234"




#-------------------
# Pre backup script:
#-------------------

# enable pre backup script functionality (possible values: 1|0)
pre_backup_script_required="1"

# path to pre backup script 
pre_backup_script="/home/pi/pre.sh"

# exit glsysbackup in case execution of pre backup script was not successful
pre_backup_exit_when_unsuccessful="1"



#--------------------
# Post backup script:
#--------------------

# enable post backup script functionality (possible values: 1|0)
post_backup_script_required="1"

# path to post backup script 
post_backup_script="/home/pi/post.sh"

# exit glsysbackup in case execution of post backup script was not successful
post_backup_exit_when_unsuccessful="1"
```



## Example job output:
```
20:12:52 [pi@localhost]:~$ sudo ./glsysbackup 
[2017-03-28 20:12:55] glsysbackup: [17864] glsysbackup 1.0.1-beta1 starting... (PID=17864)
[2017-03-28 20:12:55] glsysbackup: [17864] Check if root priviliges are required...
[2017-03-28 20:12:55] glsysbackup: [17864] Root privileges are required, checking privileges...
[2017-03-28 20:12:55] glsysbackup: [17864] HOORAY, we have root privileges. :)
[2017-03-28 20:12:55] glsysbackup: [17864] Get user which starts the script...
[2017-03-28 20:12:55] glsysbackup: [17864] glsysbackup was started by: 'pi'.
[2017-03-28 20:12:55] glsysbackup: [17864] Checking bash version...
[2017-03-28 20:12:55] glsysbackup: [17864] Bash version: '4' meets requirements.
[2017-03-28 20:12:55] glsysbackup: [17864] Check if another instance of: 'glsysbackup' is already running...
[2017-03-28 20:12:55] glsysbackup: [17864] Check if lock file: '/var/lock/glsysbackup' exists and if it is read and writeable...
[2017-03-28 20:12:55] glsysbackup: [17864] Lock file doesnt exist.
[2017-03-28 20:12:55] glsysbackup: [17864] No other instance of: 'glsysbackup' is currently running (Lockfile: '/var/lock/glsysbackup' doesnt exist and no processes are running).
[2017-03-28 20:12:55] glsysbackup: [17864] Check if script lock directory: '/var/lock/' exists and permissions to set lock are ok...
[2017-03-28 20:12:55] glsysbackup: [17864] Script lock directory exists and permissions are ok.
[2017-03-28 20:12:55] glsysbackup: [17864] Setting lock...
[2017-03-28 20:12:55] glsysbackup: [17864] Setting lock was successful.
[2017-03-28 20:12:56] glsysbackup: [17864] Check if re-niceing is required...
[2017-03-28 20:12:56] glsysbackup: [17864] Re-niceing is not required.
[2017-03-28 20:12:56] glsysbackup: [17864] Check if re-ioniceing is required...
[2017-03-28 20:12:56] glsysbackup: [17864] Re-ioniceing is not required.
[2017-03-28 20:12:56] glsysbackup: [17864] Pre backup script: '/home/pi/pre.sh' is defined...
[2017-03-28 20:12:56] glsysbackup: [17864] Check if pre backup script exists and if it is executeable...
[2017-03-28 20:12:56] glsysbackup: [17864] Pre backup script exists and it is executeable, so we execute it...
[2017-03-28 20:12:56] glsysbackup: [17864] Doing something here in pre script.... 1
[2017-03-28 20:12:56] glsysbackup: [17864] Doing something here in pre script.... 2
[2017-03-28 20:12:56] glsysbackup: [17864] Doing something here in pre script.... 3
[2017-03-28 20:12:57] glsysbackup: [17864] Doing something here in pre script.... 4
[2017-03-28 20:12:57] glsysbackup: [17864] Doing something here in pre script.... 5
[2017-03-28 20:12:57] glsysbackup: [17864] Execution of pre backup script was successful. rc: '7'.
[2017-03-28 20:12:57] glsysbackup: [17864] Check if backup rotation is required...
[2017-03-28 20:12:58] glsysbackup: [17864] Backup rotation is required.
[2017-03-28 20:12:58] glsysbackup: [17864] Check if backup directory: '/var/backups/' exists and permissions to move files are ok...
[2017-03-28 20:12:58] glsysbackup: [17864] Backup directory exists and permissions are ok.
[2017-03-28 20:12:58] glsysbackup: [17864] Rotating backup files...
[2017-03-28 20:12:58] glsysbackup: [17864] Check if file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.3' exists and if it is readable...
[2017-03-28 20:12:58] glsysbackup: [17864] File doesnt exist, nothing to do.
[2017-03-28 20:12:58] glsysbackup: [17864] Check if file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.2' exists and if it is readable...
[2017-03-28 20:12:58] glsysbackup: [17864] File doesnt exist, nothing to do.
[2017-03-28 20:12:58] glsysbackup: [17864] Check if file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.1' exists and if it is readable...
[2017-03-28 20:12:58] glsysbackup: [17864] File exists and is readable, do rotation...
[2017-03-28 20:12:58] glsysbackup: [17864] Rotating file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.1' ==> '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.2' was successful.
[2017-03-28 20:12:58] glsysbackup: [17864] Check if file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz' exists and if it is readable...
[2017-03-28 20:12:58] glsysbackup: [17864] File doesnt exist, nothing to do.
[2017-03-28 20:12:58] glsysbackup: [17864] Check if installed packages are required...
[2017-03-28 20:12:58] glsysbackup: [17864] Installed packages are required.
[2017-03-28 20:12:58] glsysbackup: [17864] The supported system package manager we found is: 'dpkg'.
[2017-03-28 20:12:58] glsysbackup: [17864] Creating installed packages file: '/root/installed_syspackages.txt'...
[2017-03-28 20:12:58] glsysbackup: [17864] Creating installed packages file was successful, adding file to backup_items config array.
[2017-03-28 20:12:58] glsysbackup: [17864] Check if we need to build excluding options...
[2017-03-28 20:12:58] glsysbackup: [17864] Building of excluding options is required.
[2017-03-28 20:12:59] glsysbackup: [17864] We build the following excluding options: '--exclude='/old.backups' --exclude='/old.mysqldumps/''.
[2017-03-28 20:12:59] glsysbackup: [17864] Building backup options...
[2017-03-28 20:12:59] glsysbackup: [17864] We build the following backup options: '--create --file=/var/backups/glsysbackup.wurlitzer.tar.gz --gzip --verbose'.
[2017-03-28 20:12:59] glsysbackup: [17864] Starting backup job...
[2017-03-28 20:12:59] glsysbackup: [17864] /bin/tar: Entferne führende „/“ von Elementnamen
[2017-03-28 20:12:59] glsysbackup: [17864] /etc/
[2017-03-28 20:12:59] glsysbackup: [17864] /etc/locale.alias
[2017-03-28 20:12:59] glsysbackup: [17864] /etc/subgid
[2017-03-28 20:12:59] glsysbackup: [17864] /etc/sgml/
[2017-03-28 20:12:59] glsysbackup: [17864] /etc/sgml/xml-core.cat
[2017-03-28 20:12:59] glsysbackup: [17864] /etc/sgml/catalog
.
.
[snip]
.
.
[2017-03-28 20:14:13] glsysbackup: [17864] /var/lib/mpd/state
[2017-03-28 20:14:14] glsysbackup: [17864] /usr/local/bin/
[2017-03-28 20:14:14] glsysbackup: [17864] /usr/local/bin/mpc_playlist_update
[2017-03-28 20:14:14] glsysbackup: [17864] /usr/local/bin/youtube-dl
[2017-03-28 20:14:14] glsysbackup: [17864] /usr/local/bin/wurlitzerd
[2017-03-28 20:14:14] glsysbackup: [17864] /usr/local/bin/bashlib
[2017-03-28 20:14:14] glsysbackup: [17864] /usr/local/bin/glsysbackup
[2017-03-28 20:14:14] glsysbackup: [17864] /usr/local/bin/sysupdate
[2017-03-28 20:14:14] glsysbackup: [17864] /boot/config.txt
[2017-03-28 20:14:14] glsysbackup: [17864] /bin/tar: Entferne führende „/“ von Zielen harter Verknüpfungen
[2017-03-28 20:14:14] glsysbackup: [17864] /root/installed_syspackages.txt
[2017-03-28 20:14:14] glsysbackup: [17864] Backup job was successful.
[2017-03-28 20:14:14] glsysbackup: [17864] Backup encryption is enabled...
[2017-03-28 20:14:14] glsysbackup: [17864] Encrypting backup file: '/var/backups/glsysbackup.wurlitzer.tar.gz'...
[2017-03-28 20:14:15] glsysbackup: [17864] Encrypting job was successful.
[2017-03-28 20:14:15] glsysbackup: [17864] HINT: To decrypt use this command: '/usr/bin/openssl aes-256-cbc -d -salt -in /var/backups/glsysbackup.wurlitzer.aes.tar.gz -out /var/backups/glsysbackup.wurlitzer.tar.gz'.
[2017-03-28 20:14:15] glsysbackup: [17864] Post backup script: '/home/pi/post.sh' is defined...
[2017-03-28 20:14:15] glsysbackup: [17864] Check if post backup script exists and if it is executeable...
[2017-03-28 20:14:15] glsysbackup: [17864] Post backup script exists and it is executeable, so we execute it...
[2017-03-28 20:14:15] glsysbackup: [17864] Doing something here in post script.... 1
[2017-03-28 20:14:15] glsysbackup: [17864] Doing something here in post script.... 2
[2017-03-28 20:14:15] glsysbackup: [17864] Doing something here in post script.... 3
[2017-03-28 20:14:16] glsysbackup: [17864] Doing something here in post script.... 4
[2017-03-28 20:14:16] glsysbackup: [17864] Doing something here in post script.... 5
[2017-03-28 20:14:16] glsysbackup: [17864] Execution of post backup script was successful. rc: '7'.
[2017-03-28 20:14:16] glsysbackup: [17864] Caught: 'EXIT', exiting script...
[2017-03-28 20:14:17] glsysbackup: [17864] Now we are doing some cleanup jobs...
[2017-03-28 20:14:17] glsysbackup: [17864] Backup encryption is enabled, now we check if unencrypted backup file exists, if it is writeable and delete it...
[2017-03-28 20:14:17] glsysbackup: [17864] Check if unencrypted backup file: '/var/backups/glsysbackup.wurlitzer.tar.gz' exists and if it is writeable...
[2017-03-28 20:14:17] glsysbackup: [17864] Unencrypted backup file  exists and it is writeable.
[2017-03-28 20:14:17] glsysbackup: [17864] Deleting unencrypted backup file...
[2017-03-28 20:14:17] glsysbackup: [17864] Deleting unencrypted backup file was successful.
[2017-03-28 20:14:17] glsysbackup: [17864] Installed packages functionality is enabled, now we check if the file exists, if it is writeable and delete it...
[2017-03-28 20:14:17] glsysbackup: [17864] Check if installed packages file: '/root/installed_syspackages.txt' exists and if it is writeable...
[2017-03-28 20:14:17] glsysbackup: [17864] Installed packages file exists and is writeable.
[2017-03-28 20:14:17] glsysbackup: [17864] Deleting installed packages file...
[2017-03-28 20:14:17] glsysbackup: [17864] Deleting installed packages file was successful.
[2017-03-28 20:14:17] glsysbackup: [17864] We did a great job. :)
[2017-03-28 20:14:17] glsysbackup: [17864] Check if lock file: '/var/lock/glsysbackup' exists and if it is read and writeable...
[2017-03-28 20:14:17] glsysbackup: [17864] Lock file exists and it is read/writeable.
[2017-03-28 20:14:17] glsysbackup: [17864] Removing lock...
[2017-03-28 20:14:17] glsysbackup: [17864] Removing lock was successful.
[2017-03-28 20:14:17] glsysbackup: [17864] Script was running: '83' seconds.
[2017-03-28 20:14:17] glsysbackup: [17864] Bye, bye...
```



## Backupfile structure with enabled openssl encryption and backup rotation:
```
20:23:47 [pi@localhost]:~$ ls -lah /var/backups/glsysbackup.wurlitzer.aes.tar.gz*
-rw-r--r-- 1 root root 3,3M Mär 26 12:37 /var/backups/glsysbackup.wurlitzer.aes.tar.gz
-rw-r--r-- 1 root root 3,3M Mär 24 19:07 /var/backups/glsysbackup.wurlitzer.aes.tar.gz.1
-rw-r--r-- 1 root root 3,3M Mär 24 19:05 /var/backups/glsysbackup.wurlitzer.aes.tar.gz.2
-rw-r--r-- 1 root root 3,3M Mär 23 20:24 /var/backups/glsysbackup.wurlitzer.aes.tar.gz.3
-rw-r--r-- 1 root root 3,5M Mär  9 20:28 /var/backups/glsysbackup.wurlitzer.aes.tar.gz.4
```



## Features:
- Lock functionality. Only one instance is possible to run. (Lock file and check with pgrep)
- Verbose logging to stdout and/or system logfile and/or individual logfile.
- Excluding of files
- Backupfile rotation
- Creates a file with installed packages (rpm || dpkg || pacman || equery || pkgutil)
- Encryption with openssl
- CLI options and arguments
- Renicing
- Re-ioniceing
- Execution of pre-backup-script
- Execution of post-backup-script



## Description:
1. Check if the required binaries exists
2. Set configuration of logHandler
3. Check if glsysbackup will be exectuted with root privileges
4. Get and log the user, who starts glsysbackup
5. Check if bash version meets requirements
6. Check if an instance is already running via lockfile and pgrep
7. Re-nice glsysbackup if required
8. Re-ionice glsysbackup if required
9. Execute the pre-backup-script, if it is defined and executeable
10. Rotation of backup files
11. Create file with installed packages
12. Build excluding options from config array
13. Make the tar backup
14. Enrypt the backup with openssl if required
15. Execute the post-backup-script, if it is defined and executeable



## It requires the following binaries:
- **bash** (Version 3 || 4)
- **which** to get the full path to the required binaries through environment variable $PATH
- **pgrep** to check if an instance of glsysbackup is already running
- **whoami** to check the user who executes glsysbackup
- **date** for logging purposes (Only required if bash version < 4.2. Else printf builtin will be used.)
- **tar** to create the backup
- **rm** to delete files
- **mv** to move files
- **mkdir** to create directories
- **kill** to send kill signal to glsysbackup



## Optionally used binaries:
- **logger** to log to the system log
- **rpm** || **dpkg** || **pacman** || **equery** || **pkgutil** to create a file with installed packages
- **openssl** to encrypt the backup file
- **renice** to renice glsysbackup and all child processes
- **ionice** to re-ionice glsysbackup and all child processes



## Future plans:
- use config files for multiple jobs
- add sendEmail functionality
- add incremental backup feature
