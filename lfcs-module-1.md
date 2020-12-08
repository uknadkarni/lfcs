# LFCS Module 1: Advanced System Administration
## Lesson 1: An Introduction to Bash Shell Scripting
In a script, use `read` to read from stdin

### Loop Structures
if... then... fi  
while... do... done  
until... do... done  
case... in... esac  
for... in... do... done  

`while [ $COUNTER -gt 0 ]`  
`do`  
`  echo "Counter is $COUNTER"`  
`done`  
  
  
`while true`  
`do` .  
`   echo "do something"` .  
`done` .  

### Conditional Statements
`if [ -z $1 ]`    
`then` . 
`  echo "You have to provide an argument"` . 
`  exit 6` . 
`fi` . 

## Lesson 2: Managing Local Security
### ulimits
Each line in limits.conf describes a limit for user/group in the form  
`<domain> <type> <item> <value>`  
Where `domain` can be a user name, a group name, a wildcard (* for default entry), a wildcard % for group syntax  
`type` can be hard/soft  
`item` can be core, data, fsize, memlock, nofile, etc  

`sudo less /etc/security/limits.conf` 

In a more recent version, systemd cgroups takes over from ulimits

### Pluggable Authentication Modules (PAM)
`/etc/pam.d` describes which service goes to which backend  
If you want to integrate a third party fingerprint reader with PAM, simply create a module like `pam_finger.so` that understands how to read the finger print and authenticate with PAM. Then include the module in the PAM configuration file as required  

### Secure Mount Options
Different mount options can be used in `/etc/fstab`  
* *nodev* disables accessing devices  
* *nosuid* disables any binary that has a SUID or SGID set  
* *noexec* restricts execution of binaries  
* *ro* read-only  
To apply them on a running system, run `mount -o remount`  

## Lesson 3: Configuring Networking

### Applying persistent networking in Ubuntu
Ubuntu uses `netplan` -- a YAML network configuration abstraction for various backends.  
The configuration file is `/etc/netplan/50-cloud-init.yaml`  
Edit the file and run `netplan apply` and then view the results using `ip a` and `ip route show`   
On Ubuntu Server use the `netplan` cli. On a workstation, use the NetworkManager GUI utility.  

### Using systemd-networkd
`systemctl enable systemd-networkd`  
`systemctl enable systemd-resolvd`  
`rm /etc/resolv.conf`  
`ln -s /run/systemd/resolv/resolv.conf /etc/resolv.conf`  
`mkdir /etc/systemd/network`  
And create appropriate configuration files  

Remember to `reboot` after making the configuration changes

### Commands
`netplan`



## Lesson 4: Managing Advanced Systemd Features
### Modifying Systemd Units
`systemctl show unit.type` shows possible unit parameters  
Change settings using `systemctl edit unit.type`  
After changing settings, run `systemctl daemon-reload` to update systemd  
Next, use `systemctl restart unit.type` to restart the unit with new settings.  
Use `sshd.service` as an example of a unit  

`systemctl list-unit-files | grep timer`  


## Lesson 5: Configuring rsyslogd
Settings are stored in `rsyslog.conf` under `/etc`  
Snapin files are stored in `rsyslog.d` directory under `/etc`  
Functionality can be extended by using modules  
Log actions can be defined by *facility, priority, destination*   
Example: To log actions from all facilities and all priorities to the destination `/var/log/everything.log` simply add the line `*.*       /var/log/everything.log` to the snapin file in `rsyslog.d`  
rsyslog supports remote logging  
* Enable remote log reception using `$ModLoad imudp / imtcp and UDPServerRun 514 / InputTCPServerRun 514`  
* Send log messages to remote servers using `*.* @@ip-address-of-remote-server:514` in the client's configuration  
** Where the *.* denotes all facilities and all priorities; the double @ denotes tcp (use a single @ for UDP); and 514 is the port where rsyslog runs  
### Log rotation
Log rotation runs as a daily cron job  
Configuration in `/etc/logrotate.conf` and drop in files in `/etc/logrotate.d`   
You don't need to restart an service when changes are made to logrotate configuration files because logrotate is kicked off by a cron job  

### Making systemd-journald logs persistent
In the `[Journal]` section of the configuration file -- `/etc/systemd/journald.conf` -- set `Storage=persistent`  
See `man 5 journald.conf`  
Note: Remember to create the directory `/var/log/journal` to store the journal persistently
Use `systemctl force-reload systemd-journald` to reload the journal  

## Lesson 8: Basic Kernel Management
### Working with kernel modules
`lsmod` lists kernel modules that are currently loaded  
`modinfo` provides more info about modules  
`modprobe` is used to load modules (but this isn't needed as much because the kernel load modules upon sensing new hardware)
Kernel module parameters are specified in `/etc/modprobe.d`  
`lspci -k` lists the kernel modules on the pci bus

The module configurations are in `/etc/modprobe.d`

### proc
`/proc` is a pseudo file system that provides and interface to the linux kernel  
`/proc/sys` contains tunables to dynamically change kernels in runtime
To make changes in `/proc/sys` persistent make changes to `sysctl.conf`  

Use `sysctl -a` to view the tunable parameters  

### Commands
`lsmod, modinfo, modprobe, sysctl`  

## Lesson 7: Managing the Boot Procedure
### Shutting down the system
`systemctl poweroff`  
`systemctl reboot`  
`systemctl hibernate`  
`poweroff`  
`reboot`  
`shutdown`  

### Configuring the GRUB2 Boot Loader
While booting, specific kernel options can be used to alter the kernel behavior  
`systemd.unit=emergency.target`  to start with minimal services.  
`systemd.unit=rescue.target` to start without password, with minimal services and networking.  
`mem=256M` to start with predefined memory  
Write persistent changes to `/etc/default/grub`  
Commit changes to the bootloader using `grub2-mkconfig`  
* `grub2-mkconfig -o /boot/grub/grub.cfg` on BIOS systems  
* `grub2-mkconfig -o /boot/efi/EFI/xxx/grub.cfg` on UEFI systems  

In exceptional cases, start in initrd mode with `rd.break`  
