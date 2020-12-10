# LFCS Module 3: Storage Management

## Lesson 10: Managing Partitions
Devices in linux are usually called `/dev/sd*` where *dev* stands for device; and sd stands for scsi disk.  
There are two partitioning schemes:
1. MBR: This is used by centOS and allows up to 4 primary partitions; to add more, use logical partitions.  
2. GPT: This is used by Ubuntu and is a newer partitioning scheme.  
  
`lsblk` to list block devices.  
`blkid` to locate/print block device attributes.  
`fdisk` to manipulate disk partition table  
`partprobe` to inform the OS of partition table changes  
`gdisk` for GPT Partitions  
`mkfs` to create filesystems  
`findmnt` to see where in the directory structure devices are mounted.  
To mount a device `mount /dev/sdvb1 /data`
`umount /dev/sdb1` to unmount  
To make the mount persistent add to `/etc/fstab`
`lsof` to list open files if umount fails  

The sole purpose of extended partitions is to add logical partitions in it. Think of extended partitions as an empty box.  
`fdisk` creates partitions on MBR and `gdisk` on GPT  

Use `fstrim` to trim the SSD. i.e discard unused blocks on a mounted filesystem.  

#### To create a Swap Partition
Use `gdisk`  
Instead of `mkfs`, use `mkswap /dev/sdb1`  
Activate swap with `swapon /dev/sdb1`  
  
The `/etc/fstab` entry will look like:  
`/dev/sdb1     swap     swap     defaults    0 0`  

Use `vmstat` to monitor swap and `free -m` to tell you if any swap is allocated.  
On a healthy server, at least 30% should be available for buffers and cache (buff/cache).  

`vmstat 2 10` and monitor si and so.  
#### To create encrypted disks
`cryptsetup luksFormat /dev/xvdc4`  
`sudo cryptsetup luksOpen /dev/xvdc4 secret`  
`sudo mkfs.ext4 /dev/mapper/secret`  
`sudo mkdir /mnt`  
`mount /dev/mapper/secret /mnt`  
`umount /mnt`  
`cryptsetup luksClose secret`  


To make this peristent:  
The `/etc/fstab` entry contains `/dev/mapper/secret      /secret  ext4   defaults                0 0`  
The `/etc/crypttab` entry contains `secret /dev/xvdc4`  

## Lesson 11: Managing File Systems and Mounts
### Filesystems
A filesystem is a data structure to store data on disks.  
The three major types of Linux Filesystems are: EXT4, XFS and BTRFS  
#### EXT4
`mkfs.ext4 /dev/sdb`  
  
To create many small files on disk, you may want to increase the number of inodes on the ext4 filesystem by running  
`mkfs.ext4 -N <number of inodes> /dev/sdb`  

`tune2fs -l /dev/sdb` to list all properties of the filesystem  

To create labels  
`tune2fs -L videos /dev/sdb`  

#### XFS
`xfs_admin`  
`xfs_info`  
`xfs_repair`  

To create labels  
`xfs_admin -L books /dev/sdb`  



### Mounting filesystems
Edit `/etc/fstab` and run `mount -a`  



## Lesson 12: Managing LVM
LVM: Logical Volume Manager  



## Lesson 13: Managing Software RAID

`cat /proc/mdstat`  
Create two or more RAID paritions.  
`sudo mdadm --create /dev/md0 --level=1 --raid-disks=2 /dev/xvdb /dev/xvd`  
`sudo mkfs.ext4 /dev/md0`  
Add to `/etc/fstab`  
`/dev/md0 /raid ext4 default 0 0`  
  
`sudo mdadm --detail --scan > mdadm.conf`  
`sudo mv mdadm.conf /etc/mdadm/mdadm.conf`  
`sudo cat /proc/mdstat`  
`sudo mdadm --detail /dev/md0`  

`sudo mdadm --create /dev/md2 --level=5 --raid-disks=3 --spare-devices=1 /dev/xvdh1 /dev/xvdh2 /dev/xvdh3 /dev/xvdh4`  
`sudo mdadm --fail /dev/md0 /dev/xvda`  
`sudo mdadm  --remove /dev/md0 /dev/xvda`  
`sudo mdm --stop /dev/md0`  
`sudo mkfs.ext4 /dev/md0`  
`sudo umount /dev/md0`  

## Lesson 14: Managing User Quotas
Quotas are used to limit the amount of disk space a user or group can use on a filesystem. To limit the amount of space for directories, use partitioning of disks or separate disks instead. XFS and EXT4 don't support limiting disk space for directories using quotas; BTRFS does.  
### To Set Up User Quotas
Install the quota package  
`sudo apt install quota linux-generic`  
`quota --version`  
  
Edit `/etc/fstab` and add these properties to the partition: `usrquota,grpquota`  
  
Mount the edited `/etc/fstab`  
sudo mount -a  
And confirm using `cat /proc/mounts`  

Run quotacheck to 
`sudo quotacheck -ugm /`  
This should create two files `/aquota.user` and `/aquota.group`  

Turn quota on with `sudo quotaon -v /`  

Use `edquota` to set quota for specific users  

Use `repquota -aug` to report quota usage  