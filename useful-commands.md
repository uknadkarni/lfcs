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

`!mo` repeats the last command from the command prompt that begins with an mo.  

An example systemd mount file is located in `/usr/share/systemd/`
Simply `sudo cp -v /usr/share/systemd/tmp.mount /etc/systemd/system/ `  
Edit the new mount file, say `books.mount`  
Then run `systemctl daemon-reload` followed by `systemctl enable --now books.mount`  

`systemctl list-unit-files`
`systemctl list-unit-files | grep automount`

`sudo -i`
