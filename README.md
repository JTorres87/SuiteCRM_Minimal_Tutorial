# SuiteCRM_Minimal_Tutorial
SuiteCRM Install from Scratch

Start by preparing a minimal CentOS 7 environment. You may get a pefectly great environment for about $5 bucks a month from VULTR.
Please use my refferal link as a donation if you are to use them. You can pick from 14 Datacenters around the world and instantly start a VPS securely with automatic backups and DDOS Protection.

[http://www.vultr.com/?ref=6879438](http://www.vultr.com/?ref=6879438)


For a decent tutorial on how to set up a minimal CentOS7 environment follor the link below.
https://www.howtoforge.com/apache_php_mysql_on_centos_7_lamp

The only exception to the tutorial above I wold usggest is to USE php-mysqlnd instead of php-mysql. The php module has better peformance and can be configured to use php-mysqlnd_ms. This is useful for MASTER/SLAVE environment with the crm. This is useful for advanced database load management.

Get a FREE SSL certificate utilizing certbot. The Electronic Frontier Foundation is pushing for everyone to encrypt! Its good, protect your server.
Learn more at https://letsencrypt.org/ and https://certbot.eff.org/

For security reasons I would sugges tto setup the vultr VPS with a KEY for SSH instead of a password and to only allow phpmyadmin access from your IP. If you have a static IP. You can even get creative and only allow PHPMyAdmin access from LOCALHOST and use an ssh tunnel with putty.

Once you have the environmetn setup we will start the TUT.

You can use PUTTY and puttygen to connect to your server.


Install additional php monudles
```
yum install php-mbstring php-gd gd php-imap php-ldap
```
###Update php.ini settings
use your favorite editor to edit /etc/php.ini
```
vim /etc/php.ini
```
*Configure the following settings*
```
memory_limit = 512M
max_execution_time = 300
error_reporting = E_ALL & ~E_NOTICE
post_max_size = 64M
upload_max_filesize = 64M
display_errors = Off
```

Restart apache
```
systemctl restart httpd.service
```

Download the latest version of SuiteCRM
```
mkdir ~/download
cd ~/download
wget https://github.com/salesagility/SuiteCRM/archive/master.zip
```

Extract to /var/www/html/devcrm or your appropiate directory in which you woul dlike the CRM to reside.

```
yum install unzip
unzip ~/download/master.zip -d /var/www/html/devcrm/
```

Next we move the SuiteCRM-master contents up a directory
```
 mv /var/www/html/devcrm/SuiteCRM-master/* /var/www/html/devcrm/
 rm -rfv /var/www/html/devcrm/SuiteCRM-master
```

Go to phpmyadmin and create a Database
USERS>add user
Username: Whatevr you want
Host:Local
Password: Generate one

CHECK
Create database with same name and grant all privileges.
Hit GO

###Begin SuiteCRM Install
**Set permissions**
```
cd /var/www/html/devcrm/
chown -R apache:apache /var/www/html/devcrm
chmod -R 755 ./
chmod -R 775 cache custom modules themes data upload config_override.php

```

Go to YOURDOMAIN.TLD/YOURCRMDIRECTORY to begin the isntallation process.
Click "I Agree" and then click "Next"

Ensure all system checks are "OK" or you will have issues with the installation. On the system check screen you will find the corn job that needs to be setup to run.

Should look something like:
```
*    *    *    *    *     cd /var/www/html/devcrm; php -f cron.php > /dev/null 2>&1 
```

So go to command line and enter crontab -e (MAKE SURE TO BACKUP CRONTAB as -r and -e are right next to eachother, and -r deletes the crontab....and I have done it before and it sucks to not back it up)vim cron
```
mkdir ~/crontabs
cp /var/spool/cron/USERCRONTABFILE ~/crontabs/USERCRONTABFILE.bak
```

Follow the Configuration and enter setting accordingly. If you have a primary smtp email to send rom set tha tup here as well.

