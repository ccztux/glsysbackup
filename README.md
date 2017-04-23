[![Travis branch](https://img.shields.io/travis/ccztux/glsysbackup/master.svg?label=build%20%28master%29)](https://travis-ci.org/ccztux/glsysbackup)
[![Travis branch](https://img.shields.io/travis/ccztux/glsysbackup/devel.svg?label=build%20%28devel%29)](https://travis-ci.org/ccztux/glsysbackup)
[![Latest Release](https://img.shields.io/github/release/ccztux/glsysbackup.svg?label=latest%20release)](https://github.com/ccztux/glsysbackup/releases/latest)
[![Latest Pre-release](https://img.shields.io/badge/latest%20pre--release-v1.0.1--beta1-orange.svg)](https://github.com/ccztux/glsysbackup/releases/tag/1.0.1-beta1)
[![GitHub license](https://img.shields.io/badge/license-AGPL-blue.svg)](https://github.com/ccztux/glsysbackup/blob/master/LICENSE)



# glsysbackup
(**g**eneric **l**inux **sys**tem **backup**) is an advanced backup tool written in bash.



## Example help output:
```
18:43:45 [root@localhost]:~$ ./glsysbackup -h
[2017-04-23 18:43:45] glsysbackup: [3778] glsysbackup 1.0.1 starting... (PID=3778)
[2017-04-23 18:43:45] glsysbackup: [3778] Getting options...

Usage: glsysbackup OPTIONS

Author:			Christian Zettel (ccztux)
Last modification:	2017-04-23
Version:		1.0.1

Description:		glsysbackup (Generic Linux System Backup) is an advanced backup tool written in bash.

OPTIONS:
   -h		Shows this help.
      -o		Override lock in case glsysbackup was terminated abnormally.
         -v		Shows detailed version information.

	 [2017-04-23 18:43:45] glsysbackup: [3778] Caught: 'EXIT', exiting script...
	 [2017-04-23 18:43:45] glsysbackup: [3778] We hope you are informed better now. :P This was a lazy job. :)
	 [2017-04-23 18:43:45] glsysbackup: [3778] Script was running: '0' seconds.
	 [2017-04-23 18:43:45] glsysbackup: [3778] Bye, bye...

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
re_nice_enabled="0"

# set renice priority (HINT: have a look at: 'man renice')
re_nice_priority="19"



#-----------
# Re-ionice:
#-----------

# enable re-ioniceing of glsysbackup and child procs (possible values: 1|0)
re_ionice_enabled="0"

# set re-ionice scheduling class (HINT: have a look at: 'man ionice')
re_ionice_scheduling_class="2"

# set re-ionice priority (HINT: have a look at: 'man ionice')
re_ionice_priority="7"



#----------
# Rotation:
#----------

# enable backup rotation (possible values: 1|0)
backup_rotation_enabled="1"

# number of backup files to keep
backup_rotation_files_to_keep="5"



#--------------------
# Installed packages:
#--------------------

# enable the creation of installed packages file (possible values: 1|0)
installed_packages_enabled="1"

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
backup_encryption_enabled="1"

# set password for encryption
backup_encryption_password="test1234"




#-------------------
# Pre backup script:
#-------------------

# enable pre backup script functionality (possible values: 1|0)
pre_backup_script_enabled="1"

# path to pre backup script 
pre_backup_script="/home/pi/pre.sh"

# exit glsysbackup in case execution of pre backup script was not successful
pre_backup_exit_when_unsuccessful="1"



#--------------------
# Post backup script:
#--------------------

# enable post backup script functionality (possible values: 1|0)
post_backup_script_enabled="1"

# path to post backup script 
post_backup_script="/home/pi/post.sh"

# exit glsysbackup in case execution of post backup script was not successful
post_backup_exit_when_unsuccessful="1"
```



## Example job output:
```
18:46:20 [pi@localhost]:~$ sudo ./glsysbackup 
[2017-04-23 18:46:23] glsysbackup: [25695] glsysbackup 1.0.1 starting... (PID=25695)
[2017-04-23 18:46:24] glsysbackup: [25695] Check if root priviliges are required...
[2017-04-23 18:46:24] glsysbackup: [25695] Root privileges are required, checking privileges...
[2017-04-23 18:46:24] glsysbackup: [25695] HOORAY, we have root privileges. :)
[2017-04-23 18:46:24] glsysbackup: [25695] Get user which starts the script...
[2017-04-23 18:46:24] glsysbackup: [25695] glsysbackup was started by user: 'pi'.
[2017-04-23 18:46:24] glsysbackup: [25695] Checking bash version...
[2017-04-23 18:46:24] glsysbackup: [25695] Bash version: '4' meets requirements.
[2017-04-23 18:46:24] glsysbackup: [25695] Check if another instance of: 'glsysbackup' is already running...
[2017-04-23 18:46:24] glsysbackup: [25695] Check if lock file: '/var/lock/glsysbackup' exists and if it is read and writeable...
[2017-04-23 18:46:24] glsysbackup: [25695] Lock file doesnt exist.
[2017-04-23 18:46:24] glsysbackup: [25695] No other instance of: 'glsysbackup' is currently running (Lockfile: '/var/lock/glsysbackup' doesnt exist and no processes are running).
[2017-04-23 18:46:24] glsysbackup: [25695] Check if script lock directory: '/var/lock/' exists and permissions to set lock are ok...
[2017-04-23 18:46:24] glsysbackup: [25695] Script lock directory exists and permissions are ok.
[2017-04-23 18:46:24] glsysbackup: [25695] Setting lock...
[2017-04-23 18:46:24] glsysbackup: [25695] Setting lock was successful.
[2017-04-23 18:46:24] glsysbackup: [25695] Check if re-niceing is enabled...
[2017-04-23 18:46:24] glsysbackup: [25695] Re-niceing is enabled.
[2017-04-23 18:46:24] glsysbackup: [25695] Check if renice priority is in range...
[2017-04-23 18:46:24] glsysbackup: [25695] Re-nice priority: '19' is in range.
[2017-04-23 18:46:24] glsysbackup: [25695] Re-niceing...
[2017-04-23 18:46:25] glsysbackup: [25695] 25695 (process ID) old priority 0, new priority 19
[2017-04-23 18:46:25] glsysbackup: [25695] Re-niceing was successful.
[2017-04-23 18:46:25] glsysbackup: [25695] Check if re-ioniceing is enabled...
[2017-04-23 18:46:25] glsysbackup: [25695] Re-ioniceing is enabled.
[2017-04-23 18:46:25] glsysbackup: [25695] Check if re-ioniceing parameters are in range...
[2017-04-23 18:46:25] glsysbackup: [25695] Re-ioniceing scheduling class: '2' is in range.
[2017-04-23 18:46:25] glsysbackup: [25695] Selected re-ioniceing scheduling class: '2' requires re-ioniceing priority.
[2017-04-23 18:46:25] glsysbackup: [25695] Check if re-ioniceing priority is in range...
[2017-04-23 18:46:25] glsysbackup: [25695] Re-ioniceing priority: '2' is in range.
[2017-04-23 18:46:25] glsysbackup: [25695] Re-ioniceing...
[2017-04-23 18:46:25] glsysbackup: [25695] Re-ioniceing was successful.
[2017-04-23 18:46:25] glsysbackup: [25695] Check if pre backup script functionality is enabled...
[2017-04-23 18:46:25] glsysbackup: [25695] Pre backup script functionality is enabled.
[2017-04-23 18:46:25] glsysbackup: [25695] Check if pre backup script (/home/pi/pre.sh) exists and if it is executeable...
[2017-04-23 18:46:25] glsysbackup: [25695] Pre backup script exists and it is executeable, so we execute it...
[2017-04-23 18:46:25] glsysbackup: [25695] Doing something here in pre script.... 1
[2017-04-23 18:46:25] glsysbackup: [25695] Doing something here in pre script.... 2
[2017-04-23 18:46:26] glsysbackup: [25695] Doing something here in pre script.... 3
[2017-04-23 18:46:26] glsysbackup: [25695] Doing something here in pre script.... 4
[2017-04-23 18:46:26] glsysbackup: [25695] Doing something here in pre script.... 5
[2017-04-23 18:46:26] glsysbackup: [25695] Execution of pre backup script was successful. rc: '0'.
[2017-04-23 18:46:26] glsysbackup: [25695] Check if backup rotation is enabled...
[2017-04-23 18:46:26] glsysbackup: [25695] Backup rotation is enabled.
[2017-04-23 18:46:26] glsysbackup: [25695] Check if backup directory: '/var/backups/' exists and permissions to move files are ok...
[2017-04-23 18:46:26] glsysbackup: [25695] Backup directory exists and permissions are ok.
[2017-04-23 18:46:26] glsysbackup: [25695] Rotating backup files...
[2017-04-23 18:46:26] glsysbackup: [25695] Check if file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.3' exists and if it is readable...
[2017-04-23 18:46:26] glsysbackup: [25695] File exists and is readable, do rotation...
[2017-04-23 18:46:26] glsysbackup: [25695] Rotating file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.3' ==> '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.4' was successful.
[2017-04-23 18:46:26] glsysbackup: [25695] Check if file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.2' exists and if it is readable...
[2017-04-23 18:46:26] glsysbackup: [25695] File doesnt exist, nothing to do.
[2017-04-23 18:46:26] glsysbackup: [25695] Check if file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.1' exists and if it is readable...
[2017-04-23 18:46:26] glsysbackup: [25695] File exists and is readable, do rotation...
[2017-04-23 18:46:27] glsysbackup: [25695] Rotating file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.1' ==> '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.2' was successful.
[2017-04-23 18:46:27] glsysbackup: [25695] Check if file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz' exists and if it is readable...
[2017-04-23 18:46:27] glsysbackup: [25695] File doesnt exist, nothing to do.
[2017-04-23 18:46:27] glsysbackup: [25695] Check if installed packages are enabled...
[2017-04-23 18:46:27] glsysbackup: [25695] Installed packages are enabled.
[2017-04-23 18:46:27] glsysbackup: [25695] The supported system package manager we found is: 'dpkg'.
[2017-04-23 18:46:27] glsysbackup: [25695] Creating installed packages file: '/root/installed_syspackages.txt'...
[2017-04-23 18:46:27] glsysbackup: [25695] Creating installed packages file was successful, adding file to backup_items config array.
[2017-04-23 18:46:27] glsysbackup: [25695] Check if we need to build excluding options...
[2017-04-23 18:46:27] glsysbackup: [25695] We dont have to build excluding options...
[2017-04-23 18:46:27] glsysbackup: [25695] Building backup options...
[2017-04-23 18:46:27] glsysbackup: [25695] We build the following backup options: '--create --file=/var/backups/glsysbackup.wurlitzer.tar.gz --gzip --verbose'.
[2017-04-23 18:46:27] glsysbackup: [25695] Starting backup job...
[2017-04-23 18:46:27] glsysbackup: [25695] /bin/tar: Entferne führende „/“ von Elementnamen
[2017-04-23 18:46:27] glsysbackup: [25695] /etc/
[2017-04-23 18:46:27] glsysbackup: [25695] /etc/locale.alias
[2017-04-23 18:46:28] glsysbackup: [25695] /etc/subgid
[2017-04-23 18:46:28] glsysbackup: [25695] /etc/sgml/
[2017-04-23 18:46:28] glsysbackup: [25695] /etc/sgml/xml-core.cat
[2017-04-23 18:46:28] glsysbackup: [25695] /etc/sgml/catalog
[2017-04-23 18:46:28] glsysbackup: [25695] /etc/securetty
[2017-04-23 18:46:28] glsysbackup: [25695] /etc/ppp/
[2017-04-23 18:46:28] glsysbackup: [25695] /etc/ppp/ip-up.d/
[2017-04-23 18:46:28] glsysbackup: [25695] /etc/ppp/ip-up.d/000resolvconf
[2017-04-23 18:46:28] glsysbackup: [25695] /etc/ppp/ip-down.d/
[2017-04-23 18:46:28] glsysbackup: [25695] /etc/ppp/ip-down.d/000resolvconf
[2017-04-23 18:46:28] glsysbackup: [25695] /etc/locale.gen
[2017-04-23 18:46:29] glsysbackup: [25695] /etc/nanorc
[2017-04-23 18:46:29] glsysbackup: [25695] /etc/rmt
[2017-04-23 18:46:29] glsysbackup: [25695] /etc/rsyslog.conf
[2017-04-23 18:46:29] glsysbackup: [25695] /etc/kbd/
[2017-04-23 18:46:29] glsysbackup: [25695] /etc/kbd/config
[2017-04-23 18:46:29] glsysbackup: [25695] /etc/kbd/remap
[2017-04-23 18:46:29] glsysbackup: [25695] /etc/default/
[2017-04-23 18:46:29] glsysbackup: [25695] /etc/default/keyboard
[2017-04-23 18:46:29] glsysbackup: [25695] /etc/default/triggerhappy
[2017-04-23 18:46:29] glsysbackup: [25695] /etc/default/fake-hwclock
[2017-04-23 18:46:29] glsysbackup: [25695] /etc/default/rsyslog
[2017-04-23 18:46:29] glsysbackup: [25695] /etc/default/useradd
[2017-04-23 18:46:30] glsysbackup: [25695] /etc/default/nss
[2017-04-23 18:46:30] glsysbackup: [25695] /etc/default/nfs-common
[2017-04-23 18:46:30] glsysbackup: [25695] /etc/default/console-setup
[2017-04-23 18:46:30] glsysbackup: [25695] /etc/default/rcS
[2017-04-23 18:46:30] glsysbackup: [25695] /etc/default/halt
[2017-04-23 18:46:30] glsysbackup: [25695] /etc/default/networking
[2017-04-23 18:46:30] glsysbackup: [25695] /etc/default/rsync
[2017-04-23 18:46:30] glsysbackup: [25695] /etc/default/crda
[2017-04-23 18:46:30] glsysbackup: [25695] /etc/default/dbus
[2017-04-23 18:46:30] glsysbackup: [25695] /etc/default/locale
[2017-04-23 18:46:30] glsysbackup: [25695] /etc/default/tmpfs
[2017-04-23 18:46:31] glsysbackup: [25695] /etc/default/devpts
[2017-04-23 18:46:31] glsysbackup: [25695] /etc/default/hwclock
[2017-04-23 18:46:31] glsysbackup: [25695] /etc/default/bsdmainutils
[2017-04-23 18:46:31] glsysbackup: [25695] /etc/default/mpd
[2017-04-23 18:46:31] glsysbackup: [25695] /etc/default/avahi-daemon
[2017-04-23 18:46:31] glsysbackup: [25695] /etc/default/cron
[2017-04-23 18:46:31] glsysbackup: [25695] /etc/default/ssh
[2017-04-23 18:46:31] glsysbackup: [25695] /etc/default/ntp
[2017-04-23 18:46:31] glsysbackup: [25695] /etc/cron.weekly/
[2017-04-23 18:46:31] glsysbackup: [25695] /etc/cron.weekly/.placeholder
[2017-04-23 18:46:31] glsysbackup: [25695] /etc/cron.weekly/man-db
.
.
[snip]
.
.
[2017-04-23 18:47:55] glsysbackup: [25695] /usr/lib/cgi-bin/wurlitzer-webui/inc/
[2017-04-23 18:47:55] glsysbackup: [25695] /usr/lib/cgi-bin/wurlitzer-webui/inc/mpc.functions.sh
[2017-04-23 18:47:55] glsysbackup: [25695] /usr/lib/cgi-bin/wurlitzer-webui/inc/base.functions.sh
[2017-04-23 18:47:55] glsysbackup: [25695] /usr/lib/cgi-bin/wurlitzer-webui/js/
[2017-04-23 18:47:55] glsysbackup: [25695] /usr/lib/cgi-bin/wurlitzer-webui/js/wurlitzer_title.js
[2017-04-23 18:47:55] glsysbackup: [25695] /usr/lib/cgi-bin/wurlitzer-webui/helper/
[2017-04-23 18:47:55] glsysbackup: [25695] /usr/lib/cgi-bin/wurlitzer-webui/helper/wurlitzer_title.cgi
[2017-04-23 18:47:55] glsysbackup: [25695] /usr/lib/cgi-bin/wurlitzer-webui/index.cgi
[2017-04-23 18:47:55] glsysbackup: [25695] /usr/lib/cgi-bin/wurlitzer-webui/img/
[2017-04-23 18:47:55] glsysbackup: [25695] /usr/lib/cgi-bin/wurlitzer-webui/img/youtube_logo.png
[2017-04-23 18:47:55] glsysbackup: [25695] /usr/lib/cgi-bin/wurlitzer-webui/img/music.png
[2017-04-23 18:47:55] glsysbackup: [25695] /usr/lib/cgi-bin/wurlitzer-webui/img/favicon.ico
[2017-04-23 18:47:55] glsysbackup: [25695] /boot/config.txt
[2017-04-23 18:47:55] glsysbackup: [25695] /bin/tar: Entferne führende „/“ von Zielen harter Verknüpfungen
[2017-04-23 18:47:56] glsysbackup: [25695] /root/installed_syspackages.txt
[2017-04-23 18:47:56] glsysbackup: [25695] Backup job was successful.
[2017-04-23 18:47:56] glsysbackup: [25695] Backup encryption is enabled...
[2017-04-23 18:47:56] glsysbackup: [25695] Encrypting backup file: '/var/backups/glsysbackup.wurlitzer.tar.gz'...
[2017-04-23 18:47:56] glsysbackup: [25695] Encrypting job was successful.
[2017-04-23 18:47:56] glsysbackup: [25695] HINT: To decrypt use this command: '/usr/bin/openssl aes-256-cbc -d -salt -in /var/backups/glsysbackup.wurlitzer.aes.tar.gz -out /var/backups/glsysbackup.wurlitzer.tar.gz'.
[2017-04-23 18:47:56] glsysbackup: [25695] Check if post backup script functionality is enabled...
[2017-04-23 18:47:56] glsysbackup: [25695] Post backup script functionality is enabled.
[2017-04-23 18:47:56] glsysbackup: [25695] Check if post backup script (/home/pi/post.sh) exists and if it is executeable...
[2017-04-23 18:47:57] glsysbackup: [25695] Post backup script exists and it is executeable, so we execute it...
[2017-04-23 18:47:57] glsysbackup: [25695] Doing something here in post script.... 1
[2017-04-23 18:47:57] glsysbackup: [25695] Doing something here in post script.... 2
[2017-04-23 18:47:57] glsysbackup: [25695] Doing something here in post script.... 3
[2017-04-23 18:47:57] glsysbackup: [25695] Doing something here in post script.... 4
[2017-04-23 18:47:57] glsysbackup: [25695] Doing something here in post script.... 5
[2017-04-23 18:47:57] glsysbackup: [25695] Execution of post backup script was successful. rc: '0'.
[2017-04-23 18:47:57] glsysbackup: [25695] Caught: 'EXIT', exiting script...
[2017-04-23 18:47:57] glsysbackup: [25695] Now we are doing some cleanup jobs...
[2017-04-23 18:47:57] glsysbackup: [25695] Backup encryption is enabled, now we check if unencrypted backup file exists, if it is writeable and delete it...
[2017-04-23 18:47:57] glsysbackup: [25695] Check if unencrypted backup file: '/var/backups/glsysbackup.wurlitzer.tar.gz' exists and if it is writeable...
[2017-04-23 18:47:57] glsysbackup: [25695] Unencrypted backup file  exists and it is writeable.
[2017-04-23 18:47:57] glsysbackup: [25695] Deleting unencrypted backup file...
[2017-04-23 18:47:57] glsysbackup: [25695] Deleting unencrypted backup file was successful.
[2017-04-23 18:47:58] glsysbackup: [25695] Installed packages functionality is enabled, now we check if the file exists, if it is writeable and delete it...
[2017-04-23 18:47:58] glsysbackup: [25695] Check if installed packages file: '/root/installed_syspackages.txt' exists and if it is writeable...
[2017-04-23 18:47:58] glsysbackup: [25695] Installed packages file exists and is writeable.
[2017-04-23 18:47:58] glsysbackup: [25695] Deleting installed packages file...
[2017-04-23 18:47:58] glsysbackup: [25695] Deleting installed packages file was successful.
[2017-04-23 18:47:58] glsysbackup: [25695] We did a great job. :)
[2017-04-23 18:47:58] glsysbackup: [25695] Check if lock file: '/var/lock/glsysbackup' exists and if it is read and writeable...
[2017-04-23 18:47:58] glsysbackup: [25695] Lock file exists and it is read/writeable.
[2017-04-23 18:47:58] glsysbackup: [25695] Removing lock...
[2017-04-23 18:47:58] glsysbackup: [25695] Removing lock was successful.
[2017-04-23 18:47:58] glsysbackup: [25695] Script was running: '95' seconds.
[2017-04-23 18:47:58] glsysbackup: [25695] Bye, bye...
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
