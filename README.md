# glsysbackup
Generic Linux System Backup is an advanced backup tool written in bash. It uses the following programs:
- tar to create the backup
- openssl to encrypt the backup files if you want
- logger to log to system log
- rpm/dpkg to create a file with installed packages


Description:
- Rotating of old backup files
- Creating installed packages file (dpkg, rpm)
- Creating tar.gz backup with files/folders you want and installed packages file. Optionally you can define excluded items.
- Encryption of backup file with openssl if you want it


Features:
- Lock function. Only one instance is possible to run.
- Verbose logging to stdout and/or system logfile and/or individual logfile.
- Excluding of files
- Backupfile encrypting with openssl


Future:
- use multiple config files
- add cli options and arguments
- add sendEmail functionality
