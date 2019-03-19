[![Travis branch](https://img.shields.io/travis/ccztux/glsysbackup/master.svg?label=shellcheck%20%28master%29)](https://travis-ci.org/ccztux/glsysbackup)
[![Travis branch](https://img.shields.io/travis/ccztux/glsysbackup/devel.svg?label=shellcheck%20%28devel%29)](https://travis-ci.org/ccztux/glsysbackup)
[![Latest Release](https://img.shields.io/github/release/ccztux/glsysbackup.svg?label=latest%20release)](https://github.com/ccztux/glsysbackup/releases/latest)
[![Latest Pre-release](https://img.shields.io/badge/latest%20pre--release-v2.0.0--beta2-orange.svg)](https://github.com/ccztux/glsysbackup/releases/tag/2.0.0-beta2)
[![GitHub license](https://img.shields.io/badge/license-AGPL-blue.svg)](https://github.com/ccztux/glsysbackup/blob/master/LICENSE)



# glsysbackup
(**g**eneric **l**inux **sys**tem **backup**) is an advanced backup tool written in bash.



## Installation:

Download the latest tarball and extract it:

```bash
cd /tmp
wget "https://api.github.com/repos/ccztux/glsysbackup/tarball" -O glsysbackup.latest.tar.gz
tar -xvzf glsysbackup.latest.tar.gz
cd ccztux-glsysbackup-*
```


Copy the files:

```bash
cp -av ./usr/local/glsysbackup/ /usr/local/
cp -av etc/logrotate.d/glsysbackup /etc/logrotate.d/
```


Change the file ownership for your benefits:

```bash
chown -R root:root /usr/local/glsysbackup/
chown root:root /etc/logrotate.d/glsysbackup
chmod 644 /etc/logrotate.d/glsysbackup
```


Edit the config for your benefits:

```bash
vim /usr/local/glsysbackup/etc/glsysbackup.conf
```


Start glsysbackup:

```bash
/usr/local/glsysbackup/bin/glsysbackup -c /usr/local/glsysbackup/etc/glsysbackup.conf
```



## Example help output:
```
15:11:58 [pi@wurlitzer]:~$ sudo /usr/local/glsysbackup/bin/glsysbackup -h
Usage: glsysbackup OPTIONS

Author:				Christian Zettel (ccztux)
Last modification:	2019-03-19
Version:			2.0.0-beta3

Description:		glsysbackup (Generic Linux System Backup) is an advanced backup tool written in bash.

OPTIONS:
   -c		Path to config file. Example: '/usr/local/glsysbackup/etc/glsysbackup.conf'. (The file extension has to be: '.conf')
   -h		Shows this help.
   -v		Shows detailed version information.
```



## Configuration variables (default):
```bash
#---------
# Logging:
#---------

# enable log to file. (possible values: 1|0)
log_to_file="1"

# enable log to stdout (possible values: 1|0)
log_to_stdout="1"

# enable log to system logfile (possible values: 1|0)
log_to_syslog="0"

# timestamp format for log messages.
log_timestamp_format="%Y-%m-%d %H:%M:%S"

# truncate logfile at each backup cycle (possible values: 1|0)
log_to_file_truncate="0"



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

# rotation day of month for monthly rotation. 1 is monday. (possible values: 1|2|3|...|last day of month)
backup_rotation_monthly_day_of_month="1"

# number of backup files to keep of monthly backups
backup_rotation_monthly_max_backups="6"



#--------------------
# Installed packages:
#--------------------

# enable the creation of installed packages file (possible values: 1|0)
installed_packages_enabled="1"

# force this package manager to create installed packages file, if you have more than one package manager installed (possible values: rpm|dpkg|pacman|equery|pkgutil)
installed_packages_forced_manager=""

# path where installed packages file should be created
installed_packages_directory="/root"



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

# individual tar options
backup_individual_options=(
""
)

# set backup destination path
backup_destination_path="/var/backups"

# set backup filename
backup_base_filename="my_backup"

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
""
)



#------------
# Encryption:
#------------

# enable backup encryption with openssl (possible values: 1|0)
backup_encryption_enabled="0"

# set password for encryption
backup_encryption_password="test1234"



#-------------------
# Pre backup script:
#-------------------

# enable pre backup script functionality (possible values: 1|0)
pre_backup_script_enabled="0"

# path to pre backup script
pre_backup_script="/home/pi/pre.sh"

# exit glsysbackup in case execution of pre backup script was not successful
pre_backup_exit_when_unsuccessful="1"



#--------------------
# Post backup script:
#--------------------

# enable post backup script functionality (possible values: 1|0)
post_backup_script_enabled="0"

# path to post backup script
post_backup_script="/home/pi/post.sh"

# exit glsysbackup in case execution of post backup script was not successful
post_backup_exit_when_unsuccessful="1"
```



## Example job output (initial run with default config):
```
15:12:00 [pi@wurlitzer]:~$ sudo /usr/local/glsysbackup/bin/glsysbackup -c /usr/local/glsysbackup/etc/glsysbackup.conf
[2019-03-19 15:12:43] glsysbackup: [26411] glsysbackup 2.0.0-beta3 starting... (PID=26411)
[2019-03-19 15:12:43] glsysbackup: [26411] We are using config file: '/usr/local/glsysbackup/etc/glsysbackup.conf'.
[2019-03-19 15:12:43] glsysbackup: [26411] Check if root priviliges are required...
[2019-03-19 15:12:43] glsysbackup: [26411] Root privileges are required, checking privileges...
[2019-03-19 15:12:43] glsysbackup: [26411] HOORAY, we have root privileges. :)
[2019-03-19 15:12:43] glsysbackup: [26411] Get user which starts the script...
[2019-03-19 15:12:43] glsysbackup: [26411] glsysbackup was started by user: 'pi'.
[2019-03-19 15:12:43] glsysbackup: [26411] Checking bash version...
[2019-03-19 15:12:43] glsysbackup: [26411] Bash version: '4' meets requirements.
[2019-03-19 15:12:43] glsysbackup: [26411] Check if another instance of: 'glsysbackup' is already running...
[2019-03-19 15:12:43] glsysbackup: [26411] Check if lock file: '/usr/local/glsysbackup/var/lock/glsysbackup.glsysbackup.lock' exists and if it is read and writeable...
[2019-03-19 15:12:43] glsysbackup: [26411] Lock file doesnt exist.
[2019-03-19 15:12:43] glsysbackup: [26411] No other instance of: 'glsysbackup' is currently running (Neither lockfile: '/usr/local/glsysbackup/var/lock/glsysbackup.glsysbackup.lock' nor running processes detected).
[2019-03-19 15:12:44] glsysbackup: [26411] Check if script lock directory: '/usr/local/glsysbackup/var/lock' exists and permissions to set lock are ok...
[2019-03-19 15:12:44] glsysbackup: [26411] Script lock directory exists and permissions are ok.
[2019-03-19 15:12:44] glsysbackup: [26411] Setting lock...
[2019-03-19 15:12:44] glsysbackup: [26411] Setting lock was successful.
[2019-03-19 15:12:44] glsysbackup: [26411] Check if re-niceing is enabled...
[2019-03-19 15:12:44] glsysbackup: [26411] Re-niceing is not enabled.
[2019-03-19 15:12:44] glsysbackup: [26411] Check if re-ioniceing is enabled...
[2019-03-19 15:12:44] glsysbackup: [26411] Re-ioniceing is not enabled.
[2019-03-19 15:12:44] glsysbackup: [26411] Check backup dir structure...
[2019-03-19 15:12:44] glsysbackup: [26411] Check if backup directory: '/var/backups' exists and permissions to move files are ok...
[2019-03-19 15:12:44] glsysbackup: [26411] Backup directory exists and permissions are ok.
[2019-03-19 15:12:44] glsysbackup: [26411] Directory: '/var/backups/wurlitzer/glsysbackup/daily' exist, nothing to do.
[2019-03-19 15:12:44] glsysbackup: [26411] Directory: '/var/backups/wurlitzer/glsysbackup/weekly' exist, nothing to do.
[2019-03-19 15:12:44] glsysbackup: [26411] Directory: '/var/backups/wurlitzer/glsysbackup/monthly' exist, nothing to do.
[2019-03-19 15:12:44] glsysbackup: [26411] Directory: '/var/backups/wurlitzer/glsysbackup/latest' exist, nothing to do.
[2019-03-19 15:12:44] glsysbackup: [26411] Check if pre backup script functionality is enabled...
[2019-03-19 15:12:44] glsysbackup: [26411] Pre backup script functionality is not enabled.
[2019-03-19 15:12:44] glsysbackup: [26411] Check if installed packages are enabled...
[2019-03-19 15:12:44] glsysbackup: [26411] Installed packages are enabled.
[2019-03-19 15:12:44] glsysbackup: [26411] The supported system package manager we found is: 'dpkg'.
[2019-03-19 15:12:44] glsysbackup: [26411] Creating installed packages file: '/root/glsysbackup.glsysbackup.installed_packges.txt'...
[2019-03-19 15:12:44] glsysbackup: [26411] Creating installed packages file was successful, adding file to backup_items config array.
[2019-03-19 15:12:44] glsysbackup: [26411] Check if we need to build excluding options...
[2019-03-19 15:12:44] glsysbackup: [26411] We dont have to build excluding options...
[2019-03-19 15:12:44] glsysbackup: [26411] Building backup command...
[2019-03-19 15:12:44] glsysbackup: [26411] We build the following backup command: '/bin/tar --create --gzip --file=/var/backups/wurlitzer/glsysbackup/latest/my_backup.tar.gz --verbose --totals /etc/ /home/ /root/ /var/lib/mpd/ /usr/local/bin/ /boot/config.txt /root/glsysbackup.glsysbackup.installed_packges.txt'.
[2019-03-19 15:12:44] glsysbackup: [26411] Starting backup job...
[2019-03-19 15:12:44] glsysbackup: [26411] /bin/tar: Entferne führende „/“ von Elementnamen
[2019-03-19 15:12:44] glsysbackup: [26411] /etc/
[2019-03-19 15:12:44] glsysbackup: [26411] /etc/locale.alias
[2019-03-19 15:12:44] glsysbackup: [26411] /etc/usb_modeswitch.conf
[2019-03-19 15:12:44] glsysbackup: [26411] /etc/shadow
[2019-03-19 15:12:44] glsysbackup: [26411] /etc/timidity/
[2019-03-19 15:12:44] glsysbackup: [26411] /etc/timidity/freepats.cfg
.
.
[snip
.
.
[2019-03-19 15:14:03] glsysbackup: [26411] /usr/local/bin/
[2019-03-19 15:14:04] glsysbackup: [26411] /usr/local/bin/mpc_playlist_update
[2019-03-19 15:14:04] glsysbackup: [26411] /usr/local/bin/sysupdate
[2019-03-19 15:14:04] glsysbackup: [26411] /usr/local/bin/ctags
[2019-03-19 15:14:04] glsysbackup: [26411] /usr/local/bin/youtube-dl
[2019-03-19 15:14:04] glsysbackup: [26411] /usr/local/bin/proxy_switch
[2019-03-19 15:14:04] glsysbackup: [26411] /usr/local/bin/wurlitzerd
[2019-03-19 15:14:04] glsysbackup: [26411] /usr/local/bin/bashlib
[2019-03-19 15:14:04] glsysbackup: [26411] /boot/config.txt
[2019-03-19 15:14:04] glsysbackup: [26411] /bin/tar: Entferne führende „/“ von Zielen harter Verknüpfungen
[2019-03-19 15:14:04] glsysbackup: [26411] /root/glsysbackup.glsysbackup.installed_packges.txt
[2019-03-19 15:14:04] glsysbackup: [26411] Gesamtzahl geschriebener Bytes: 86108160 (83MiB, 1,2MiB/s)
[2019-03-19 15:14:04] glsysbackup: [26411] Backup job was successful.
[2019-03-19 15:14:04] glsysbackup: [26411] Backup encryption is not enabled...
[2019-03-19 15:14:04] glsysbackup: [26411] Check if post backup script functionality is enabled...
[2019-03-19 15:14:04] glsysbackup: [26411] Post backup script functionality is not enabled.
[2019-03-19 15:14:04] glsysbackup: [26411] Caught: 'EXIT', exiting script...
[2019-03-19 15:14:04] glsysbackup: [26411] Now we are doing some cleanup jobs...
[2019-03-19 15:14:04] glsysbackup: [26411] Installed packages functionality is enabled, now we check if the file exists, if it is writeable and delete it...
[2019-03-19 15:14:04] glsysbackup: [26411] Check if installed packages file: '/root/glsysbackup.glsysbackup.installed_packges.txt' exists and if it is writeable...
[2019-03-19 15:14:04] glsysbackup: [26411] Installed packages file exists and is writeable.
[2019-03-19 15:14:04] glsysbackup: [26411] Deleting installed packages file...
[2019-03-19 15:14:04] glsysbackup: [26411] Deleting installed packages file was successful.
[2019-03-19 15:14:04] glsysbackup: [26411] Checking for old, leftover backups in latest folder...
[2019-03-19 15:14:04] glsysbackup: [26411] Backup encryption is not enabled, now we check if an lefover encrypted backup file exists, if it is writeable and delete it...
[2019-03-19 15:14:04] glsysbackup: [26411] Check if encrypted backup file: '/var/backups/wurlitzer/glsysbackup/latest/my_backup.aes.tar.gz' exists and if it is writeable...
[2019-03-19 15:14:04] glsysbackup: [26411] Old, leftover encrypted backup found in latest folder: '/var/backups/wurlitzer/glsysbackup/latest/my_backup.aes.tar.gz' and we have write permission, so we delete it.
[2019-03-19 15:14:04] glsysbackup: [26411] Deleting file was successful.
[2019-03-19 15:14:04] glsysbackup: [26411] Check if backup rotation is enabled...
[2019-03-19 15:14:04] glsysbackup: [26411] Backup rotation is enabled.
[2019-03-19 15:14:04] glsysbackup: [26411] Check if only one backup per day is enabled...
[2019-03-19 15:14:04] glsysbackup: [26411] Only one backup per day is enabled.
[2019-03-19 15:14:04] glsysbackup: [26411] Check if old backups of type: 'daily' from today exists...
[2019-03-19 15:14:04] glsysbackup: [26411] Backup from today: '/var/backups/wurlitzer/glsysbackup/daily/daily_my_backup_2019-03-19_14h11m_Dienstag.tar.gz' found...
[2019-03-19 15:14:04] glsysbackup: [26411] Check if permissions of file: '/var/backups/wurlitzer/glsysbackup/daily/daily_my_backup_2019-03-19_14h11m_Dienstag.tar.gz' are ok to delete it...
[2019-03-19 15:14:04] glsysbackup: [26411] We have write permissions, so we can delete it.
[2019-03-19 15:14:04] glsysbackup: [26411] Deleting file was successful.
[2019-03-19 15:14:04] glsysbackup: [26411] Backup from today: '/var/backups/wurlitzer/glsysbackup/daily/daily_my_backup_2019-03-19_14h50m_Dienstag.aes.tar.gz' found...
[2019-03-19 15:14:04] glsysbackup: [26411] Check if permissions of file: '/var/backups/wurlitzer/glsysbackup/daily/daily_my_backup_2019-03-19_14h50m_Dienstag.aes.tar.gz' are ok to delete it...
[2019-03-19 15:14:04] glsysbackup: [26411] We have write permissions, so we can delete it.
[2019-03-19 15:14:04] glsysbackup: [26411] Deleting file was successful.
[2019-03-19 15:14:04] glsysbackup: [26411] Copying actual backup: '/var/backups/wurlitzer/glsysbackup/latest/my_backup.tar.gz' ==> '/var/backups/wurlitzer/glsysbackup/daily/daily_my_backup_2019-03-19_15h14m_Dienstag.tar.gz'.
[2019-03-19 15:14:04] glsysbackup: [26411] Copy job was successful.
[2019-03-19 15:14:04] glsysbackup: [26411] Check if rotation of daily backups is necessary...
[2019-03-19 15:14:04] glsysbackup: [26411] Rotation of daily backups is not necessary (backup_files_count: '2' <= backup_rotation_daily_max_backups: '10').
[2019-03-19 15:14:04] glsysbackup: [26411] Check if today is the day, where weekly backups should be rotated...
[2019-03-19 15:14:04] glsysbackup: [26411] Today is not the day, where we should rotate the weekly backups (actual_weekday: '2' != backup_rotation_weekly_weekday: '1').
[2019-03-19 15:14:04] glsysbackup: [26411] Check if today is the day, where monthly backups should be rotated...
[2019-03-19 15:14:04] glsysbackup: [26411] Today is not the day, where we should rotate the monthly backups (actual_day_of_month: '19' != backup_rotation_monthly_day_of_month: '1').
[2019-03-19 15:14:04] glsysbackup: [26411] Check if lock file: '/usr/local/glsysbackup/var/lock/glsysbackup.glsysbackup.lock' exists and if it is read and writeable...
[2019-03-19 15:14:04] glsysbackup: [26411] Lock file exists and it is read/writeable.
[2019-03-19 15:14:04] glsysbackup: [26411] Removing lock...
[2019-03-19 15:14:04] glsysbackup: [26411] Removing lock was successful.
[2019-03-19 15:14:04] glsysbackup: [26411] We did a great job. :)
[2019-03-19 15:14:04] glsysbackup: [26411] Script was running: '81' seconds, script_exit_code: '0'.
[2019-03-19 15:14:04] glsysbackup: [26411] Bye, bye...
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
