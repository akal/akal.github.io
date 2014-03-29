---
layout: post
title:  "Ubuntu Server setup"
date:   2014-02-28 20:46:00
categories: ubuntu server
---
I've just started to set up a develpment server at
[DigitalOcean](http://www.digitalocean.com). This post will document
progress.

First, when creating a user, the adduser utility gives the following
error message:

```
    perl: warning: Setting locale failed.
    perl: warning: Please check that your locale settings:
            LANGUAGE = (unset),
            LC_ALL = (unset),
            LC_CTYPE = "UTF-8",
            LANG = "en_US.UTF-8"
        are supported and installed on your system.
```
To configure the first two variables, I've added `LC_ALL="en_US.utf8"` to
`/etc/environment`.

Then, the user can be added to sudoers group:
`sudo adduser <username> sudo`

Adding swap file:

```
sudo dd if=/dev/zero of=/swapfile bs=1024 count=512k
sudo mkswap /swapfile
sudo swapon /swapfile
# check with:
swapon -s
```

To make it permanent, add the line to `/etc/fstab`:
`/swapfile       none    swap    sw      0       0`

Tuning swappiness:
To `/etc/sysctrl.conf`, add `vm.swappiness=20`, then
`sudo sysctl -p`

To prevent other from reading it:

```
sudo chown root:root /swapfile 
sudo chmod 0600 /swapfile
```

Disallow root login:
Edit `/etc/ssh/sshd_config`, set `PermitRootLogin no`,
then `sudo reload ssh`

Install fail2ban for ssh brute force attacks. With some additional
configuration, it can protect a bunch of other services too.

```
sudo apt-get install fail2ban
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo service fail2ban restart
# check fail2ban record in iptables
sudo iptables -L
```

Add new user who will execute sensitive apps eg web application:

```
sudo adduser cms
sudo usermod -s /bin/false cms
```


Install postgresql:
`sudo apt-get install postgresql postgresql-contrib postgresql-client`

Change the postgres user password, connect as
`sudo -u postgres psql postgres`
Set a password with
`\password postgres`
Exit with CTRL+D

Create db
`sudo -u postgres createdb sandbox`

Further reading [here](https://help.ubuntu.com/community/PostgreSQL)

To be continued..












