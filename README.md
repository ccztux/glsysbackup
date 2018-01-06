[![Travis branch](https://img.shields.io/travis/ccztux/glsysbackup/master.svg?label=build%20%28master%29)](https://travis-ci.org/ccztux/glsysbackup)
[![Travis branch](https://img.shields.io/travis/ccztux/glsysbackup/devel.svg?label=build%20%28devel%29)](https://travis-ci.org/ccztux/glsysbackup)
[![Latest Release](https://img.shields.io/github/release/ccztux/glsysbackup.svg?label=latest%20release)](https://github.com/ccztux/glsysbackup/releases/latest)
[![Latest Pre-release](https://img.shields.io/badge/latest%20pre--release-v1.0.1--beta1-orange.svg)](https://github.com/ccztux/glsysbackup/releases/tag/1.0.1-beta1)
[![GitHub license](https://img.shields.io/badge/license-AGPL-blue.svg)](https://github.com/ccztux/glsysbackup/blob/master/LICENSE)



# glsysbackup
(**g**eneric **l**inux **sys**tem **backup**) is an advanced backup tool written in bash.



## Example help output:
```
14:18:24 [pi@localhost]:~$ sudo ./glsysbackup -h
Usage: glsysbackup OPTIONS

Author:			Christian Zettel (ccztux)
Last modification:	2018-01-06
Version:		2.0.0-alpha1

Description:		glsysbackup (Generic Linux System Backup) is an advanced backup tool written in bash.

OPTIONS:
   -h		Shows this help.
   -c		Path to config file. The file extension has to be: '.conf'. (If undefined, default value: '/usr/local/glsysbackup/etc/glsysbackup.conf' will be used.)
   -o		Override lock in case glsysbackup was terminated abnormally.
   -v		Shows detailed version information.
```



## Configuration variables (default):
```bash
#---------
# Logging:
#---------

# enable log to file. (possible values: 1|0)
log_to_file="1"

# log directory
log_directory="${script_base_path}/var/log/"

# filename of logfile
log_filename="${script_config_name}.log"

# enable log to stdout (possible values: 1|0)
log_to_stdout="1"

# enable log to system logfile (possible values: 1|0)
log_to_syslog="0"

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

# keep max 1 backup a day
backup_rotation_one_backup_per_day_only="1"

# enable daily backup rotation (possible values: 1|0)
backup_rotation_daily_enabled="1"

# number of backup files to keep of daily backups
backup_rotation_daily_max_backups="10"

# enable weekly backup rotation (possible values: 1|0)
backup_rotation_weekly_enabled="1"

# rotation weekday for weekly rotation. 1 is monday. (possible values: 1|2|3|4|5|6|7)
backup_rotation_weekly_weekday="1"

# number of backup files to keep of weekly backups
backup_rotation_weekly_max_backups="8"

# enable monthly backup rotation (possible values: 1|0)
backup_rotation_monthly_enabled="1"

# rotation weekday for monthly rotation. 1 is monday. (possible values: 1|2|3|4|5|6|7)
backup_rotation_monthly_weekday="1"

# number of backup files to keep of monthly backups
backup_rotation_monthly_max_backups="6"



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

# show backup totals
backup_show_totals="1"

# set backup destination path
backup_destination_path="/var/backups"

# set backup filename
backup_filename_prefix="my_backup"

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



## Example job output (initial run with default config):
```
14:18:24 [pi@localhost]:~$ sudo ./glsysbackup 
[2018-01-06 14:18:24] glsysbackup: [24563] glsysbackup 2.0.0-alpha1 starting... (PID=24563)
[2018-01-06 14:18:25] glsysbackup: [24563] We are using config file: '/usr/local/glsysbackup/etc/glsysbackup.conf'.
[2018-01-06 14:18:25] glsysbackup: [24563] Check if root priviliges are required...
[2018-01-06 14:18:25] glsysbackup: [24563] Root privileges are required, checking privileges...
[2018-01-06 14:18:25] glsysbackup: [24563] HOORAY, we have root privileges. :)
[2018-01-06 14:18:25] glsysbackup: [24563] Get user which starts the script...
[2018-01-06 14:18:25] glsysbackup: [24563] glsysbackup was started by user: 'pi'.
[2018-01-06 14:18:25] glsysbackup: [24563] Checking bash version...
[2018-01-06 14:18:25] glsysbackup: [24563] Bash version: '4' meets requirements.
[2018-01-06 14:18:25] glsysbackup: [24563] Check if another instance of: 'glsysbackup' is already running...
[2018-01-06 14:18:25] glsysbackup: [24563] Check if lock file: '/usr/local/glsysbackup/var/lock/glsysbackup.lock' exists and if it is read and writeable...
[2018-01-06 14:18:25] glsysbackup: [24563] Lock file doesnt exist.
[2018-01-06 14:18:25] glsysbackup: [24563] No other instance of: 'glsysbackup' is currently running (Lockfile: '/usr/local/glsysbackup/var/lock/glsysbackup.lock' doesnt exist and no processes are running).
[2018-01-06 14:18:25] glsysbackup: [24563] Check if script lock directory: '/usr/local/glsysbackup/var/lock' exists and permissions to set lock are ok...
[2018-01-06 14:18:25] glsysbackup: [24563] Script lock directory exists and permissions are ok.
[2018-01-06 14:18:25] glsysbackup: [24563] Setting lock...
[2018-01-06 14:18:25] glsysbackup: [24563] Setting lock was successful.
[2018-01-06 14:18:25] glsysbackup: [24563] Check if re-niceing is enabled...
[2018-01-06 14:18:25] glsysbackup: [24563] Re-niceing is not enabled.
[2018-01-06 14:18:25] glsysbackup: [24563] Check if re-ioniceing is enabled...
[2018-01-06 14:18:25] glsysbackup: [24563] Re-ioniceing is not enabled.
[2018-01-06 14:18:25] glsysbackup: [24563] Check backup dir structure...
[2018-01-06 14:18:25] glsysbackup: [24563] Check if backup directory: '/var/backups' exists and permissions to move files are ok...
[2018-01-06 14:18:25] glsysbackup: [24563] Backup directory exists and permissions are ok.
[2018-01-06 14:18:25] glsysbackup: [24563] Directory: '/var/backups/wurlitzer/glsysbackup/daily' doesnt exist.
[2018-01-06 14:18:25] glsysbackup: [24563] Creating directory...
[2018-01-06 14:18:25] glsysbackup: [24563] Creating directory: '/var/backups/wurlitzer/glsysbackup/daily' was successful.
[2018-01-06 14:18:25] glsysbackup: [24563] Directory: '/var/backups/wurlitzer/glsysbackup/weekly' doesnt exist.
[2018-01-06 14:18:25] glsysbackup: [24563] Creating directory...
[2018-01-06 14:18:25] glsysbackup: [24563] Creating directory: '/var/backups/wurlitzer/glsysbackup/weekly' was successful.
[2018-01-06 14:18:25] glsysbackup: [24563] Directory: '/var/backups/wurlitzer/glsysbackup/monthly' doesnt exist.
[2018-01-06 14:18:25] glsysbackup: [24563] Creating directory...
[2018-01-06 14:18:26] glsysbackup: [24563] Creating directory: '/var/backups/wurlitzer/glsysbackup/monthly' was successful.
[2018-01-06 14:18:26] glsysbackup: [24563] Directory: '/var/backups/wurlitzer/glsysbackup/latest' doesnt exist.
[2018-01-06 14:18:26] glsysbackup: [24563] Creating directory...
[2018-01-06 14:18:26] glsysbackup: [24563] Creating directory: '/var/backups/wurlitzer/glsysbackup/latest' was successful.
[2018-01-06 14:18:26] glsysbackup: [24563] Check if pre backup script functionality is enabled...
[2018-01-06 14:18:26] glsysbackup: [24563] Pre backup script functionality is enabled.
[2018-01-06 14:18:26] glsysbackup: [24563] Check if pre backup script (/home/pi/pre.sh) exists and if it is executeable...
[2018-01-06 14:18:26] glsysbackup: [24563] Pre backup script exists and it is executeable, so we execute it...
[2018-01-06 14:18:26] glsysbackup: [24563] Doing something here in pre script.... 1
[2018-01-06 14:18:26] glsysbackup: [24563] Doing something here in pre script.... 2
[2018-01-06 14:18:26] glsysbackup: [24563] Doing something here in pre script.... 3
[2018-01-06 14:18:26] glsysbackup: [24563] Doing something here in pre script.... 4
[2018-01-06 14:18:26] glsysbackup: [24563] Doing something here in pre script.... 5
[2018-01-06 14:18:26] glsysbackup: [24563] Execution of pre backup script was successful. rc: '0'.
[2018-01-06 14:18:26] glsysbackup: [24563] Check if installed packages are enabled...
[2018-01-06 14:18:26] glsysbackup: [24563] Installed packages are enabled.
[2018-01-06 14:18:26] glsysbackup: [24563] The supported system package manager we found is: 'dpkg'.
[2018-01-06 14:18:26] glsysbackup: [24563] Creating installed packages file: '/root/installed_syspackages.txt'...
[2018-01-06 14:18:27] glsysbackup: [24563] Creating installed packages file was successful, adding file to backup_items config array.
[2018-01-06 14:18:27] glsysbackup: [24563] Check if we need to build excluding options...
[2018-01-06 14:18:27] glsysbackup: [24563] Building of excluding options is enabled.
[2018-01-06 14:18:27] glsysbackup: [24563] We build the following excluding options: '--exclude='/old.backups' --exclude='/old.mysqldumps/''.
[2018-01-06 14:18:27] glsysbackup: [24563] Building backup options...
[2018-01-06 14:18:27] glsysbackup: [24563] We build the following backup options: '--create --gzip --file=/var/backups/wurlitzer/glsysbackup/latest/my_backup.tar.gz --verbose --totals'.
[2018-01-06 14:18:27] glsysbackup: [24563] Starting backup job...
[2018-01-06 14:18:27] glsysbackup: [24563] /bin/tar: Entferne führende „/“ von Elementnamen
[2018-01-06 14:18:27] glsysbackup: [24563] /etc/
[2018-01-06 14:18:27] glsysbackup: [24563] /etc/locale.alias
[2018-01-06 14:18:27] glsysbackup: [24563] /etc/subgid
[2018-01-06 14:18:27] glsysbackup: [24563] /etc/sgml/
[2018-01-06 14:18:27] glsysbackup: [24563] /etc/sgml/xml-core.cat
[2018-01-06 14:18:27] glsysbackup: [24563] /etc/sgml/catalog
[2018-01-06 14:18:27] glsysbackup: [24563] /etc/securetty
.
.
.
[snip]
.
.
.
[2018-01-06 14:19:11] glsysbackup: [24563] /var/lib/mpd/state
[2018-01-06 14:19:11] glsysbackup: [24563] /usr/local/bin/
[2018-01-06 14:19:11] glsysbackup: [24563] /usr/local/bin/mpc_playlist_update
[2018-01-06 14:19:11] glsysbackup: [24563] /usr/local/bin/youtube-dl
[2018-01-06 14:19:11] glsysbackup: [24563] /usr/local/bin/wurlitzerd
[2018-01-06 14:19:11] glsysbackup: [24563] /usr/local/bin/bashlib
[2018-01-06 14:19:11] glsysbackup: [24563] /usr/local/bin/glsysbackup
[2018-01-06 14:19:11] glsysbackup: [24563] /usr/local/bin/sysupdate
[2018-01-06 14:19:11] glsysbackup: [24563] /boot/config.txt
[2018-01-06 14:19:11] glsysbackup: [24563] /bin/tar: Entferne führende „/“ von Zielen harter Verknüpfungen
[2018-01-06 14:19:11] glsysbackup: [24563] /root/installed_syspackages.txt
[2018-01-06 14:19:11] glsysbackup: [24563] Gesamtzahl geschriebener Bytes: 8980480 (8,6MiB, 465KiB/s)
[2018-01-06 14:19:11] glsysbackup: [24563] Backup job was successful.
[2018-01-06 14:19:11] glsysbackup: [24563] Backup encryption is enabled...
[2018-01-06 14:19:11] glsysbackup: [24563] Encrypting backup file: '/var/backups/wurlitzer/glsysbackup/latest/my_backup.tar.gz'...
[2018-01-06 14:19:12] glsysbackup: [24563] Encrypting job was successful.
[2018-01-06 14:19:12] glsysbackup: [24563] HINT: To decrypt use this command: '/usr/bin/openssl aes-256-cbc -d -salt -in /var/backups/wurlitzer/glsysbackup/latest/my_backup.aes.tar.gz -out /var/backups/wurlitzer/glsysbackup/latest/my_backup.tar.gz'.
[2018-01-06 14:19:12] glsysbackup: [24563] Check if post backup script functionality is enabled...
[2018-01-06 14:19:12] glsysbackup: [24563] Post backup script functionality is enabled.
[2018-01-06 14:19:12] glsysbackup: [24563] Check if post backup script (/home/pi/post.sh) exists and if it is executeable...
[2018-01-06 14:19:12] glsysbackup: [24563] Post backup script exists and it is executeable, so we execute it...
[2018-01-06 14:19:12] glsysbackup: [24563] Doing something here in post script.... 1
[2018-01-06 14:19:12] glsysbackup: [24563] Doing something here in post script.... 2
[2018-01-06 14:19:12] glsysbackup: [24563] Doing something here in post script.... 3
[2018-01-06 14:19:12] glsysbackup: [24563] Doing something here in post script.... 4
[2018-01-06 14:19:12] glsysbackup: [24563] Doing something here in post script.... 5
[2018-01-06 14:19:12] glsysbackup: [24563] Execution of post backup script was successful. rc: '0'.
[2018-01-06 14:19:12] glsysbackup: [24563] Caught: 'EXIT', exiting script...
[2018-01-06 14:19:12] glsysbackup: [24563] Now we are doing some cleanup jobs...
[2018-01-06 14:19:12] glsysbackup: [24563] Backup encryption is enabled, now we check if unencrypted backup file exists, if it is writeable and delete it...
[2018-01-06 14:19:12] glsysbackup: [24563] Check if unencrypted backup file: '/var/backups/wurlitzer/glsysbackup/latest/my_backup.tar.gz' exists and if it is writeable...
[2018-01-06 14:19:13] glsysbackup: [24563] Unencrypted backup file  exists and it is writeable.
[2018-01-06 14:19:13] glsysbackup: [24563] Deleting unencrypted backup file...
[2018-01-06 14:19:13] glsysbackup: [24563] Deleting unencrypted backup file was successful.
[2018-01-06 14:19:13] glsysbackup: [24563] Installed packages functionality is enabled, now we check if the file exists, if it is writeable and delete it...
[2018-01-06 14:19:13] glsysbackup: [24563] Check if installed packages file: '/root/installed_syspackages.txt' exists and if it is writeable...
[2018-01-06 14:19:13] glsysbackup: [24563] Installed packages file exists and is writeable.
[2018-01-06 14:19:13] glsysbackup: [24563] Deleting installed packages file...
[2018-01-06 14:19:13] glsysbackup: [24563] Deleting installed packages file was successful.
[2018-01-06 14:19:13] glsysbackup: [24563] Check if backup rotation is enabled...
[2018-01-06 14:19:13] glsysbackup: [24563] Backup rotation is enabled.
[2018-01-06 14:19:13] glsysbackup: [24563] Do the daily rotation...
[2018-01-06 14:19:13] glsysbackup: [24563] Check if old backups of type: 'daily' from today exists...
[2018-01-06 14:19:13] glsysbackup: [24563] No old backups of type: 'daily' from today found.
[2018-01-06 14:19:13] glsysbackup: [24563] Copying actual backup: '/var/backups/wurlitzer/glsysbackup/latest/my_backup.aes.tar.gz' ==> '/var/backups/wurlitzer/glsysbackup/daily/daily_my_backup_2018-01-06_14h19m_Samstag.aes.tar.gz'.
[2018-01-06 14:19:13] glsysbackup: [24563] Copy job was successful.
[2018-01-06 14:19:13] glsysbackup: [24563] Check if rotation of daily backups is necessary...
[2018-01-06 14:19:13] glsysbackup: [24563] Rotation of daily backups is not necessary (Number of daily backup files: '1' <= max daily backup files: '10').
[2018-01-06 14:19:13] glsysbackup: [24563] Check if rotation of weekly backups is necessary...
[2018-01-06 14:19:13] glsysbackup: [24563] Rotation of weekly backups is not necessary (Number of weekly backup files: '1' <= max weekly backup files: '8').
[2018-01-06 14:19:13] glsysbackup: [24563] Check if rotation of monthly backups is necessary...
[2018-01-06 14:19:13] glsysbackup: [24563] Rotation of monthly backups is not necessary (Number of monthly backup files: '1' <= max monthly backup files: '6').
[2018-01-06 14:19:13] glsysbackup: [24563] We did a great job. :)
[2018-01-06 14:19:13] glsysbackup: [24563] Check if lock file: '/usr/local/glsysbackup/var/lock/glsysbackup.lock' exists and if it is read and writeable...
[2018-01-06 14:19:13] glsysbackup: [24563] Lock file exists and it is read/writeable.
[2018-01-06 14:19:13] glsysbackup: [24563] Removing lock...
[2018-01-06 14:19:13] glsysbackup: [24563] Removing lock was successful.
[2018-01-06 14:19:13] glsysbackup: [24563] Script was running: '49' seconds, script_exit_code: '0'.
[2018-01-06 14:19:13] glsysbackup: [24563] Bye, bye...
```



## Backupfile structure with enabled openssl encryption and backup rotation:
```
14:21:33 [pi@wurlitzer]:~$ find /var/backups/wurlitzer/
/var/backups/wurlitzer/
/var/backups/wurlitzer/glsysbackup
/var/backups/wurlitzer/glsysbackup/weekly
/var/backups/wurlitzer/glsysbackup/latest
/var/backups/wurlitzer/glsysbackup/latest/my_backup.aes.tar.gz
/var/backups/wurlitzer/glsysbackup/monthly
/var/backups/wurlitzer/glsysbackup/daily
/var/backups/wurlitzer/glsysbackup/daily/daily_my_backup_2018-01-06_14h19m_Samstag.aes.tar.gz
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
9. Create backup directory structure
10. Execute the pre-backup-script, if it is defined and executeable
11. Create file with installed packages
12. Build excluding options from config array
13. Build the backup optoins from config
14. Make the tar backup
15. Enrypt the backup with openssl if required
16. Execute the post-backup-script, if it is defined and executeable
11. Rotation of backup files if backup/encryption was successful



## It requires the following binaries:
- **bash** (Version 3 || 4)
- **which** to get the full path to the required binaries through environment variable $PATH
- **pgrep** to check if an instance of glsysbackup is already running
- **whoami** to check the user who executes glsysbackup
- **date** for logging purposes (Only required if bash version < 4.2. Else printf bash builtin will be used.)
- **tar** to create the backup
- **rm** to delete files
- **cp** to copy files
- **mkdir** to create directories
- **kill** to send kill signal to glsysbackup



## Optionally used binaries:
- **logger** to log to the system log
- **rpm** || **dpkg** || **pacman** || **equery** || **pkgutil** to create a file with installed packages
- **openssl** to encrypt the backup file
- **renice** to renice glsysbackup and all child processes
- **ionice** to re-ionice glsysbackup and all child processes



## Future plans:
- add sendEmail functionality
