| Branch: | Shellcheck state: |
| ------------- | ------------- |
| Master | [![Build Status](https://travis-ci.org/ccztux/glsysbackup.svg?branch=master)](https://travis-ci.org/ccztux/glsysbackup) |
| Devel  | [![Build Status](https://travis-ci.org/ccztux/glsysbackup.svg?branch=devel)](https://travis-ci.org/ccztux/glsysbackup) |



# glsysbackup 1.0.1-alpha3
## Generic Linux System Backup is an advanced backup tool written in bash.



### Example usage:
```
[2017-03-23 18:48:06] glsysbackup: [4347] glsysbackup 1.0.1-alpha3 starting... (PID=4347)
[2017-03-23 18:48:06] glsysbackup: [4347] Getting options...

Usage: glsysbackup OPTIONS

Author:			ccztux
Last modification:	2017-03-23
Version:		1.0.1-alpha3
License:		GNU GPLv3

Description:		glsysbackup (Generic Linux System Backup) is an advanced backup tool written in bash.

OPTIONS:
   -h		Shows this help.
   -o		Override lock in case glsysbackup was terminated abnormally.
   -v		Shows script version.

[2017-03-23 18:48:06] glsysbackup: [4347] Caught: 'EXIT', exiting script...
[2017-03-23 18:48:06] glsysbackup: [4347] Now we are doing some cleanup jobs...
[2017-03-23 18:48:06] glsysbackup: [4347] Backup encryption is enabled, now we check if unencrypted backup file exists, if it is writeable and delete it...
[2017-03-23 18:48:06] glsysbackup: [4347] Check if unencrypted backup file: '/var/backups/glsysbackup.tux01.tar.gz' exists and if it is writeable...
[2017-03-23 18:48:06] glsysbackup: [4347] Unencrypted backup file doesnt exist, nothing to do.
[2017-03-23 18:48:06] glsysbackup: [4347] Installed packages functionality is enabled, now we check if the file exists, if it is writeable and delete it...
[2017-03-23 18:48:06] glsysbackup: [4347] Check if installed packages file: '/root/installed_packages_filename.txt' exists and if it is writeable...
[2017-03-23 18:48:06] glsysbackup: [4347] Installed packages file doesnt exist, nothing to do.
[2017-03-23 18:48:06] glsysbackup: [4347] Ooops!!! Something went wrong. :(
[2017-03-23 18:48:06] glsysbackup: [4347] Read the log or output to determine what exactly went wrong.
[2017-03-23 18:48:06] glsysbackup: [4347] Check if lock file: '/var/lock/glsysbackup' exists and if it is read and writeable...
[2017-03-23 18:48:06] glsysbackup: [4347] Lock file doesnt exist.
[2017-03-23 18:48:06] glsysbackup: [4347] Script was running: '0' seconds.
[2017-03-23 18:48:06] glsysbackup: [4347] Bye, bye...
```



### Configuration variables:
```bash
#-------------------------
# Configuration variables:
#-------------------------

log_to_syslog="1"
log_to_file="0"
log_file="/var/log/${script_name}.log"
log_to_stdout="1"

root_privileges_required="1"

backup_destination_path="/var/backups/"
backup_filename="${script_name}.${script_hostname}.tar.gz"
backup_full_path="${backup_destination_path}${backup_filename}"
items_to_backup="/home/ /root/ /var/lib/mpd/ /usr/local/bin/ /boot/config.txt"
items_to_exclude=("/root/old" "/tmp/var/")

backup_rotating_required="1"
backup_rotating_files_to_keep="5"

installed_packages_required="1"
installed_packages_filename="/root/installed_packages_filename.txt"

backup_encryption_required="1"
backup_encryption_password="test1234"
backup_encryption_filename="${backup_full_path//tar.gz/aes.tar.gz}"

pre_backup_script="/home/pi/pre.sh"
post_backup_script="/home/pi/post.sh"
```



### It requires the following binaries:
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



### Optionally used binaries:
- **logger** to log to the system log
- **rpm** || **dpkg** to create a file with installed packages
- **openssl** to encrypt the backup file
- **renice** to renice glsysbackup and all child processes
- **ionice** to re-ionice glsysbackup and all child processes



### Description:
1. Check if glsysbackup will be exectuted with root privileges
2. Get and log the user, who starts glsysbackup
3. Check if required, common binaries exists
4. Check if an instance is already running via lockfile and pgrep
5. Check if bash version meets requirements
6. Rotation of old backup files
7. Get syspackage manager and if it is supported, create file with installed packages
8. Build excluding options from config array
9. Execute Prebackup script, if it is defined
10. Make the backup
11. Execute Prebackup script, if it is defined



### Features:
- Lock functionality. Only one instance is possible to run. (Lock file and check with pgrep)
- Verbose logging to stdout and/or system logfile and/or individual logfile.
- Excluding of files
- Backupfile rotation
- Creates a file with installed packages (rpm || dpkg)
- Encryption with openssl
- CLI options and arguments
- Renicing
- Re-ioniceing



### Future plans:
- use config files for multiple jobs
- add sendEmail functionality
- add incremental backup feature
