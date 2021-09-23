# Set **SSH**
## Generate local SSH keys
```
ssh-keygen -t rsa
```

## Copy public key to server
```
ssh-copy-id -i ~/.ssh/id_rsa.pub [Server IP]
```
Enter the server's root password to finish.

# Set **Denyhosts**
## Description
A script to help thwart SSH server attacks.

## Installation
Clear SSH authorization log, in case Denyhosts blocks the owner's IP, and then install DenyHosts.
``` 
echo "" > /var/log/auth.log
apt-get install denyhosts
```

## Configuration
```
vim /etc/denyhosts.conf
```
Set as below.
```
	PURGE_DENY = 100d
	DENY_THRESHOLD_INVALID = 2
	HOSTNAME_LOOKUP=YES
```	

# Enlarge Swap space
Check Swap space.
```
free -h
```
Delete current Swap.
```
swapoff -a
```
Create a larger Swap space. (1G)
```
dd if=/dev/zero of=/root/swapfile bs=1M count=1024
chmod 0600 /root/swapfile
mkswap /root/swapfile
swapon /root/swapfile
```
Edit /etc/fstab

# Change **apt Source**
## Backup source list
```
cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

## Choose the best source list
```
vim /etc/apt/sources.list
```
Source list as below:
```
# 台湾ubuntu官网
deb http://tw.archive.ubuntu.com/ubuntu bionic main restricted universe multiverse
deb http://tw.archive.ubuntu.com/ubuntu bionic-security main restricted universe multiverse
deb http://tw.archive.ubuntu.com/ubuntu bionic-updates main restricted universe multiverse
deb http://tw.archive.ubuntu.com/ubuntu bionic-proposed main restricted universe multiverse
# deb-src http://tw.archive.ubuntu.com/ubuntu bionic main restricted universe multiverse
# deb-src http://tw.archive.ubuntu.com/ubuntu bionic-security main restricted universe multiverse
# deb-src http://tw.archive.ubuntu.com/ubuntu bionic-updates main restricted universe multiverse
# deb-src http://tw.archive.ubuntu.com/ubuntu bionic-proposed main restricted universe multiverse

# 北理工
deb http://mirror.bit.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb http://mirror.bit.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirror.bit.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirror.bit.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src http://mirror.bit.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src http://mirror.bit.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src http://mirror.bit.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src http://mirror.bit.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

# 网易
deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
# deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
# deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse

# 阿里
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

# 清华
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

# 上海交大
deb http://ftp.sjtu.edu.cn/ubuntu/ bionic main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ bionic-proposed main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ bionic-security main multiverse restricted universe
deb http://ftp.sjtu.edu.cn/ubuntu/ bionic-updates main multiverse restricted universe
# deb-src http://ftp.sjtu.edu.cn/ubuntu/ bionic main multiverse restricted universe
# deb-src http://ftp.sjtu.edu.cn/ubuntu/ bionic-proposed main multiverse restricted universe
# deb-src http://ftp.sjtu.edu.cn/ubuntu/ bionic-security main multiverse restricted universe
# deb-src http://ftp.sjtu.edu.cn/ubuntu/ bionic-updates main multiverse restricted universe

# 香港中文大学
deb http://ftp.cuhk.edu.hk/pub/Linux/ubuntu bionic main restricted universe multiverse
deb http://ftp.cuhk.edu.hk/pub/Linux/ubuntu bionic-security main restricted universe multiverse
deb http://ftp.cuhk.edu.hk/pub/Linux/ubuntu bionic-updates main restricted universe multiverse
deb http://ftp.cuhk.edu.hk/pub/Linux/ubuntu bionic-proposed main restricted universe multiverse
# deb-src http://ftp.cuhk.edu.hk/pub/Linux/ubuntu bionic main restricted universe multiverse
# deb-src http://ftp.cuhk.edu.hk/pub/Linux/ubuntu bionic-security main restricted universe multiverse
# deb-src http://ftp.cuhk.edu.hk/pub/Linux/ubuntu bionic-updates main restricted universe multiverse
# deb-src http://ftp.cuhk.edu.hk/pub/Linux/ubuntu bionic-proposed main restricted universe

# Ubuntu官方
deb http://archive.ubuntu.com/ubuntu/ bionic main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src http://archive.ubuntu.com/ubuntu/ bionic main restricted universe multiverse
# deb-src http://archive.ubuntu.com/ubuntu/ bionic-security main restricted universe multiverse
# deb-src http://archive.ubuntu.com/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src http://archive.ubuntu.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://archive.canonical.com/ubuntu/ bionic partner
```