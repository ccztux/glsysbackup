[![Travis branch](https://img.shields.io/travis/ccztux/glsysbackup/master.svg?label=shellcheck%20%28master%29)](https://travis-ci.org/ccztux/glsysbackup)
[![Travis branch](https://img.shields.io/travis/ccztux/glsysbackup/devel.svg?label=shellcheck%20%28devel%29)](https://travis-ci.org/ccztux/glsysbackup)
[![Latest Release](https://img.shields.io/github/release/ccztux/glsysbackup.svg?label=latest%20release)](https://github.com/ccztux/glsysbackup/releases/latest)
[![Latest Pre-release](https://img.shields.io/badge/latest%20pre--release-v2.0.0--beta4-orange.svg)](https://github.com/ccztux/glsysbackup/releases/tag/2.0.0-beta4)
[![GitHub license](https://img.shields.io/badge/license-AGPL-blue.svg)](https://github.com/ccztux/glsysbackup/blob/master/LICENSE)



# glsysbackup
(**g**eneric **l**inux **sys**tem **backup**) is an advanced backup tool written in bash.


## Features:
- Lock functionality. Only one instance is possible to run. (Lock file and check with pgrep)
- Verbose logging to stdout and/or system logfile and/or system journal and/or individual logfile.
- Excluding of files
- Backupfile rotation (daily || weekly || monthly)
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
3. Check if glsysbackup should be exectuted with root privileges
4. Get and log the user, who starts glsysbackup
5. Check if bash version meets requirements
6. Check if an instance is already running via lockfile and pgrep
7. Re-nice glsysbackup if required
8. Re-ionice glsysbackup if required
9. Create backup directory structure
10. Execute the pre-backup-script, if it is defined and executeable
11. Create file with installed packages
12. Build excluding options from config array
13. Build the backup options from config
14. Make the tar backup
15. Enrypt the backup with openssl if required
16. Execute the post-backup-script, if it is defined and executeable
11. Rotation of backup files if backup/encryption was successful



## It requires the following binaries:
- **bash** (Version >= 3)
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
- **systemd-cat** to log to the system journal
- **rpm** || **dpkg** || **pacman** || **equery** || **pkgutil** to create a file with installed packages
- **openssl** to encrypt the backup file
- **renice** to renice glsysbackup and all child processes
- **ionice** to re-ionice glsysbackup and all child processes

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
cp -av ./etc/logrotate.d/glsysbackup /etc/logrotate.d/
```


Change the file ownership:

```bash
chown -R root:root /usr/local/glsysbackup/
chown root:root /etc/logrotate.d/glsysbackup
chmod 644 /etc/logrotate.d/glsysbackup
```


Edit the config:

```bash
vim /usr/local/glsysbackup/etc/glsysbackup.conf
```


Start glsysbackup:

```bash
/usr/local/glsysbackup/bin/glsysbackup -c /usr/local/glsysbackup/etc/glsysbackup.conf
```



## Example help output:
```
Usage: glsysbackup OPTIONS

Author:				Christian Zettel (ccztux)
Last modification:	2019-11-17
Version:			2.0.0-beta4

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

# enable log to file
# (possible values: 1|0)
log_to_file="1"

# enable log to stdout
# (possible values: 1|0)
log_to_stdout="1"

# enable log to system logfile
# (possible values: 1|0)
log_to_syslog="0"

# enable log to system journal
# (possible values: 1|0)
log_to_journal="0"

# timestamp format for log messages
# (HINT: have a look at: 'man strftime')
log_timestamp_format="%Y-%m-%d %H:%M:%S"

# truncate logfile at each backup cycle
# (possible values: 1|0)
log_to_file_truncate="0"



#------------
# Privileges:
#------------

# enable this to ensure glsysbackup is running with root privileges
# (possible values: 1|0)
root_privileges_required="1"



#--------
# Renice:
#--------

# enable reniceing of glsysbackup and child procs
# (possible values: 1|0)
re_nice_enabled="0"

# set renice priority
# (possible values: -20...19)
# (HINT: have a look at: 'man renice')
re_nice_priority="19"



#-----------
# Re-ionice:
#-----------

# enable re-ioniceing of glsysbackup and child procs
# (possible values: 1|0)
re_ionice_enabled="0"

# set re-ionice scheduling class
# (possible values: 0|1|2|3)
# (HINT: have a look at: 'man ionice')
re_ionice_scheduling_class="2"

# set re-ionice priority
# (possible values: 0...7)
# (HINT: have a look at: 'man ionice')
re_ionice_priority="7"



#----------
# Rotation:
#----------

# keep max 1 backup a day
backup_rotation_one_backup_per_day_only="1"

# enable daily backup rotation
# (possible values: 1|0)
backup_rotation_daily_enabled="1"

# number of backup files to keep of daily backups
backup_rotation_daily_max_backups="10"

# enable weekly backup rotation
# (possible values: 1|0)
backup_rotation_weekly_enabled="1"

# rotation weekday for weekly rotation (1 is monday)
# (possible values: 1|2|3|4|5|6|7)
backup_rotation_weekly_weekday="1"

# number of backup files to keep of weekly backups
backup_rotation_weekly_max_backups="8"

# enable monthly backup rotation
# (possible values: 1|0)
backup_rotation_monthly_enabled="1"

# rotation day of month for monthly rotation (1 is monday)
# (possible values: 1|2|3|...|last day of month)
backup_rotation_monthly_day_of_month="1"

# number of backup files to keep of monthly backups
backup_rotation_monthly_max_backups="6"



#--------------------
# Installed packages:
#--------------------

# enable the creation of installed packages file
# (possible values: 1|0)
installed_packages_enabled="1"

# force this package manager to create installed packages file, if you have more than one package
# manager installed
# (possible values: rpm|dpkg|pacman|equery|pkgutil)
installed_packages_forced_manager=""

# path where installed packages file should be created
installed_packages_directory="/root"



#--------
# Backup:
#--------

# if this value is less equal than the tar rc, the backup job will be interpreted
# as 'backup successful'
# (possible values: 0|1|2)
# (HINT: have a look at: 'man tar' section: 'RETURN VALUE')
backup_successful_tar_rc="1"

# enable backup compression
# (possible values: 1|0)
backup_compression_enabled="1"

# backup compression type
# (possible values: gzip|bzip2|xz|lzip|lzma|lzop)
backup_compression_type="gzip"

# enable backup verbose mode
backup_verbose_mode_enabled="1"

# show backup totals
# (possible values: 1|0)
backup_show_totals="1"

# individual tar options
# (HINT: have a look at: 'man tar')
backup_individual_options=(
""
)

# set backup destination path
backup_destination_path="/var/backups"

# set backup filename
backup_base_filename="${script_config_name}"

# files and folders you want to backup
backup_items=(
"/etc/"
"/home/"
"/root/"
"/var/lib/mpd/"
"/usr/local/bin/"
"/boot/config.txt"
)

# exclude this items from backup
# (HINT: have a look at: 'man tar')
backup_exlude_items=(
""
)



#------------
# Encryption:
#------------

# enable backup encryption with openssl
# (possible values: 1|0)
backup_encryption_enabled="0"

# set password for encryption
backup_encryption_password="test1234"



#-------------------
# Pre backup script:
#-------------------

# enable pre backup script functionality
# (possible values: 1|0)
pre_backup_script_enabled="0"

# path to pre backup script
pre_backup_script="/home/pi/pre.sh"

# exit glsysbackup in case execution of pre backup script was not successful
# (possible values: 1|0)
pre_backup_exit_when_unsuccessful="1"



#--------------------
# Post backup script:
#--------------------

# enable post backup script functionality
# (possible values: 1|0)
post_backup_script_enabled="0"

# path to post backup script
post_backup_script="/home/pi/post.sh"

# exit glsysbackup in case execution of post backup script was not successful
# (possible values: 1|0)
post_backup_exit_when_unsuccessful="1"
```



## Example job output:
```
13:55:45 [pi@wurlitzer]:~$ sudo /usr/local/glsysbackup/bin/glsysbackup -c /usr/local/glsysbackup/etc/systembackup.conf
2019-11-17 13:55:49 |  24728 |    checkLogHandlerRequirements | glsysbackup 2.0.0-beta4 starting... (PID=24728)
2019-11-17 13:55:49 |  24728 |    checkLogHandlerRequirements | We are using config file: '/usr/local/glsysbackup/etc/systembackup.conf'.
2019-11-17 13:55:49 |  24728 |            checkRootPrivileges | Check if root priviliges are required...
2019-11-17 13:55:49 |  24728 |            checkRootPrivileges | Root privileges are required, checking privileges...
2019-11-17 13:55:49 |  24728 |            checkRootPrivileges | HOORAY, we have root privileges. :)
2019-11-17 13:55:49 |  24728 |                        getUser | Get user which starts the script...
2019-11-17 13:55:49 |  24728 |                        getUser | glsysbackup was started by user: 'pi'.
2019-11-17 13:55:49 |  24728 |               checkBashVersion | Checking bash version...
2019-11-17 13:55:49 |  24728 |               checkBashVersion | Bash version: '5' meets requirements.
2019-11-17 13:55:49 |  24728 |    checkAlreadyRunningInstance | Check if another instance of: 'glsysbackup' is already running...
2019-11-17 13:55:49 |  24728 |                      checkLock | Check if lock file: '/usr/local/glsysbackup/var/lock/glsysbackup.systembackup.lock' exists and if it is read and writeable...
2019-11-17 13:55:49 |  24728 |                      checkLock | Lock file doesnt exist.
2019-11-17 13:55:49 |  24728 |    checkAlreadyRunningInstance | No other instance of: 'glsysbackup' is currently running (Neither lockfile: '/usr/local/glsysbackup/var/lock/glsysbackup.systembackup.lock' nor running processes detected).
2019-11-17 13:55:49 |  24728 |                        setLock | Check if script lock directory: '/usr/local/glsysbackup/var/lock' exists and permissions to set lock are ok...
2019-11-17 13:55:49 |  24728 |                        setLock | Script lock directory exists and permissions are ok.
2019-11-17 13:55:49 |  24728 |                        setLock | Setting lock...
2019-11-17 13:55:49 |  24728 |                        setLock | Setting lock was successful.
2019-11-17 13:55:49 |  24728 |                         reNice | Check if re-niceing is enabled...
2019-11-17 13:55:49 |  24728 |                         reNice | Re-niceing is not enabled.
2019-11-17 13:55:49 |  24728 |                       reIONice | Check if re-ioniceing is enabled...
2019-11-17 13:55:49 |  24728 |                       reIONice | Re-ioniceing is not enabled.
2019-11-17 13:55:49 |  24728 |        checkBackupDirStructure | Check backup dir structure...
2019-11-17 13:55:49 |  24728 |        checkBackupDirStructure | Check if backup directory: '/var/backups' exists and permissions to move files are ok...
2019-11-17 13:55:49 |  24728 |        checkBackupDirStructure | Backup directory exists and permissions are ok.
2019-11-17 13:55:49 |  24728 |        checkBackupDirStructure | Directory: '/var/backups/wurlitzer/systembackup/daily' exist, nothing to do.
2019-11-17 13:55:49 |  24728 |        checkBackupDirStructure | Directory: '/var/backups/wurlitzer/systembackup/weekly' exist, nothing to do.
2019-11-17 13:55:49 |  24728 |        checkBackupDirStructure | Directory: '/var/backups/wurlitzer/systembackup/monthly' exist, nothing to do.
2019-11-17 13:55:49 |  24728 |        checkBackupDirStructure | Directory: '/var/backups/wurlitzer/systembackup/latest' exist, nothing to do.
2019-11-17 13:55:49 |  24728 |         executePreBackupScript | Check if pre backup script functionality is enabled...
2019-11-17 13:55:49 |  24728 |         executePreBackupScript | Pre backup script functionality is not enabled.
2019-11-17 13:55:49 |  24728 |    createInstalledPackagesFile | Check if installed packages are enabled...
2019-11-17 13:55:49 |  24728 |    createInstalledPackagesFile | Installed packages are enabled.
2019-11-17 13:55:49 |  24728 |    createInstalledPackagesFile | The supported system package manager we found is: 'dpkg'.
2019-11-17 13:55:49 |  24728 |    createInstalledPackagesFile | Creating installed packages file: '/root/glsysbackup.systembackup.installed_packges.txt'...
2019-11-17 13:55:50 |  24728 |    createInstalledPackagesFile | Creating installed packages file was successful, adding file to backup_items config array.
2019-11-17 13:55:50 |  24728 |               buildExcludeOpts | Check if we need to build excluding options...
2019-11-17 13:55:50 |  24728 |               buildExcludeOpts | Building of excluding options is enabled.
2019-11-17 13:55:50 |  24728 |               buildExcludeOpts | We build the following excluding options: '--exclude=/home/pi/old --exclude=/home/pi/recovery'.
2019-11-17 13:55:50 |  24728 |                 buildBackupCmd | Building backup command...
2019-11-17 13:55:50 |  24728 |                 buildBackupCmd | We build the following backup command: '/bin/tar --exclude=/home/pi/old --exclude=/home/pi/recovery --create --gzip --file=/var/backups/wurlitzer/systembackup/latest/systembackup.tar.gz --verbose --totals /boot/ /etc/ /home/ /root/ /var/lib/mpd/ /usr/local/bin/ /usr/lib/cgi-bin/ /root/glsysbackup.systembackup.installed_packges.txt'.
2019-11-17 13:55:50 |  24728 |                     makeBackup | Starting backup job...
2019-11-17 13:55:50 |  24728 |                     makeBackup | /bin/tar: Removing leading /' from member names
2019-11-17 13:55:50 |  24728 |                     makeBackup | /boot/
2019-11-17 13:55:50 |  24728 |                     makeBackup | /boot/overlays/
2019-11-17 13:55:50 |  24728 |                     makeBackup | /boot/overlays/act-led.dtbo
2019-11-17 13:55:50 |  24728 |                     makeBackup | /bin/tar: Removing leading /' from hard link targets
2019-11-17 13:55:50 |  24728 |                     makeBackup | /boot/overlays/README
2019-11-17 13:55:50 |  24728 |                     makeBackup | /boot/overlays/akkordion-iqdacplus.dtbo
2019-11-17 13:55:50 |  24728 |                     makeBackup | /boot/overlays/adau1977-adc.dtbo
2019-11-17 13:55:50 |  24728 |                     makeBackup | /boot/overlays/adau7002-simple.dtbo
2019-11-17 13:55:50 |  24728 |                     makeBackup | /boot/overlays/ads1015.dtbo
2019-11-17 13:55:50 |  24728 |                     makeBackup | /boot/overlays/ads1115.dtbo
2019-11-17 13:55:50 |  24728 |                     makeBackup | /boot/overlays/ads7846.dtbo
.
.
[snip]
.
.
2019-11-17 13:57:11 |  24728 |                     makeBackup | /usr/lib/cgi-bin/wurlitzer/gallery.css
2019-11-17 13:57:11 |  24728 |                     makeBackup | /usr/lib/cgi-bin/wurlitzer/gallery.cgi
2019-11-17 13:57:11 |  24728 |                     makeBackup | /usr/lib/cgi-bin/wurlitzer/history.csv
2019-11-17 13:57:11 |  24728 |                     makeBackup | /usr/lib/cgi-bin/wurlitzer/js/
2019-11-17 13:57:11 |  24728 |                     makeBackup | /usr/lib/cgi-bin/wurlitzer/js/wurlitzer_title.js
2019-11-17 13:57:11 |  24728 |                     makeBackup | /usr/lib/cgi-bin/wurlitzer/img/
2019-11-17 13:57:11 |  24728 |                     makeBackup | /usr/lib/cgi-bin/wurlitzer/img/youtube_logo.png
2019-11-17 13:57:11 |  24728 |                     makeBackup | /usr/lib/cgi-bin/wurlitzer/img/favicon.ico
2019-11-17 13:57:11 |  24728 |                     makeBackup | /usr/lib/cgi-bin/wurlitzer/img/music.png
2019-11-17 13:57:11 |  24728 |                     makeBackup | /root/glsysbackup.systembackup.installed_packges.txt
2019-11-17 13:57:11 |  24728 |                     makeBackup | Total bytes written: 68956160 (66MiB, 1.1MiB/s)
2019-11-17 13:57:11 |  24728 |                     makeBackup | Backup job was successful. (tar return value: '0' <= backup_successful_tar_rc: '1')
2019-11-17 13:57:11 |  24728 |                  encryptBackup | Backup encryption is not enabled...
2019-11-17 13:57:11 |  24728 |        executePostBackupScript | Check if post backup script functionality is enabled...
2019-11-17 13:57:11 |  24728 |        executePostBackupScript | Post backup script functionality is not enabled.
2019-11-17 13:57:11 |  24728 |                  signalHandler | Caught: 'EXIT', exiting script...
2019-11-17 13:57:11 |  24728 |                        cleanUp | Now we are doing some cleanup jobs...
2019-11-17 13:57:11 |  24728 |    removeInstalledPackagesFile | Installed packages functionality is enabled, now we check if the file exists, if it is writeable and delete it...
2019-11-17 13:57:11 |  24728 |    removeInstalledPackagesFile | Check if installed packages file: '/root/glsysbackup.systembackup.installed_packges.txt' exists and if it is writeable...
2019-11-17 13:57:11 |  24728 |    removeInstalledPackagesFile | Installed packages file exists and is writeable.
2019-11-17 13:57:11 |  24728 |    removeInstalledPackagesFile | Deleting installed packages file...
2019-11-17 13:57:11 |  24728 |    removeInstalledPackagesFile | Deleting installed packages file was successful.
2019-11-17 13:57:11 |  24728 | removeOldLeftoverLatestBackups | Checking for old, leftover backups in latest folder...
2019-11-17 13:57:11 |  24728 | removeOldLeftoverLatestBackups | Backup encryption is not enabled, now we check if an lefover encrypted backup file exists, if it is writeable and delete it...
2019-11-17 13:57:11 |  24728 | removeOldLeftoverLatestBackups | Check if encrypted backup file: '/var/backups/wurlitzer/systembackup/latest/systembackup.aes.tar.gz' exists and if it is writeable...
2019-11-17 13:57:11 |  24728 | removeOldLeftoverLatestBackups | No old, leftover encrypted backup found in latest folder.
2019-11-17 13:57:11 |  24728 |                  rotateHandler | Check if backup rotation is enabled...
2019-11-17 13:57:11 |  24728 |                  rotateHandler | Backup rotation is enabled.
2019-11-17 13:57:11 |  24728 |                  rotateHandler | Check if only one backup per day is enabled...
2019-11-17 13:57:11 |  24728 |                  rotateHandler | Only one backup per day is enabled.
2019-11-17 13:57:11 |  24728 |         removeTodaysOldBackups | Check if old backups of type: 'daily' from today exists...
2019-11-17 13:57:11 |  24728 |         removeTodaysOldBackups | Backup from today: '/var/backups/wurlitzer/systembackup/daily/daily_systembackup_2019-11-17_13h53m_Sunday.tar.gz' found...
2019-11-17 13:57:11 |  24728 |         removeTodaysOldBackups | Check if permissions of file: '/var/backups/wurlitzer/systembackup/daily/daily_systembackup_2019-11-17_13h53m_Sunday.tar.gz' are ok to delete it...
2019-11-17 13:57:11 |  24728 |         removeTodaysOldBackups | We have write permissions, so we can delete it.
2019-11-17 13:57:11 |  24728 |         removeTodaysOldBackups | Deleting file was successful.
2019-11-17 13:57:11 |  24728 |                  rotateHandler | Copying actual backup: '/var/backups/wurlitzer/systembackup/latest/systembackup.tar.gz' ==> '/var/backups/wurlitzer/systembackup/daily/daily_systembackup_2019-11-17_13h57m_Sunday.tar.gz'.
2019-11-17 13:57:12 |  24728 |                  rotateHandler | Copy job was successful.
2019-11-17 13:57:12 |  24728 |               removeOldBackups | Check if rotation of daily backups is necessary...
2019-11-17 13:57:12 |  24728 |               removeOldBackups | Rotation of daily backups is not necessary (backup_files_count: '10' <= backup_rotation_daily_max_backups: '10').
2019-11-17 13:57:12 |  24728 |                  rotateHandler | Check if today is the day, where weekly backups should be rotated...
2019-11-17 13:57:12 |  24728 |                  rotateHandler | Today is not the day, where we should rotate the weekly backups (actual_weekday: '7' != backup_rotation_weekly_weekday: '1').
2019-11-17 13:57:12 |  24728 |                  rotateHandler | Check if today is the day, where monthly backups should be rotated...
2019-11-17 13:57:12 |  24728 |                  rotateHandler | Today is not the day, where we should rotate the monthly backups (actual_day_of_month: '17' != backup_rotation_monthly_day_of_month: '1').
2019-11-17 13:57:12 |  24728 |                      checkLock | Check if lock file: '/usr/local/glsysbackup/var/lock/glsysbackup.systembackup.lock' exists and if it is read and writeable...
2019-11-17 13:57:12 |  24728 |                      checkLock | Lock file exists and it is read/writeable.
2019-11-17 13:57:12 |  24728 |                     removeLock | Removing lock...
2019-11-17 13:57:12 |  24728 |                     removeLock | Removing lock was successful.
2019-11-17 13:57:12 |  24728 |                  signalHandler | We did a great job. :)
2019-11-17 13:57:12 |  24728 |                  signalHandler | glsysbackup (2.0.0-beta4) was running: '83' seconds, script_exit_code: '0'.
2019-11-17 13:57:12 |  24728 |                  signalHandler | Bye, bye...
```



## Backupfile structure with enabled backup rotation:
```
13:59:41 [pi@wurlitzer]:~$ find /var/backups/wurlitzer/systembackup/ | sort
/var/backups/wurlitzer/systembackup/
/var/backups/wurlitzer/systembackup/daily
/var/backups/wurlitzer/systembackup/daily/daily_systembackup_2019-11-08_00h09m_Friday.tar.gz
/var/backups/wurlitzer/systembackup/daily/daily_systembackup_2019-11-09_00h09m_Saturday.tar.gz
/var/backups/wurlitzer/systembackup/daily/daily_systembackup_2019-11-10_00h09m_Sunday.tar.gz
/var/backups/wurlitzer/systembackup/daily/daily_systembackup_2019-11-11_00h09m_Monday.tar.gz
/var/backups/wurlitzer/systembackup/daily/daily_systembackup_2019-11-12_00h09m_Tuesday.tar.gz
/var/backups/wurlitzer/systembackup/daily/daily_systembackup_2019-11-13_00h09m_Wednesday.tar.gz
/var/backups/wurlitzer/systembackup/daily/daily_systembackup_2019-11-14_00h09m_Thursday.tar.gz
/var/backups/wurlitzer/systembackup/daily/daily_systembackup_2019-11-15_00h09m_Friday.tar.gz
/var/backups/wurlitzer/systembackup/daily/daily_systembackup_2019-11-16_00h09m_Saturday.tar.gz
/var/backups/wurlitzer/systembackup/daily/daily_systembackup_2019-11-17_13h57m_Sunday.tar.gz
/var/backups/wurlitzer/systembackup/latest
/var/backups/wurlitzer/systembackup/latest/systembackup.tar.gz
/var/backups/wurlitzer/systembackup/monthly
/var/backups/wurlitzer/systembackup/monthly/monthly_systembackup_2019-06-01_00h09m_June.tar.gz
/var/backups/wurlitzer/systembackup/monthly/monthly_systembackup_2019-07-01_00h09m_July.tar.gz
/var/backups/wurlitzer/systembackup/monthly/monthly_systembackup_2019-08-01_00h09m_August.tar.gz
/var/backups/wurlitzer/systembackup/monthly/monthly_systembackup_2019-09-01_00h09m_September.tar.gz
/var/backups/wurlitzer/systembackup/monthly/monthly_systembackup_2019-10-01_00h09m_October.tar.gz
/var/backups/wurlitzer/systembackup/monthly/monthly_systembackup_2019-11-01_00h09m_November.tar.gz
/var/backups/wurlitzer/systembackup/weekly
/var/backups/wurlitzer/systembackup/weekly/weekly_systembackup_2019-09-23_00h09m_38.tar.gz
/var/backups/wurlitzer/systembackup/weekly/weekly_systembackup_2019-09-30_00h09m_39.tar.gz
/var/backups/wurlitzer/systembackup/weekly/weekly_systembackup_2019-10-07_00h09m_40.tar.gz
/var/backups/wurlitzer/systembackup/weekly/weekly_systembackup_2019-10-14_00h09m_41.tar.gz
/var/backups/wurlitzer/systembackup/weekly/weekly_systembackup_2019-10-21_00h09m_42.tar.gz
/var/backups/wurlitzer/systembackup/weekly/weekly_systembackup_2019-10-28_00h09m_43.tar.gz
/var/backups/wurlitzer/systembackup/weekly/weekly_systembackup_2019-11-04_00h09m_44.tar.gz
/var/backups/wurlitzer/systembackup/weekly/weekly_systembackup_2019-11-11_00h09m_45.tar.gz
```
