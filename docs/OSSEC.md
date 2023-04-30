# OSSEC Setup On Ubuntu 
Reference: https://www.rapid7.com/blog/post/2017/06/30/how-to-install-and-configure-ossec-on-ubuntu-linux/  
Latest versions: https://github.com/ossec/ossec-hids/branches/all  

## Error  
```
root@ip-172-31-37-103:/var/www/html# apt-get install build-essential gcc make apache2 libapache2-mod-php7.0 php7.0 php7.0-cli php7.0-common apache2-utils unzip wget sendmail inotify-tools -y
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Note, selecting 'php7.0-thrift' for regex 'php7.0'
Note, selecting 'php7.0-common' for regex 'php7.0'
Note, selecting 'php7.0-curl' for regex 'php7.0'
Package php7.0-common is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or
is only available from another source

E: Unable to locate package libapache2-mod-php7.0
E: Couldn't find any package by glob 'libapache2-mod-php7.0'
E: Couldn't find any package by regex 'libapache2-mod-php7.0'
E: Unable to locate package php7.0-cli
E: Couldn't find any package by glob 'php7.0-cli'
E: Couldn't find any package by regex 'php7.0-cli'
E: Package 'php7.0-common' has no installation candidate
```
### Answer  
https://stackoverflow.com/questions/38815245/unable-to-locate-package-libapache2-mod-php7



### To Fix: Build fails because of missing pcre2-10.32  
I was able to update from ossec 2.9.x to 3.3.0 on ubuntu 18.04 -- the two new packages I had to install were libpcre2-dev and zlib1g-dev. Specifically, this is what worked for me:
```
sudo apt install libpcre2-dev zlib1g-dev  
sudo PCRE2_SYSTEM=yes ./install.sh  
```
https://github.com/ossec/ossec-hids/issues/1663  

### To fix: fatal error: openssl/opensslv.h: No such file or directory  
To fix this problem, you have to install OpenSSL development package, which is available in standard repositories of all modern Linux distributions.  

To install OpenSSL development package on Debian, Ubuntu or their derivatives:  
```
$ sudo apt-get install libssl-dev  
```

To install OpenSSL development package on Fedora, CentOS or RHEL:  
```
$ sudo yum install openssl-devel  
```
Edit : As @isapir has pointed out, for Fedora version>=22 use the DNF package manager :
```
dnf install openssl-devel 
```

### error: make: *** [Makefile:930: ossec-maild] Error 1
reference: https://github.com/ossec/ossec-hids/issues/2039  
```
apt install -y libsystemd-dev  
```

Follow the above steps to proceed with the OSSEC
