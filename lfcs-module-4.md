# LFCS Module 4: Service Configuration

## Lesson 15: Managing SSH Services
### ssh
`/etc/ssh` has the config files. `sshd_config` is the server config and `ssh_config` is the client config.  

`ssh-keygen` to generate keys for the user. The passphrase is used to encrypt the private key.  
`ssh-copy-id` to copy the public key to the target server. The username on the source can be different from the username on the destination. The key gets copied to `~/.ssh/authorized_keys`  
To prevent entering the passphrase
Use `ssh-agent /bin/bash` to run an agent to cache the key and then `ssh-add` to add your current key to the cache. The keys are cached only within the sub-shell created. Exiting the subshell will lose the cache and require entering the passphrase for ssh logins.  
Use `scp` to copy remote files securely  
 ### rsync 
`rsync` remotely synchronize files with ssh. It uses delta sync. It supports creation and deletion of remote files.  
The following two are different  
`rsync -av /usr/share share`  
`rsync -av /usr/share .`  
  
`rsync -avz foo user@ubuntu` to copy remotely  

### ssh Local Port Forwarding
Use port forwarding to make remote ports available locally.  
ssh port forwarding need the following three:  
* The local port to assign  
* The destination port on the remote machine  
* The intermittent machine you are ssh'ing into  
Example: `ssh -4 -L 2233:remotehost:22 localhost` where -4 specifies IPv4, 2233 is the local port, remotehost and 22 are the remote machine and destination port respectively, and localhost is the intermittent host.  
The only requirement is that the remote host and port should be accessible from the intermittent host  

## Lesson 16: Managing Web Services
Ubuntu works with the *sites-available* and *sites-enabled* directories for virtual host configuration.  
The *sites-available* directory is a development directory, from there you'll move the configuration to *sites-enabled*.  
Use `a2ensite name.conf` to move a virtual host configuration file from the available to the enabled sites.  
### Understanding Apache Log Files
Like any service, Apache log messages are captured by systemd.  
However, Apache has its own log configuration. The resulting files are written to `/var/log`, but these are not managed by `rsyslog`. 
### Understanding Virtual Hosts
One Apache process can provide access to multiple websites  
Make sure name resolution is set up so that the requests arrive at your `httpd` process  
The `httpd` process reads the `httpd` header to find out which host is addressed and sends to the appropriate process  
In case a request does reach the `httpd` process on your server, but there is no matching virtual host configuration, the virtual host that is alphabetically first will offer its contents  
Use `_default_` as a catchall for requests that don't match a specific virtual host  

## Lesson 17: Configuring FTP Services


## Lesson 18: Configuring a Basic DNS Server
### DNS Record Types
**A** IPv4 address  
**AAAA** IPv6 address  
**CNAME** Alias  
**MX** message transfer agent  
**NS** Name Server  
**PTR** pointer from IP address to a name  
**SOA** Start of Authority, which is domain and zone settings  
**SRV** Service record in Kubernetes  
**TXT** Arbitrary text  

### Configuring BIND 
**bind9** on Ubuntu  
The bind9 DNS server can perform both caching and forwarding.  
bind9 will serve as a caching-only name server if no zone files are define.  
Configuration (like defining zone files) done through */etc/named.conf*  
#### Configuring Primary DNS Server
##### Configure the Options File
`/etc/bind/named.conf.options`

**listen-on** define port  
**allow-query** defines clients  
Zone defines the domain(s) serviced by this server  
If you want a caching-only name server, consider using unbound.  



## Lesson 19: Providing NFS and CIFS File Shares


## Lesson 20: Configuring a Database Server 


## Lesson 21: Configuring Basic Email Handling


## Lesson 22: Configuring a Web Proxy



