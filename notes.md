# Linux Training
[![N|Solid](https://vieo.tv/assets/uploads/nairobi-kenya-linux-event.jpg)](http://livingopensource.net)

## LVM Management

### Creating Partitions using *fdisk*
Persistent storage devices can be partitioned as either __primary__ or __extended__. Linux allows upto 4 partitions. You need to specify the __sector__ at which the partition starts and the size in __KiB__, __MiB__, or __GiB__.  

##### *fdisk* commands: 
```sh
    p # print all partitions
    n # create new partition
    t # change partition type
```

After creating the partition we need to specify its file system. Common ones are __xfs__ and __ext4__. __xfs__ is preferred as it's more modern. We then _mount_ the partition in order to manipulate it as a directory. It can be _unmount_ __ed__ afterwards. To make the process automatic on boot we store it in a __config file__ or use _partprobe_ to update the kernel.


### Creating logical volumes
```sh
# create partition, type 8e
fsdisk /dev/<device name>
```

```sh
# create physical volume using partition
pvcreate /dev/<partition name>
```

```sh
# create volume group with physical volumes
vgcreate <physical volume>
```

```sh
#create a logical volume from the volume group
lvcreate <volume group name> 
```

```sh
# set file system to xfs
mkfs.xfs /mnt/<vloume group>/<logical volume>
```

```sh
# save setting in config file for auto-setup on boot
vi /etc/fstab 
```

```sh
# notify kernel of changes
mount -a
```

### Extending Logical Volumes

```sh
# create partition, type 8e
fdisk /dev/<device name>
```

```sh
# update partition table
partprobe
```

```sh
# create physical volume
pvcreate /dev/<partition name>
```

```sh
# print list of physical volumes
pvs
```

```sh
# extend volume group
vgextend <volume group name>
```

```sh
# extend logical volume
# use -r to auto-update file system
lvextend -r /mnt/<logical volume>
```

## Linux RAM usage
Swap memory holds idle application memory to save on RAM. RAM holds buffers(write) and cache(read) that make reading from and writing to hard disk faster.

```sh
# check current memory usage
vmstat
```

```sh
#create partition, type 82
fsdisk /dev/<device name>
```

```sh
# mark partition as swap location
mkswap /dev/<device name>
```

```sh
#update /etc/fstab
echo "myswap swap swap defaults 0 0" >> /etc/fstab
```

```sh
#update kernel on new swap location
swapon -a
```

## Symbolic Links
 - inode
- hard link vs symbolic link

###### MBR (Master Boot Record)

virtualization

kvm, libvertd, virsh

virsh:
 - list --all
 - net-list
 - net-info
 - console

software installation:
 - formats: tar ball (zip file), packages, repositories
 - dependencies: libraries
 - packages: rpm(yum on redhat), dev(apt on ubuntu)
 - yum repo list
 - createrepo <directory with packages.
 - yum provides <command_name>

firewall:
 - filter incoming traffic
 - ports: 22, 80, 443
 - /etc/services
 - iptables: ufw, firewalld, susefirewall
 - nftables

iptables:
 - set policy to define default behaviors
 - chains: rules
 - port, protocols, ip addresses
 - input, output, forward chains
 - target: ACCEPT, DROP, REJECT
- OSI : hardware
 - ...:
   i/o -> interface
   s/d -> ip/dnsname
   p -> udp/tcp

- all traffic for the local loopback is allowed (lo)
# iptables -A INPUT -i lo -j ACCEPT
# iptables -A OUTPUT -O lo -j ACCEPT

- list iptables
#iptables -L

#ping localhost

-allow ssh traffic
# iptables -A INPUT -p tcp --dport 22 -j ACCEPT
# iptables -A OUTPUT -m state --state established,related -j ACCEPT


- allow ping traffic
# iptables -A INPUT -p icmp -j ACCEPT

- save to disk, make persistent
# iptables-save > /etc/sysconfig/iptables
# systemctl enable --now iptables

- web server on linux
- apache httpd vs nginx

install webserver
------------------
1. yum install httpd
2. systemctl enable --now httpd
3. /etc/httpd/conf/httpd.conf
4. /var/www/html/index.html
InstalInstalInstalInstal
Install Virtual Hosts
---------------------
1. /etc/hosts
   127.0.0.1 los,net
2. /etc/httpd/conf.d/los.net.conf
 <VirtualHost 127.0.0.1:80>
      ServerName los.net
      DocumentRoot /www/los
      CustomLog logs/los.net-access_log common
      ErrorLog logs/los.net-error_log
 </VirtualHost>
3. /etc/httpd/conf/httpd.conf
<Directory '/www/los/'>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
4. /www/los/index.html
5. setenforce 0
6. systemctl restart httpd
