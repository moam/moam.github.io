---
layout: post
title: "Raspberry Pi + LAMP + FTP + SSH (Part 3)"
---

This is part 3 of a 3 part series. [Part 1](http://moam.github.io/2013/06/04/raspberry-pi-lamp-pt1.html) and 
[Part 2](http://moam.github.io/2013/06/04/raspberry-pi-lamp-pt2.html)

##### Configure phpMyAdmin for Remote Access 

For the purposes of this excercise I downloaded & installed it on Windows PC (you’ll need to be running some sort of web server for it to work – doh!)

Open *config.inc.php* (the phpMyAdmin configuration file) in your favourite text editor

Add the following (below the First Server settings for localhost) making sure you get the domain correct

{% highlight apache %}
/*
 * Second server
 */
$i++;
/* Authentication type */
$cfg['Servers'][$i]['auth_type'] = 'cookie';
/* Server parameters */
$cfg['Servers'][$i]['host'] = 'your.domain.name';
$cfg['Servers'][$i]['connect_type'] = 'tcp';
$cfg['Servers'][$i]['compress'] = false;
/* Select mysql if your server does not have mysqli */
$cfg['Servers'][$i]['extension'] = 'mysqli';
$cfg['Servers'][$i]['AllowNoPassword'] = false;
{% endhighlight %}

Save the file

When you start phpMyAdmin you should have a drop down box where you can select the server instance

##### Harden SSH Part 1

Open up the configuration file for editing <kbd>sudo vi /etc/ssh/sshd_config</kbd> and change the following

* Set **PermitRootLogin no**
* Add (at bottom of file) **AllowUsers your_user_name**
* Uncomment **Banner /etc/issue.net** (assuming you need a banner)

Create banner file <kbd>sudo vi /etc/issue.net</kbd> and add in whatever text you want to display when you telnet into your server

Restart ssh service <kbd>sudo service ssh restart</kbd>

Test and check basic SSH working e.g.

* Download & install PuTTY (for the purposes of this excercise) to your Windows PC
* Set Host Name or IP Address
* Leave port = 22
* Give the session a name & save it
* Load the session & click Open
* Login with your username & password (assuming you get that option !)

##### Harden SSH Part 2 (PuTTY & Public / Private Keys)

Open PuTTYgen which will be used to create the public & private keys

Generate a key

Add a Key Phrase (i.e. SomeSortOfPassword)

Save the Public & Private Keys

Open PuTTY and login using the name & password method

Create a new directory <kbd>mkdir /home/your_user_name/.ssh</kbd>
Create a new file in that directory <kbd>vi /home/your_user_name/.ssh/authorized_keys</kbd> (this will open a new blank file)

Flip back to PuTTYgen and copy the contents of the Public Key (EXCLUDING the last space + rsa-key-YYYYMMDD)

{% highlight apache %}
ssh-rsa ABC123... blah blah ...321CBA== rsa-key-19000101
{% endhighlight %}

Flip back to PuTTY and right click in the vi screen – this will paste the contents of the key into that file

Make sure the text is in one continuous line and scroll to the end. Using the append key <kbd>[a]</kbd> add a space + username@hostname 
(i.e. your_user_name@server_name)

{% highlight apache %}
ssh-rsa ABC123... blah blah ...321CBA== your_user_name@server_name
{% endhighlight %}

Save the file <kbd>[:wq]</kbd> – re-open it to double check it's saved OK

Amend the permissions to the .ssh folder and authorized_keys file

* <kbd>chmod 700 /home/your_user_name/.ssh</kbd>
* <kbd>chmod 600 /home/your_user_name/.ssh/authorized_keys</kbd>

Open another PuTTY session and load up the session you want. Carry out the following actions

* Select from the options *Connection -> Data -> Auto-login Username* and add your username
* Select from the options *Connection -> SSH -> Auth* and browse for the private key file you created earlier
* Select from the options *Session -> Save*
* *Open* (to launch) then you should get prompted for your passphrase

##### Extras

**Disable 'default' site** - If you enter the IP address of the Raspberry Pi (server) in a browser then you’ll get the default site page 
(located in /var/www). To prevent this from happening (so that jokers from the internet who just enter your external IP address won't be able to 
display it either) then enter the following command <kbd>sudo a2dissite default</kbd>

The restart apache <kbd>sudo service apache2 restart</kbd>

In fact (even better) I believe that it will display the site which appears first in your list of virtual servers (alphabetically) which in this case 
is the only one I have. If you have more than one it will pick the first (alphabetically).

To re-enable the default site <kbd>cd /etc/apache2/sites-available</kbd> (may not be required) then <kbd>sudo a2ensite default</kbd>

**Apache Hardening** - Edit */etc/apache2/conf.d/security* and add or amend the following

* *ServerSignature Off*
* *ServerTokens Prod*
* *TraceEnable Off*

Apache Signature or Apache Banner is basically the same thing. It is an application name together with version name that is printed when performing a 
web request. Nobody actually needs this information at all, but it is enabled by default.

HTTP TRACE request is used to echo back all received information. It can be tricked to print HTTP cookies and as a result steal HTTP session. 
Basically this request can be used as part of the Cross Site Scripting attack, or XSS. It is recommended to disable it as a security precaution.

**Apache Logging Updates** - In the *apache2.conf* file make the following changes (if you like, saves logging loads of stuff you may never need)

{% highlight apache %}
/* Under "*Include ports.conf*" */
# Mark requests for resource files - (files with these extensions don't get logged)
SetEnvIf Request_URI ".(ico|jpg|png|gif|js|css|txt|woff|ttf|svg|eot?|swf)$" dontlog

# Mark requests from the loop-back address - (don't log traffic from loop-back adapter)
SetEnvIf Remote_Addr "^127" dontlog
# Mark requests from local lan - (don't log traffic from local lan)
SetEnvIf Remote_Addr "^192" dontlog

Change the "common" log format - (removes any unnecessary info)
LogFormat "%h %t \"%r\" %>s %O" common
{% endhighlight %}

In the "*your.domain.name*" & "*default*" sites-available files alter the last line to (respectively)

* CustomLog ${APACHE_LOG_DIR}/access.log common env=!dontlog
* CustomLog ${APACHE_LOG_DIR}/main_access.log common env=!dontlog

Restart apache <kbd>sudo service apache2 restart</kbd>

**Apache Default Site Files** - Amend permissions to */var/www* directory (so you can managed the files)

* <kbd>sudo chmod o+w /var/www</kbd>
* <kbd>sudo chmod g+w /var/www</kbd>

Remove default *index.html* <kbd>cd /var/www</kbd> then <kbd>sudo rm index.html</kbd>

FTP new files into place (assuming you want to, you may want to display something instead of a blank or not found page)

Congratulations you should now have a fully working LAMP server with remote SSH access. Couple this with a Dynamic DNS Host and you'll be able to access 
it from anywhere. With lower power consumption and (so far) great reliability it should stay trouble free for a long time.

This is part 3 of a 3 part series. [Part 1](http://moam.github.io/2013/06/04/raspberry-pi-lamp-pt1.html) and 
[Part 2](http://moam.github.io/2013/06/04/raspberry-pi-lamp-pt2.html)