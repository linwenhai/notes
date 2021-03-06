

#### 1防火墙

```bash
systemctl status firewalld.service		# 查看防火墙状态
systemctl stop firewalld.service		# 关闭防火墙
systemctl disable firewalld.service		# 禁用防火墙
```



#### 2 Selinux

```bash
vi /etc/selinux/config			
# 关闭selinux，把SELINUX=enforcing改为SELINUX=disabled即可
```



#### 3 yum源

阿里源

```bash
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
 
yum clean all
yum makecache
```



本地源

```bash
vi /etc/yum.repos.d/CentOS-Base.repo
-------------------------------------------
[rhel-source]
name=Red Hat Enterprise Linux $releasever - $basearch - Source
baseurl=file:///mnt		#/mnt为光盘挂载目录
enabled=1
gpgcheck=0
-------------------------------------------
 
yum clean all
yum makecache
```



#### 4 CPU

```bash
cat /proc/cpuinfo |grep "physical id"|sort |uniq|wc -l 	# 查看物理CPU的个数
cat /proc/cpuinfo |grep "processor"|wc -l		# 查看逻辑CPU的个数 
cat /proc/cpuinfo |grep "cores"|uniq 		# 查看CPU的核数
cat /proc/cpuinfo |grep MHz|uniq 			# 查看CPU的主频
```



#### 5 字符集

```bash
locale -a |grep "zh_CN"			# 查看系统是否安装中文语言包 
yum groupinstall "fonts" -y		# 没有输出，说明没有安装，输入命令安装
 
echo $LANG				# 看看当前系统语言环境
vi /etc/locale.conf		# 设置中文环境
----------------------------------
LANG="zh_CN"
----------------------------------
date    # 验证
```



#### 6 挂载新盘

```bash
fdisk -l
fdisk /dev/sdb
```

```bash
Welcome to fdisk (util-linux 2.23.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0xf5d505e3.
Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p  
Partition number (1-4, default 1): 1
First sector (2048-125829119, default 2048): 
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-125829119, default 125829119): 
Using default value 125829119
Partition 1 of type Linux and of size 60 GiB is set
Command (m for help): w
The partition table has been altered!
Calling ioctl() to re-read partition table.
Syncing disks
```

```bash
mkfs.xfs -f /dev/sdb1
mkdir /app
mount -t xfs /dev/sdb1 /app
vi /etc/fstab
-------------------------------------------
/dev/sdb1 /app                                 xfs     defaults        0 0
-------------------------------------------
```



