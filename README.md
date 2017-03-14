| Master | [![Build Status](https://travis-ci.org/ccztux/glsysbackup.svg?branch=master)](https://travis-ci.org/ccztux/glsysbackup) | Devel  | [![Build Status](https://travis-ci.org/ccztux/glsysbackup.svg?branch=devel)](https://travis-ci.org/ccztux/glsysbackup) |



# glsysbackup 1.0.1.a (alpha)
## Generic Linux System Backup is an advanced backup tool written in bash.



### CLI options:
```
Usage: glsysbackup OPTIONS

Author:                 ccztux
Last modification:      2017-03-14
Version:                1.0.0.a
License:                GNU GPLv3

Description:            glsysbackup (Generic Linux System Backup) is an advanced backup tool written in bash.

OPTIONS:
   -h        Shows this help.
   -o	     Override lock in case glsysbackup was terminated abnormally.
   -v        Shows script version.
```



### Configuration variables:
```bash
#-------------------------
# Configuration variables:
#-------------------------

root_privileges_required="1"

backup_destination_path="/var/backups/"
backup_filename="${script_name}.${script_hostname}.tar.gz"
backup_full_path="${backup_destination_path}${backup_filename}"
items_to_backup="/home/ /root/ /var/lib/mpd/ /usr/local/bin/ /boot/config.txt"
items_to_exclude=("/root/old" "/tmp/var")

backup_rotating_required="1"
backup_rotating_files_to_keep="5"

installed_packages_required="1"
installed_packages_filename="/root/installed_packages_filename.txt"

backup_encryption_required="1"
backup_encryption_password="test1234"
backup_encryption_filename="${backup_full_path//tar.gz/aes.tar.gz}"

log_to_syslog="1"
log_to_file="0"
log_file="/var/log/${script_name}.log"
log_to_stdout="1"
```



### It requries the following binaries:
- **bash** (Version 3 || 4)
- **which** to get the full path to the required binaries through environment variable $PATH
- **pgrep** to check if an instance of glsysbackup is already running
- **whoami** to check the user who executes glsysbackup
- **date** for logging purposes
- **tar** to create the backup
- **rm** to delete files
- **mv** to move files



### Optionally used binaries:
- **logger** to log to the system log
- **rpm** || **dpkg** to create a file with installed packages
- **openssl** to encrypt the backup file



### Description:
1. Check if glsysbackup will be exectuted with root privileges
2. Get and log the user, who starts glsysbackup
3. Check required, common binaries
4. Check if an instance is already running via lockfile and pgrep
5. Check if bash version meets requirements
6. Rotation of old backup files
7. Get syspackage manager and if it is supported, create file with installed packages
8. Build excluding options from config array
9. Make the backup



### Features:
- Lock functionality. Only one instance is possible to run. (Lock file and check with pgrep)
- Verbose logging to stdout and/or system logfile and/or individual logfile.
- Excluding of files
- Backupfile rotating
- Creates a file with installed packages (rpm || dpkg)
- Encryption with openssl
- CLI options and arguments



### Future plans:
- use config files for multiple jobs
- add sendEmail functionality
- add incremental backup feature
