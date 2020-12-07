# LFCS Module 2: Managing Security Features

## Lesson 8: Configuring a Firewall
`netfilter` is the Linux firewall. It looks at input packets, output packets and forwarded packets. `iptables` allows you to set rules for the input packets, output packets and forwarded packets. But `iptables` are complex. Instead, use `ufw` for Ubuntu and `firewalld` for RHEL.  

### ufw (universal firewall)
`sudo ufw enable`  
`sudo ufw allow ssh`  
Note: Allow ssh before you enable ufw
`sudo ufw reject out ssh`  
`sudo ufw status`  
`sudo ufw delete reject out ssh`  
`sudo ufw deny proto tcp from 10.0.0.10 to any port 22`  
`sudo ufw reset`  
`sudo ufw app list`  
`sudo ufw app info Samba`  
`sudo ufw logging on`  

### iptables
iptables/ip6tables is an administration tool for IPv4/IPv6 packet filtering and NAT  
iptables "match on exit". So, ensure that the broadest rule is lower down the priority.  
`iptables -L` tells you if you have firewalling  

`sudo iptables -F` to flush current rules  
`sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT` an example rule  
`sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT` an example rule  
`sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT` to open an input SSH port. If you open up an INPUT port for ssh, remember to open an OUTPUT port with state ESTABLISHED,RELATED. Example `sudo iptables -A OUTPUT -m state --state ESTABLISHED,RELATED -j ACCEPT`  
`sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT` to open an input port for a web server  
`sudo iptables -I INPUT 1 -i lo -j ACCEPT` to insert a rule for the loop back address  
`sudo iptables -P INPUT DROP` drop all packets to the INPUT chain  
`sudo iptables -P INPUT ACCEPT` accept all packets to the INPUT chain  
  
  `iptables-save` to save/persist the iptables  
  `iptables-restore` to restore ip tables from the configuration file that `iptables-save` has created  



## Lesson 9: Managing AppArmor
### Configuring AppArmor
Enable AppArmor through its systemd service  
Ensure an application profile is available in `/etc/apparmor.d`  
Alternatively, create a profile yourself using:  
* `aa-genprof /your/application`  
* Run the application from another terminal  
* Press `s` to scan for application events  
Loaded profiles are in `/sys/kernel/security/apparmor`  
* Use `aa-status` as an alternative  
Once the application is profiled, you may move the profile from *complain* mode to *enforce* mode using `aa-enforce /your/application`  





