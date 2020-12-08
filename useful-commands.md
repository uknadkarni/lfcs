# Useful commands
`ss -tuna`  
`netstat -tulpen`  

`systemctl daemon-reload`  
`systemctl force-reload <service>`  
  
`chattr -R +i ~/.ssh`  

Report the file system disk space usage  
`df -h`  

To view partitions on your disk  
`sudo parted /dev/sdb 'print'`  

Unmount the filesystem  
`sudo umount /dev/sdb`  

To scan available IP addresses in a given network  
`nmap -sn 172.17.0.0/24`