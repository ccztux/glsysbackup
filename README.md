# glsysbackup
Generic Linux System Backup is an advanced backup tool written in bash.


Description:
- Rotating old backup files
- Creating installed packages file (dpkg, rpm)
- Creating tar.gz backup with files/folders you want and installed packages file. Optionally you can define excluded items.


Features:
- Lock function. Only one instance can run.
- Verbose logging to stdout and/or system logfile and/or individual logfile.
- Backupfile encrypting with openssl


Future:
- use multiple config files
