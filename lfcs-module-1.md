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
<domain> <type> <item> <value> .  
Where domain can be a user name, a group name, a wildcard (* for default entry), a wildcard % for group syntax  
type can be hard/soft  
item can be core, data, fsize, memlock, nofile, etc  

`sudo less /etc/security/limits.conf` 

In a more recent version, systemd cgroups takes over from ulimits

### Pluggable Authentication Modules (PAM)
`/etc/pam.d` describes which service goes to which backend  
If you want to integrate a third party fingerprint reader with PAM, simply create a module like `pam_finger.so` that understands how to read the finger print and authenticate with PAM. Then include the module in the PAM configuration file as *required*  

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



