<h1 align="center">Linux 

</h1>
<p align="center">A simple but effective iptables script that offers basic protection to be used on Linux servers. I mean, it's better than nothing... ;)</p>

<!--ts-->
* [Prerequisites](#prerequisites)
* [Installation](#installation)
  * [Debian 9, 10 and 11](#debian-9-10-and-11)
  * [Debian 8](#debian-8)
  * [Debian 5, 6 and 7](#debian-5-6-and-7)
<!--te-->

<h2>Prerequisites</h2>

These scripts were developed using IPtables.
Yes, I know it's deprecated, that I should be using nftables, etc but I didn't have the time to study it yet. Sorry (not sorry!).

You must have IPtables available on your server for these scripts to work. If you don't, just have it installed:
```bash
apt install iptables
```

Obs: I'm using Debian (best GNU/Linux distro, if you ask me!), so everything here will assume that you are using it too.


<h2>Installation</h2>

-Create firewall scripts directory:
```bash
# mkdir /etc/fwpmnh
```

-Clone this git repository:
```bash
# cd /etc/fwpmnh
# git clone https://github.com/EdHart85/Linux-Firewall .
```
 
-Adjust permissions:
```bash
# chmod 750 /etc/fwpmnh
# chmod 750 /etc/fwpmnh/fwpmnh
# chmod 640 /etc/fwpmnh/local
# chmod 640 /etc/fwpmnh/mgmt
# chmod 640 /etc/fwpmnh/services
```

 -Create symbolic link:
```bash
# ln -s /etc/fwpmnh/fwpmnh /usr/sbin/fwpmnh
```

-Check your network interface name (Ex: ens192, eth0, etc) and broadcast address and then update the respective variable in the "local" file.

-Adjust the "mgmt" and "services" files according to your needs.

-Enable this firewall as a service on systemd (see sections below).

 

<h3>Debian 9, 10 and 11</h3>

-Create systemd service file:
```bash
# systemctl edit fwpmnh.service --force --full
```

```bash
[Unit]
Description=Linux Firewall

[Service]

# Tells systemd that this service will only run once, since it does not have a continously running process
Type=oneshot

# Since there is no continous running process, this tells systemd to consider this service up once it has started
RemainAfterExit=yes

ExecStart=/bin/bash /etc/fwpmnh/fwpmnh start
ExecStop=/bin/bash /etc/fwpmnh/fwpmnh stop
ExecReload=/bin/bash /etc/fwpmnh/fwpmnh restart


[Install]
WantedBy=multi-user.target
```

-Enable and start the service:
```bash
# systemctl enable fwpmnh.service
# systemctl start fwpmnh.service
# systemctl status fwpmnh.service
```
 

<h3>Debian 8</h3>

-Create systemd service file:
```bash
# vi /lib/systemd/system/fwpmnh.service
```

```bash
[Unit]
Description=Linux Firewall

[Service]

# Tells systemd that this service will only run once, since it does not have a continously running process
Type=oneshot

# Since there is no continous running process, this tells systemd to consider this service up once it has started
RemainAfterExit=yes

ExecStart=/bin/bash /etc/fwpmnh/fwpmnh start
ExecStop=/bin/bash /etc/fwpmnh/fwpmnh stop
ExecReload=/bin/bash /etc/fwpmnh/fwpmnh restart


[Install]
WantedBy=multi-user.target
```

-Enable and start the service:
```bash
# systemctl enable fwpmnh.service
# systemctl start fwpmnh.service
# systemctl status fwpmnh.service
```
 

<h3>Debian 5, 6 and 7</h3>

-Create a symbolic link:
```bash
# ln -s /etc/fwpmnh/fwpmnh /etc/init.d/fwpmnh
```

-Enable and start the service:
```bash
# update-rc.d fwpmnh defaults
# /etc/init.d/fwpmnh start
```
