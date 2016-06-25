Linux Command Reference
=======================

###Running as root:
```bash
$ sudo
```

###Reboot:
```bash
$ reboot
```

###Edit a file:
```bash
$ nano <filename>
```

###Check Network:
```bash
$ ip a
```
should show an ip address for eth0

###Update Packages
- Update the package list
```bash
$ sudo apt update
```
- Upgrade all to latest packages
```bash
$ sudo apt upgrade
```

###Change hostname 
```bash
edit /etc/hostname
```
then run
```bash
/etc/init.d/hostname.sh script to make it live.
```
