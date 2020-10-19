# Linux Training
[![N|Solid](https://vieo.tv/assets/uploads/nairobi-kenya-linux-event.jpg)](http://livingopensource.net)

## User Management

### Intro

```sh
# print current user and group info
id
```

### Adding, deleting users

```sh
# add user bob
useradd bob

# delete user bob, from root
userdel bob
```

### Switching users

```sh
# switch to root, note there is a space afer su
su 

# switch to user bob
su - bob
```

### Managing user groups

```sh
# add a group
groupadd sales

# add bob to the sales group (without overwriting other groups)
usermod -aG sudo bob

# delet group sales
groupdel sales
```

## File system

`/` - root

- `usr` - programs
  - `lib` - library
  - `bin` - binary files
  - `sbin` - root bin
  - `local`
- `var`
  - `log`
- `tmp`
- `dev` - device interfaces
- `proc` - exists in memory only, kernel interface
- `etc` - configuration files

## LVM Management

### Creating Partitions using *fdisk*
Persistent storage devices can be partitioned as either __primary__ or __extended__. Linux allows upto 4 partitions. You need to specify the __sector__ at which the partition starts and the size in __KiB__, __MiB__, or __GiB__.  

####  ```fdisk``` commands: 
```sh
    p # print all partitions
    n # create new partition
    t # change partition type
```

After creating the partition we need to specify its file system. Common ones are __xfs__ and __ext4__. __xfs__ is preferred as it's more modern. We then ```mount``` the partition in order to manipulate it as a directory. It can be unmounted using ```umount```  later. To make the process automatic on boot we store it in a __config file__ or use ```partprobe``` to update the kernel.


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
Swap memory holds idle application memory to save on RAM. RAM holds ___buffers___(write) and ___cache___(read) that make reading from and writing to hard disk faster.

```sh
# check current memory usage
vmstat
```

```sh
# create partition, type 82
fsdisk /dev/<device name>
```

```sh
# mark partition as swap location
mkswap /dev/<device name>
```

```sh
# update /etc/fstab
echo "myswap swap swap defaults 0 0" >> /etc/fstab
```

```sh
# update kernel on new swap location
swapon -a
```




 ## Web Servers on Linux
```sh
# Install and enable the web server service
yum install httpd
systemctl enable --now httpd
```


```sh
# Register domain name
echo 127.0.0.1 los.net >> /etc/hosts 
```

```sh
# Create config file with Virtual Host metadata
vi /etc/httpd/conf.d/los.net.conf
```

```xml
 <VirtualHost 127.0.0.1:80>
      ServerName los.net
      DocumentRoot /www/los
      CustomLog logs/los.net-access_log common
      ErrorLog logs/los.net-error_log
 </VirtualHost>
```

```sh
# add directory permissions to httpd config file
vi /etc/httpd/conf/httpd.conf
```

```xml
<Directory ''>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

```sh
# add content to public directory
cat "Hello World" > /www/los/index.html
```

```sh
# restart server service
systemctl restart httpd
```


## Managing Network Communication
### Firewall
Firewalls filter incoming traffic. Each service in Linux is by default allocated a port as specified in ```/etc/services/``` --- for example port ```22``` for ___ssh___, ```80``` for ___http___ and ```443``` for ___https___. 

 Tools used for firewall management: ```ufw```, ```firewalld```, ```susefirewall```

#### iptables
Linux uses _iptables_ to set policies that define default behaviors. Modern Linux _OS_'s use _nftables_. 

The rules are categorized into 3 __chains__: _Input_, _Output_ and _Forwarding_ 

We use __ports__ and __protocols__ to determine how traffic should be handled --- it can either ```ACCEPT```, ```DROP``` or ```REJECT``` traffic.

 #### ```iptables``` options:
 ```sh
   i/o # interface
   s/d # ip/dnsname
   p # protocol: udp/tcp
 ```

```sh
# list the iptables
iptables -L
```

#### Handling Common Traffic Types
```sh
# Allow all traffic for the local loopback(lo)
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -O lo -j ACCEPT

ping localhost
```
```sh
# Allow ssh
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A OUTPUT -m state --state established,related -j ACCEPT
```

```sh
# Allow ping
iptables -A INPUT -p icmp -j ACCEPT
```

```sh
# write iptables configuration to config file
iptables-save > /etc/sysconfig/iptables
systemctl enable --now iptables
```

## Software Installation
Software is bundled for distribution as __packages__ that perform specific tasks. __Repositories__ are collection of _packages_ that can be used to satisfy a package's _dependencies_.

 The format of the packages depends on the particular Linux Distro --- ```.deb```  on Debian-based distros  and ```.rpm``` on Red Hat-based distros. 

 Package managers  are used to conveniently install packages by automatically fetching and installing dependencies alongside the required package from the available repositories. Common package managers are: ```apt``` on Ubuntu-based distros and ```yum``` on Red Hat-based distros.

```sh
# print all repos available
yum repo list 
```
```sh
# create a repo from a directory
# adds metadata about the packages in the directory
createrepo <directory with packages>
```
```sh
# do a deep search for a tool in the repos
yum provides <command name>
```

## Virtualization
We can create virtual machines on Linux using  ```kvm```
or ```libvertd``` and manipulate them using ``` virsh```.

### ```virsh``` commands:
```sh
 list --all # print all virtual machines including inactive ones
 net-list
 net-info
 console # connect to remote virtual machine
```


## Symbolic Links
###### inode, hard link vs symbolic link
###### MBR (Master Boot Record)

