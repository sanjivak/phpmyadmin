phpmyadmin web access only my ip

https://www.thepolyglotdeveloper.com/2014/09/securing-your-apache-phpmyadmin-installation/

Securing Your Apache phpMyAdmin Installation September 27, 2014 Nic RaboyPHP, Servers

If you’re using a LAMP (Linux, Apache, MySQL, PHP) stack, chances are you’re going to be using phpMyAdmin. By default, your phpMyAdmin installation is not very secure and chances are your MySQL database has a treasure trove of excellent information for a malicious user.

By following these steps, you can make it significantly tougher for your phpMyAdmin installation to be exploited.

Change the alias path for accessing Crawlers used by malicious people are designed to look for vanilla installations of phpMyAdmin. When a crawler is used on your site, chances are it will look at http://www.yoursite.com/phpmyadmin before moving onto its next check. You can easily beat this by changing the alias of /phpmyadmin to something like /dbadmin. Such a little change is often enough to throw the weaker bots off their game.

In your Linux terminal navigate to /etc/phpmyadmin/apache.conf and change the following:

Alias /phpmyadmin /usr/share/phpmyadmin
Alias /dbadmin /usr/share/phpmyadmin In order for the change to activate you must restart Apache. After restarting, to access phpMyAdmin you will need to navigate to http://www.yoursite.com/dbadmin.

Restrict access to certain domains or sub domains on your server Let’s say you have 1000 domains and sub domains running on your server. You don’t want all 1000 to have access to phpMyAdmin because this will leave you pretty open to attack from 1000 different end points. Let’s limit which domains can access this protected area. In your /etc/phpmyadmin/apache.conf file comment out the alias lines near the top. This will remove full access to phpMyAdmin. Now navigate to one of your virtual hosts typically found in /etc/apache2/sites-available and edit one of them.

<virtualhost *:80> ServerName www.yoursite.com DocumentRoot /path/to/your/site Alias /dbadmin /usr/share/phpmyadmin Adding the alias line we had in the apache.conf file to our virtual host will make it accessible from that particular domain. In this particular scenario phpMyAdmin will only be accessible from http://www.yoursite.com/dbadmin and none of your other 999 domains.

For this to activate you must restart Apache. If you were only changing the virtual host and not the apache.conf file you could get away with a reload command.

Allow access from specific client IP addresses only It is often a good idea to lock down phpMyAdmin access to IP addresses that you know are secure. Maybe you only want to be able to access via your home network or company network, but nothing else.

In your Linux terminal navigate to /etc/phpmyadmin/apache.conf and add the following lines:

<Directory /usr/share/phpmyadmin>

Options FollowSymLinks
DirectoryIndex index.php

Order Allow,Deny
Allow from 127.0.0.1
Allow from 68.24.9.0/24

<IfModule mod_php5.c>
The above lines will allow access from 127.0.0.1 and all IP addresses on 68.24.9.x. This scenario would represent if you had the need to allow access from a business network.

In order for the change to activate you must restart Apache. After restarting, navigating from an unauthorized IP will result in an access forbidden error.

Use an SSL certificate for encrypted HTTPS requests You don’t want malicious people to be able to sniff your password when you try to sign into your phpMyAdmin panel. To prevent this you’re going to need to install an SSL certificate so your communication is encrypted. There are a ton of different SSL certificate providers around. I personally use PositiveSSL by Comodo because it is affordable, but it is entirely up to you.

I will explain how to install SSL certificates in an Apache environment in a future post.
