| Branch: | Shellcheck state: |
| ------------- | ------------- |
| Master | [![Build Status](https://travis-ci.org/ccztux/glsysbackup.svg?branch=master)](https://travis-ci.org/ccztux/glsysbackup) |
| Devel  | [![Build Status](https://travis-ci.org/ccztux/glsysbackup.svg?branch=devel)](https://travis-ci.org/ccztux/glsysbackup) |



# glsysbackup 1.0.1.a (alpha)
## Generic Linux System Backup is an advanced backup tool written in bash.



### It requries the following binaries:
- *bash* (Version 3 || 4)
- *which* to get the full path through environment variable $PATH to the required binaries 
- *pgrep* to check if an instance of glsysbackup is already running
- *whoami* to check the user who executes glsysbackup
- *date* for logging purposes
- *tar* to create the backup
- *rm* to delete files
- *mv* to move files



### Optionally used binaries:
- *logger* to log to the system log
- *rpm* || *dpkg* to create a file with installed packages
- *openssl* to encrypt the backup file



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
