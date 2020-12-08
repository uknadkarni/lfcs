# LFCS Module 3: Storage Management

## Lesson 10: Managing Partitions


## Lesson 11: Managing File Systems and Mounts



## Lesson 12: Managing LVM



## Lesson 13: Managing Software RAID


`cat /proc/mdstat`  
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