---
layout: post
title: "Raspberry Pi + LAMP + FTP + SSH (Part 2)"
---

This is part 2 of a 3 part series. [Part 1](http://moam.github.io/2013/06/04/raspberry-pi-lamp-pt1.html) and 
[Part 3](http://moam.github.io/2013/06/04/raspberry-pi-lamp-pt3.html)

##### Install and Configure VSFTP

<kbd>sudo apt-get install vsftpd</kbd>

Open configuration file for editing <kbd>sudo vi /etc/vsftpd.conf</kbd> and amend the following lines
	
* Change **anonymous_enabled=NO**
* Uncomment **local_enable=YES**
* Uncomment **write_enable=YES**
* Uncomment **local_umask=022**
* Uncomment and change **tftpd_banner=Secure FTP server. Access strictly forbidden**

Restart service <kbd>sudo /etc/init.d/vsftpd restart</kbd>

Test and check basic FTP is working from another PC.

##### Install and Configure Apache

Install Apache <kbd>sudo apt-get install apache2</kbd>

Test and check basic Web Site is working (from remote machine *http://ip_address_of_raspberry_pi*)

Install Mod Rewrite <kbd>sudo a2enmod rewrite</kbd>

Add **index.php** to */etc/apache2/mods-enabled/dir.conf* (add index.php to beginning of list) <kbd>sudo vi /etc/apache2/mods-enabled/dir.conf</kbd>

**Begin Optional Configure Virtual Host(s)**

<kbd>cd /etc/apache2/sites-available</kbd>

Copy default configuration file to one of your choice <kbd>sudo cp default hostname.of.your.site</kbd>

Edit the file you just created <kbd>sudo vi hostname.of.your.site</kbd> and amend as below (extract)

{% highlight apache %}
<VirtualHost *:80>
	ServerAdmin webmaster@hostname.of.your.site
	ServerName hostname.of.your.site
	ServerAlias *. hostname.of.your.site

	DocumentRoot /home/your_name/public_html
	<Directory />
		Options FollowSymLinks
		AllowOverride All
	</Directory>
	<Directory /home/your_name/public_html/>
		Options Indexes FollowSymLinks MultiViews
		AllowOverride All
		Order allow,deny
		allow from all
	</Directory>
</VirtualHost>

ErrorLog ${APACHE_LOG_DIR}/main_error.log

...

CustomLog ${APACHE_LOG_DIR}/main_access.log common
{% endhighlight %}

Also change the entry in the default site to show *access.log common*

Make the file you just amended the default <kbd>sudo a2ensite hostname.of.your.site</kbd> (the opposite is <kbd>a2dissite hostname.of.your.site</kbd> 
to disable it)

**End Optional Configure Virtual Host(s)**

Navigate to your home folder and create a new folder for your web documents <kbd>mkdir public_html</kbd>

Restart apache <kbd>sudo service apache2 restart</kbd>

##### Install & Configure PHP5

Install PHP <kbd>sudo apt-get install php5 php-pear php5-suhosin</kbd>

Install PHP libraries <kbd>sudo apt-get install php5-mysql</kbd>

Test and check basic PHP file is working by creating a file with the following content

{% highlight html+php %}
<?php phpinfo(); ?>
{% endhighlight %}

Save the file in the root directory of you web server which you created earlier (public_html) with the *.php* extension **OR** type <kbd>php -v</kbd> at the command prompt.

Edit the configuration file <kbd>sudo vi /etc/php5/apache2/php.ini</kbd> and change the following lines to match those below

* error_reporting = **E_ALL | E_STRICT**
* error_log = **/var/log/php_errors.log**

Manually create the log file <kbd>sudo touch /var/log/php_errors.log</kbd>

Change ownership (www-data is the account APACHE runs under so I believe) <kbd>sudo chown www-data: /var/log/php_errors.log</kbd>

Change permissions <kbd>sudo chmod +rw /var/log/php_errors.log</kbd>

Restart apache <kbd>sudo service apache2 restart</kbd>

#####Install & Configure MySQL Server

Install MySQL <kbd>sudo apt-get install mysql-server</kbd>

Edit the MySQL configuration file <kbd>sudo vi /etc/mysql/my.cnf</kbd> and change to suite the static IP you configured earlier

* bind-address = 192.168.x.y (substitute your own IP here)

Restart <kbd>MySQL sudo service mysql restart</kbd>

Log into MySQL (enter your password) <kbd>mysql -u root –p</kbd>

Use the mysql database (which holds the user table) <kbd>use mysql</kbd>

Create the user(s) you require (type these on the command line)

* create user 'your_username'@'localhost' identified by 'your_password';
* grant all privileges on \*.\* to 'your_username'@'localhost' with grant option;
* create user 'your_username'@'%' identified by 'your_password';
* grant all privileges on \*.\* to 'your_username'@'%' with grant option;
* flush privileges;

Harden the installation <kbd>mysql_secure_installation</kbd>

This is part 2 of a 3 part series. [Part 1](http://moam.github.io/2013/06/04/raspberry-pi-lamp-pt1.html) and 
[Part 3](http://moam.github.io/2013/06/04/raspberry-pi-lamp-pt3.html)