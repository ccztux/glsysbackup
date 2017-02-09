glsysbackup 1.0.0
============================
Generic Linux System Backup is an advanced backup tool written in bash.
-----------------------------------------------------------------------


It requries the following binaries:
-----------------------------------
- pgrep to check if an instance of glsysbackup is already running
- whois to check the user who executes glsysbackup
- date for logging purposes
- tar to create the backup
- rm to delete files
- mv to move files


Optionally used binaries:
-------------------------
- logger to log to system log
- rpm/dpkg to create a file with installed packages
- openssl to encrypt the backup files


Description:
------------
- Rotating old backup files
- Creating installed packages file (dpkg, rpm)
- Creating tar.gz backup with files/folders you want and installed packages file. Optionally you can define excluded items.
- Encryption of backup file with openssl if you want it


Features:
---------
- Lock function. Only one instance is possible to run.
- Verbose logging to stdout and/or system logfile and/or individual logfile.
- Excluding of files
- Backupfile rotating
- Creates a file with installed packages (rpm/dpkg)
- Encryption with openssl


Future plans:
-------------
- use config files for multiple jobs
- add cli options and arguments
- add sendEmail functionality
