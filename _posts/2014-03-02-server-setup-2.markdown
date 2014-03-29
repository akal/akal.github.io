---
layout: post
title:  "Ubuntu Server setup 2"
date:   2014-03-02 20:00:00
categories: ubuntu server python 
---

```
sudo apt-get install apache2
# enable ssl
sudo a2enmod ssl
# create self-signed cert as descibed in
https://help.ubuntu.com/13.10/serverguide/certificates-and-security.html
# finish enabling https as described in 
https://help.ubuntu.com/13.10/serverguide/httpd.html
# check the https site after this is done, it should be successful

sudo apt-get install python-setuptools
easy_install pip
pip intall virtualenv

# add cms user to sudoers, to make setting the environment up easier
sudo adduser cms sudo

# set up target dir
sudo su cms
cd /home/cms/
mkdir -p staging/cms
cd staging/cms
virtualenv env
# activate the virtualenv
source env/bin/activate

# install components
pip install django
# install oauth2 from https://github.com/simplegeo/python-oauth2
cd /home/cms/
mkdir tmp 
cd tmp
git clone https://github.com/simplegeo/python-oauth2
cd python-oauth2
python setup.py install



# remove cms from sudoers
sudo deluser cms sudo


Add staging environment

Setting up git
`http://git-scm.com/book/en/Git-on-the-Server-Setting-Up-the-Server`
```
