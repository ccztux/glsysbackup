[![Travis branch](https://img.shields.io/travis/ccztux/glsysbackup/master.svg?label=build%20%28master%29)](https://travis-ci.org/ccztux/glsysbackup)
[![Travis branch](https://img.shields.io/travis/ccztux/glsysbackup/devel.svg?label=build%20%28devel%29)](https://travis-ci.org/ccztux/glsysbackup)
[![Latest Release](https://img.shields.io/github/release/ccztux/glsysbackup.svg?label=latest%20release)](https://github.com/ccztux/glsysbackup/releases/latest)
[![Latest Pre-release](https://img.shields.io/badge/latest%20pre--release-v1.0.1--alpha3-orange.svg)](https://github.com/ccztux/glsysbackup/releases/tag/1.0.1-alpha3)
[![GitHub license](https://img.shields.io/badge/license-AGPL-blue.svg)](https://github.com/ccztux/glsysbackup/blob/master/LICENSE)



# glsysbackup
(**g**eneric **l**inux **sys**tem **backup**) is an advanced backup tool written in bash.



## Example help output:
```
20:10:35 [root@localhost]:~$ ./glsysbackup -h
[2017-03-23 20:10:35] glsysbackup: [13040] glsysbackup 1.0.1-beta1 starting... (PID=13040)
[2017-03-23 20:10:35] glsysbackup: [13040] Getting options...

Usage: glsysbackup OPTIONS

Author:			Christian Zettel (ccztux)
Last modification:	2017-03-23
Version:		1.0.1-beta1

Description:		glsysbackup (Generic Linux System Backup) is an advanced backup tool written in bash.

OPTIONS:
   -h		Shows this help.
   -o		Override lock in case glsysbackup was terminated abnormally.
   -v		Shows detailed version information.

[2017-03-23 20:10:35] glsysbackup: [13040] Caught: 'EXIT', exiting script...
[2017-03-23 20:10:35] glsysbackup: [13040] We hope you are informed better now. :P This was a lazy job. :)
[2017-03-23 20:10:35] glsysbackup: [13040] Script was running: '0' seconds.
[2017-03-23 20:10:35] glsysbackup: [13040] Bye, bye...
```



## Configuration variables:
```bash
#-------------------------
# Configuration variables:
#-------------------------

log_to_file="0"
log_to_stdout="1"
log_to_syslog="1"
log_directory="/var/log/"
log_file="${log_directory}${script_name}.log"
log_timestamp_format="%Y-%m-%d %H:%M:%S"

root_privileges_required="1"

re_nice_required="0"
re_nice_priority="19"

re_ionice_required="0"
re_ionice_scheduling_class="2"
re_ionice_priority="7"

backup_rotation_required="1"
backup_rotation_files_to_keep="5"

installed_packages_required="1"
installed_packages_force_manager=""
installed_packages_filename="/root/installed_syspackages.txt"

backup_compression_enabled="1"
backup_compression_type="gzip"
backup_verbose_mode_enabled="1"
backup_destination_path="/var/backups/"
backup_filename="${script_name}.${script_hostname}.tar.gz"
backup_full_path="${backup_destination_path}${backup_filename}"
backup_items=(
"/home/"
"/root/"
"/var/lib/mpd/"
"/usr/local/bin/"
"/boot/config.txt"
)
backup_exlude_items=(
"/blah"
"/zwei/"
)

backup_encryption_required="1"
backup_encryption_password="test1234"

break_if_pre_backup_unsuccessful="1"
pre_backup_script="/home/pi/pre.sh"
break_if_post_backup_unsuccessful="1"
post_backup_script="/home/pi/post.sh"
```



## Example job output:
```
20:23:47 [pi@localhost]:~$ sudo ./glsysbackup
[2017-03-23 20:23:47] glsysbackup: [27771] glsysbackup 1.0.1-alpha3 starting... (PID=27771)
[2017-03-23 20:23:47] glsysbackup: [27771] Check if root priviliges are required...
[2017-03-23 20:23:47] glsysbackup: [27771] Root privileges are required, checking privileges...
[2017-03-23 20:23:47] glsysbackup: [27771] HOORAY, we have root privileges. :)
[2017-03-23 20:23:47] glsysbackup: [27771] Get user which starts the script...
[2017-03-23 20:23:47] glsysbackup: [27771] glsysbackup was started by: 'pi'.
[2017-03-23 20:23:47] glsysbackup: [27771] Checking bash version...
[2017-03-23 20:23:47] glsysbackup: [27771] Bash version: '4' meets requirements.
[2017-03-23 20:23:47] glsysbackup: [27771] Check if another instance of: 'glsysbackup' is already running...
[2017-03-23 20:23:47] glsysbackup: [27771] Check if lock file: '/var/lock/glsysbackup' exists and if it is read and writeable...
[2017-03-23 20:23:47] glsysbackup: [27771] Lock file doesnt exist.
[2017-03-23 20:23:47] glsysbackup: [27771] No other instance of: 'glsysbackup' is currently running (Lockfile: '/var/lock/glsysbackup' doesnt exist and no processes are running).
[2017-03-23 20:23:47] glsysbackup: [27771] Check if script lock directory: '/var/lock/' exists and permissions to set lock are ok...
[2017-03-23 20:23:47] glsysbackup: [27771] Script lock directory exists and permissions are ok.
[2017-03-23 20:23:47] glsysbackup: [27771] Setting lock...
[2017-03-23 20:23:47] glsysbackup: [27771] Setting lock was successful.
[2017-03-23 20:23:47] glsysbackup: [27771] Check if re-niceing is required...
[2017-03-23 20:23:47] glsysbackup: [27771] Re-niceing is required.
[2017-03-23 20:23:47] glsysbackup: [27771] Check if renice priority is in range...
[2017-03-23 20:23:47] glsysbackup: [27771] Re-nice priority: '19' is in range.
[2017-03-23 20:23:47] glsysbackup: [27771] Re-niceing...
[2017-03-23 20:23:47] glsysbackup: [27771] 23858 (process ID) old priority 0, new priority 19
[2017-03-23 20:23:47] glsysbackup: [27771] Re-niceing was successful.
[2017-03-23 20:23:47] glsysbackup: [27771] Check if re-ioniceing is required...
[2017-03-23 20:23:47] glsysbackup: [27771] Re-ioniceing is required.
[2017-03-23 20:23:47] glsysbackup: [27771] Check if re-ioniceing parameters are in range...
[2017-03-23 20:23:47] glsysbackup: [27771] Re-ioniceing scheduling class: '2' is in range.
[2017-03-23 20:23:47] glsysbackup: [27771] Selected re-ioniceing scheduling class: '2' requires re-ioniceing priority.
[2017-03-23 20:23:47] glsysbackup: [27771] Check if re-ioniceing priority is in range...
[2017-03-23 20:23:47] glsysbackup: [27771] Re-ioniceing priority: '2' is in range.
[2017-03-23 20:23:47] glsysbackup: [27771] Re-ioniceing...
[2017-03-23 20:23:47] glsysbackup: [27771] Re-ioniceing was successful.
[2017-03-23 20:23:47] glsysbackup: [27771] Pre backup script: '/home/pi/pre.sh' is defined...
[2017-03-23 20:23:47] glsysbackup: [27771] Check if pre backup script exists and if it is executeable...
[2017-03-23 20:23:48] glsysbackup: [27771] Pre backup script exists and it is executeable, so we execute it...
[2017-03-23 20:23:48] glsysbackup: [27771] Doing something here in pre script.... 1
[2017-03-23 20:23:48] glsysbackup: [27771] Doing something here in pre script.... 2
[2017-03-23 20:23:48] glsysbackup: [27771] Doing something here in pre script.... 3
[2017-03-23 20:23:49] glsysbackup: [27771] Doing something here in pre script.... 4
[2017-03-23 20:23:49] glsysbackup: [27771] Doing something here in pre script.... 5
[2017-03-23 20:23:49] glsysbackup: [27771] Execution of pre backup script seems to be successful. rc: '0'.
[2017-03-23 20:23:49] glsysbackup: [27771] Check if backup rotating is required...
[2017-03-23 20:23:49] glsysbackup: [27771] Backup rotating is required.
[2017-03-23 20:23:49] glsysbackup: [27771] Check if backup directory: '/var/backups/' exists and permissions to move files are ok...
[2017-03-23 20:23:49] glsysbackup: [27771] Backup directory exists and permissions are ok.
[2017-03-23 20:23:49] glsysbackup: [27771] Rotating backup files...
[2017-03-23 20:23:49] glsysbackup: [27771] Check if file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.3' exists and if it is readable...
[2017-03-23 20:23:49] glsysbackup: [27771] File doesnt exist, nothing to do.
[2017-03-23 20:23:50] glsysbackup: [27771] Check if file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.2' exists and if it is readable...
[2017-03-23 20:23:50] glsysbackup: [27771] File doesnt exist, nothing to do.
[2017-03-23 20:23:50] glsysbackup: [27771] Check if file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.1' exists and if it is readable...
[2017-03-23 20:23:50] glsysbackup: [27771] File exists and is readable, do rotating...
[2017-03-23 20:23:50] glsysbackup: [27771] Rotating file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.1' ==> '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.2' was successful.
[2017-03-23 20:23:50] glsysbackup: [27771] Check if file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz' exists and if it is readable...
[2017-03-23 20:23:50] glsysbackup: [27771] File exists and is readable, do rotating...
[2017-03-23 20:23:50] glsysbackup: [27771] Rotating file: '/var/backups/glsysbackup.wurlitzer.aes.tar.gz' ==> '/var/backups/glsysbackup.wurlitzer.aes.tar.gz.1' was successful.
[2017-03-23 20:23:50] glsysbackup: [27771] Check if installed packages are required...
[2017-03-23 20:23:50] glsysbackup: [27771] Installed packages are required.
[2017-03-23 20:23:50] glsysbackup: [27771] The supported system package manager we found is: 'dpkg'.
[2017-03-23 20:23:50] glsysbackup: [27771] Creating installed packages file: '/root/installed_packages_filename.txt'...
[2017-03-23 20:23:50] glsysbackup: [27771] Creating installed packages file was successful.
[2017-03-23 20:23:50] glsysbackup: [27771] Check if we need to build excluding options...
[2017-03-23 20:23:50] glsysbackup: [27771] Building of excluding options is required.
[2017-03-23 20:23:50] glsysbackup: [27771] We build the following excluding options: '--exclude='/blah' --exclude='/zwei''.
[2017-03-23 20:23:51] glsysbackup: [27771] Starting backup job...
[2017-03-23 20:23:51] glsysbackup: [27771] /bin/tar: Entferne führende „/“ von Elementnamen
[2017-03-23 20:23:51] glsysbackup: [27771] /home/
[2017-03-23 20:23:51] glsysbackup: [27771] /home/pi/
[2017-03-23 20:23:51] glsysbackup: [27771] /home/pi/.gitconfig
[2017-03-23 20:23:51] glsysbackup: [27771] /home/pi/.lesshst
[2017-03-23 20:23:51] glsysbackup: [27771] /home/pi/.bash_history
[2017-03-23 20:23:51] glsysbackup: [27771] /home/pi/.viminfo
[2017-03-23 20:23:51] glsysbackup: [27771] /home/pi/post.sh
[2017-03-23 20:23:51] glsysbackup: [27771] /home/pi/.aptitude/
[2017-03-23 20:23:51] glsysbackup: [27771] /home/pi/.aptitude/config
[2017-03-23 20:23:51] glsysbackup: [27771] /home/pi/.aptitude/cache
[2017-03-23 20:23:51] glsysbackup: [27771] /home/pi/pre.sh
.
.
[snip]
.
.
[2017-03-23 20:24:04] glsysbackup: [27771] /usr/local/bin/
[2017-03-23 20:24:04] glsysbackup: [27771] /usr/local/bin/mpc_playlist_update
[2017-03-23 20:24:04] glsysbackup: [27771] /usr/local/bin/youtube-dl
[2017-03-23 20:24:04] glsysbackup: [27771] /usr/local/bin/wurlitzerd
[2017-03-23 20:24:04] glsysbackup: [27771] /usr/local/bin/bashlib
[2017-03-23 20:24:04] glsysbackup: [27771] /usr/local/bin/glsysbackup
[2017-03-23 20:24:04] glsysbackup: [27771] /usr/local/bin/sysupdate
[2017-03-23 20:24:04] glsysbackup: [27771] /boot/config.txt
[2017-03-23 20:24:04] glsysbackup: [27771] Backup job was successful.
[2017-03-23 20:24:05] glsysbackup: [27771] Backup encryption is enabled...
[2017-03-23 20:24:05] glsysbackup: [27771] Encrypting backup file: '/var/backups/glsysbackup.wurlitzer.tar.gz'...
[2017-03-23 20:24:05] glsysbackup: [27771] Encrypting job was successful.
[2017-03-23 20:24:05] glsysbackup: [27771] HINT: To decrypt use this command: '/usr/bin/openssl aes-256-cbc -d -salt -in /var/backups/glsysbackup.wurlitzer.aes.tar.gz -out /var/backups/glsysbackup.wurlitzer.tar.gz'.
[2017-03-23 20:24:05] glsysbackup: [27771] Post backup script: '/home/pi/post.sh' is defined...
[2017-03-23 20:24:05] glsysbackup: [27771] Check if post backup script exists and if it is executeable...
[2017-03-23 20:24:05] glsysbackup: [27771] Post backup script exists and it is executeable, so we execute it...
[2017-03-23 20:24:05] glsysbackup: [27771] Doing something here in post script.... 1
[2017-03-23 20:24:06] glsysbackup: [27771] Doing something here in post script.... 2
[2017-03-23 20:24:06] glsysbackup: [27771] Doing something here in post script.... 3
[2017-03-23 20:24:06] glsysbackup: [27771] Doing something here in post script.... 4
[2017-03-23 20:24:07] glsysbackup: [27771] Doing something here in post script.... 5
[2017-03-23 20:24:07] glsysbackup: [27771] Execution of post backup script seems to be unssuccessful. rc: '10'.
[2017-03-23 20:24:07] glsysbackup: [27771] Caught: 'EXIT', exiting script...
[2017-03-23 20:24:07] glsysbackup: [27771] Now we are doing some cleanup jobs...
[2017-03-23 20:24:07] glsysbackup: [27771] Backup encryption is enabled, now we check if unencrypted backup file exists, if it is writeable and delete it...
[2017-03-23 20:24:07] glsysbackup: [27771] Check if unencrypted backup file: '/var/backups/glsysbackup.wurlitzer.tar.gz' exists and if it is writeable...
[2017-03-23 20:24:07] glsysbackup: [27771] Unencrypted backup file  exists and it is writeable.
[2017-03-23 20:24:07] glsysbackup: [27771] Deleting unencrypted backup file...
[2017-03-23 20:24:07] glsysbackup: [27771] Deleting unencrypted backup file was successful.
[2017-03-23 20:24:07] glsysbackup: [27771] Installed packages functionality is enabled, now we check if the file exists, if it is writeable and delete it...
[2017-03-23 20:24:07] glsysbackup: [27771] Check if installed packages file: '/root/installed_packages_filename.txt' exists and if it is writeable...
[2017-03-23 20:24:07] glsysbackup: [27771] Installed packages file exists and is writeable.
[2017-03-23 20:24:07] glsysbackup: [27771] Deleting installed packages file...
[2017-03-23 20:24:08] glsysbackup: [27771] Deleting installed packages file was successful.
[2017-03-23 20:24:08] glsysbackup: [27771] We did a great job. :)
[2017-03-23 20:24:08] glsysbackup: [27771] Check if lock file: '/var/lock/glsysbackup' exists and if it is read and writeable...
[2017-03-23 20:24:08] glsysbackup: [27771] Lock file exists and it is read/writeable.
[2017-03-23 20:24:08] glsysbackup: [27771] Removing lock...
[2017-03-23 20:24:08] glsysbackup: [27771] Removing lock was successful.
[2017-03-23 20:24:08] glsysbackup: [27771] Script was running: '22' seconds.
[2017-03-23 20:24:08] glsysbackup: [27771] Bye, bye...
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
