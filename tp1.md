# TP 1
Idryss Judéaux  Groupe A 2ème année


# 0. Prérequis
2 machine:
[idryss@localhost ~] 
[idryssj@localhost ~]

[idryss@localhost ~]$ su -
[idryssj@localhost ~]$ su -

Pour faciliter les prochaines commandes.

## Partitionnement


[idryssj@localhost ~]$ lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0    8G  0 disk
├─sda1            8:1    0    1G  0 part /boot
└─sda2            8:2    0    7G  0 part
  ├─centos-root 253:0    0  6.2G  0 lvm  /
  └─centos-swap 253:1    0  820M  0 lvm  [SWAP]
sdb               8:16   0    5G  0 disk
sr0              11:0    1 1024M  0 rom

##

[idryss@localhost ~]$ lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0    8G  0 disk
├─sda1            8:1    0    1G  0 part /boot
└─sda2            8:2    0    7G  0 part
  ├─centos-root 253:0    0  6.2G  0 lvm  /
  └─centos-swap 253:1    0  820M  0 lvm  [SWAP]
sdb               8:16   0    5G  0 disk
sr0              11:0    1 1024M  0 rom

##
[root@localhost ~]# vgcreate data /dev/sdb
  Physical volume "/dev/sdb" successfully created.
  Volume group "data" successfully created
[root@localhost ~]#

##
[root@localhost ~]# lvcreate -L 2G data -n SRV
  Logical volume "SRV" created.
  ##
  [root@localhost ~]# lvcreate -L 3G data -n HOME
  Volume group "data" has insufficient free space (767 extents): 768 required.
[root@localhost ~]# lvcreate -L 2.5G data -n HOME
  Logical volume "HOME" created.
##

[root@localhost ~]# mkfs -t ext4 /dev/data/SRV
[root@localhost ~]# mkfs -t ext4 /dev/data/HOME

[root@localhost ~]# mkdir /srv/data1
[root@localhost ~]# mkdir /srv/data2

##

Partition des deux machines.

[idryss@localhost ~]$ df -h

Filesystem               Size  Used Avail Use% Mounted on
devtmpfs                 484M     0  484M   0% /dev
tmpfs                    496M     0  496M   0% /dev/shm
tmpfs                    496M  6.8M  489M   2% /run
tmpfs                    496M     0  496M   0% /sys/fs/cgroup
/dev/mapper/centos-root  6.2G  1.2G  5.1G  19% /
/dev/sda1               1014M  136M  879M  14% /boot
tmpfs                    100M     0  100M   0% /run/user/1000
/dev/mapper/data-SRV     2.0G  6.0M  1.8G   1% /srv/data1
/dev/mapper/data-HOME    2.4G  7.5M  2.3G   1% /srv/data2
##

[idryssj@localhost ~]$ df -h

Filesystem               Size  Used Avail Use% Mounted on
devtmpfs                 484M     0  484M   0% /dev
tmpfs                    496M     0  496M   0% /dev/shm
tmpfs                    496M  6.8M  489M   2% /run
tmpfs                    496M     0  496M   0% /sys/fs/cgroup
/dev/mapper/centos-root  6.2G  1.2G  5.1G  19% /
/dev/sda1               1014M  136M  879M  14% /boot
tmpfs                    100M     0  100M   0% /run/user/1000
/dev/mapper/data-SRV     2.0G  6.0M  1.8G   1% /srv/data1
/dev/mapper/data-HOME    2.4G  7.5M  2.3G   1% /srv/data2

##
[idryss@localhost ~]$ cat /etc/fstab
[...]

/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=ccfc3870-11d9-4af9-a9d9-976bcc16ea75 /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0

/dev/data/SRV /srv/data1 ext4 defaults 0 0
/dev/data/HOME /srv/data2 ext4 defaults 0 0
##
[idryssj@localhost ~]$ cat /etc/fstab
[...]
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=ccfc3870-11d9-4af9-a9d9-976bcc16ea75 /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0

/dev/data/SRV /srv/data1 ext4 defaults 0 0
/dev/data/HOME /srv/data2 ext4 defaults 0 0
##


[root@localhost ~]# lvs
  LV   VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root centos -wi-ao----  <6.20g                                                
  swap centos -wi-ao---- 820.00m                                                
  HOME data   -wi-ao----   2.50g                                                
  SRV  data   -wi-ao----   2.00g  


[root@localhost ~]# mount -av
/                        : ignored
/boot                    : already mounted
swap                     : ignored
/srv/data1               : already mounted
/srv/data2               : already mounted

## Ping

[idryssj@localhost ~]$ ping google.com
PING google.com (216.58.213.142) 56(84) bytes of data.
64 bytes from par21s03-in-f14.1e100.net (216.58.213.142): icmp_seq=1 ttl=113 time=17.0 ms
64 bytes from par21s03-in-f14.1e100.net (216.58.213.142): icmp_seq=2 ttl=113 time=17.0 ms
^C
--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 17.006/17.021/17.037/0.131 ms
##
[idryss@localhost ~]$ ping google.com
PING google.com (216.58.213.142) 56(84) bytes of data.
64 bytes from par21s03-in-f142.1e100.net (216.58.213.142): icmp_seq=1 ttl=113 time=15.8 ms
64 bytes from par21s03-in-f142.1e100.net (216.58.213.142): icmp_seq=2 ttl=113 time=18.8 ms
^C
--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
rtt min/avg/max/mdev = 15.811/17.306/18.801/1.495 ms
##

[idryss@localhost ~]$ ip a
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:5d:24:66 brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.105/24 brd 192.168.56.255 scope global noprefixroute dynamic enp0s8

[idryssj@localhost ~]$ ip a
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:35:18:f3 brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.106/24 brd 192.168.56.255 scope global noprefixroute dynamic enp0s8

[idryssj@localhost ~]$ ip r s
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
192.168.56.0/24 dev enp0s8 proto kernel scope link src 192.168.56.106 metric 101

[idryss@localhost ~]$ ip r s
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
192.168.56.0/24 dev enp0s8 proto kernel scope link src 192.168.56.105 metric 101
##
[idryssj@localhost ~]$ hostname
localhost.localdomain

[idryss@localhost ~]$ hostname
localhost.localdomain


[root@localhost ~]# hostname node2.tp1.b2
[root@localhost ~]# exit
logout
[idryssj@localhost ~]$ hostname
node2.tp1.b2

[root@localhost ~]# hostname node1.tp1.b2
[root@localhost ~]# exit
logout
[idryss@localhost ~]$ hostname
node1.tp1.b2

##
[root@node2 etc]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.1.12 node2.tp1.b2
192.168.1.11 node1.tp1.b2


[root@node1 ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.1.11 node1.tp1.b2
192.168.1.12 node2.tp1.b2
##
[idryss@localhost ~]# ping -c 2 node2.tp1.b2
PING node2.tp1.b2 (192.168.1.12) 56(84) bytes of data.
64 bytes from node2.tp1.b2 (192.168.1.12): icmp_seq=1 ttl=64 time=0.960 ms
64 bytes from node2.tp1.b2 (192.168.1.12): icmp_seq=2 ttl=64 time=1.00 ms

--- node2.tp1.b2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 0.960/0.982/1.004/0.022 ms

[idryssj@localhost ~]# ping -c 2 node1.tp1.b2
PING node1.tp1.b2 (192.168.1.11) 56(84) bytes of data.
64 bytes from node1.tp1.b2 (192.168.1.11): icmp_seq=1 ttl=64 time=0.960 ms
64 bytes from node1.tp1.b2 (192.168.1.11): icmp_seq=2 ttl=64 time=1.00 ms

--- node1.tp1.b2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 0.960/0.982/1.004/0.022 ms

## Utilisateur
[root@node1 ~]# useradd idryss
useradd: user 'idryss' already exists
[root@node2 ~]# useradd idryssj
useradd: user 'idryssj' already exists
visudo : 
idryss ALL = (ALL) ALL  
idryssj ALL = (ALL) ALL

## Firewall

[root@node1 ~]# firewall-cmd --add-port=22/tcp --permanent

[root@node1 ~]# firewall-cmd --reload

[root@node1 ~]# sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: dhcpv6-client ssh
  ports: 22/tcp
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
  
## Selinux


[root@node1 ~]# sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31


[root@node2 ~]# sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31



vi / etc / sysconfig / selinux
SELINUX = desabled

[idryss@localhost ~]$ sestatus
SELinux status:                 disabled




## I. Configuration du serveur Web


[root@localhost ~]# yum install epel-release
[root@localhost ~]# yum install nginx



# ajout user
[idryss@node1~]$ sudo useradd web

[idryss@node1~]$ sudo passwd web
# permission
[web@node1 ~]$ sudo chown web /srv/site1

[web@node1 ~]$ sudo chown web /srv/site2

[web@node1 ~]$ sudo chmod 400 / srv / site1

[web@node1 ~]$ sudo chmod 400 / srv / site2
# clé et certificat associé

[web@node1 ~]$ openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout server.key -out server.crt
# firewall
[web@node1 ~]$ sudo firewall-cmd --add-port = 80 / tcp --permanent

[web@node1 ~]$ sudo firewall-cmd --add-port = 443 / tcp --permanent

[web@node1 ~]$  sudo firewall-cmd --reload

[root@node1 ~]# sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: dhcpv6-client ssh
  ports: 80 / tcp 443 / tcp
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

# connexion
test sur vm 2

 [root@node2 ~] $ curl -Lk node1.tp1.b2 / site1
 < h 1> bonjour la france code de la vm 1 < / h 1> 
[root@node2 ~ ] $ curl -Lk node1.tp1.b2 / site2
 < h 1> bonjour bordeaux code de la vm 2 < / h 1>
 
## Script de sauvegarde

```
#!/bin/sh

# utilisation : ./tp1_backup.sh /srv/site1

sauvegarde()
{
    # "site${1: -1}_$(date "+%Y%m%d_%H%M")"
    tar -czvf site${1: -1}_$(date "+%Y%m%d_%H%M").tar.gz $1
}

suppression()
{
    if test $(ls | grep site${1: -1} | wc -w) -gt 7
    then
         ls | grep site${1: -1} | sort > tmp
         rm -rf $(head -1 tmp)
         rm -f tmp
    fi
}

sauvegarde $1
suppression $1
```

Pour les copier collé de mes deux vm je l'ai est connecté ave putty.exe