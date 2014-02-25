---
layout: post
title: "Raspberry Pi + LAMP + FTP + SSH (Part 1)"
---

This is part 1 of a 3 part series. [Part 2](http://moam.github.io/2013/06/04/raspberry-pi-lamp-pt2.html) and 
[Part 3](http://moam.github.io/2013/06/04/raspberry-pi-lamp-pt3.html)

Right folks I thought it was about time I got my finger out and published details of how I configured my Raspberry Pi with the LAMP stack as well as 
FTP and SSH. I'm sure this isn't the only way to do it and other people may have their own view on how things should be done. People may even gasp in 
horror at what I've done should I have missed out an important security hardening step or introduced some horrendous security flaw. If you spot any 
howlers I would appreciate a quick word in my shell-like so I can learn where I went wrong and put it right.

I won't be covering any specifics about connecting the Raspberry Pi to other devices or the creation of the Operating System on the SD card.

##### Initial Configuration

So you have connected your device to a monitor, you have a network connection and have input devices attached. After booting the device for the first 
time you will be presented with a menu. My settings are listed below.

* Expand root partition to fill SD card - **YES**
* Set keyboard layout - **NO**
* Change password for pi user - **YES**
* Set Locale - **NO**
* Set Timezone - **NO**
* Enable SSH server - **YES**
* Start desktop on boot - **NO**
* Try to upgrade configuration - **YES**

Follow the onscreen prompts (where applicable) to configure those items that you have enabled. After finialising the configuration the OS will boot to the 
command prompt. 

##### Update IP address to static & check DNS settings

To set the static IP address open the configuration file for editing <kbd>sudo vi /etc/network/interfaces</kbd>

Make sure the file contents are amended e.g.

{% highlight apache linenos %}
auto eth0
iface eth0 inet static
	address 192.168.1.x (whatever you want)
	netmask 255.255.255.0
	gateway 192.168.1.254
	dns-nameservers 192.168.1.254
{% endhighlight %}

Check to make sure DNS is set by opening the configuration file for editing <kbd>sudo vi /etc/resolv.conf</kbd>

##### Change Hostname

Open up the configuration file for editing <kbd>sudo vi /etc/hostname</kbd>

Change the default of **raspberrypi** to one of your choosing

Open up another configuration file for editing <kbd>sudo vi /etc/hosts</kbd>

Change the default of **raspberrypi** to one of your choosing

Reboot the Raspberry Pi by typing <kbd>sudo reboot</kbd>

##### Upgrade the OS and Firmware

At the command prompt type <kbd>sudo apt-get update</kbd>. When this has completed type <kbd>sudo apt-get upgrade</kbd>

##### Create A New User

At the command prompt type <kbd>sudo useradd –m your_user_name</kbd>. Then type <kbd>sudo passwd your_user_name</kbd> followed by the password of your choice.

Edit a configuration file to give the new user the necessary permissions <kbd>sudo vi /etc/group</kbd>

Add the new user to the end of all the groups that 'pi' is in i.e. *adm:x:4:pi,your_user_name*

Exit from the 'pi' account by typing <kbd>exit</kbd> at the command prompt and test your new login

##### Give new account ability to SUDO

Add standard user to configuration file (opens file in nano) <kbd>visudo</kbd>

Comment out the line **pi ALL=(ALL) NOPASSWD: ALL**

Add a line at the bottom <kbd>your_user_name ALL=(ALL) ALL</kbd>

Press <kbd>ctrl+X</kbd> then <kbd>Y</kbd> then the <kbd>Enter</kbd> key

This is part 1 of a 3 part series. [Part 2](http://moam.github.io/2013/06/04/raspberry-pi-lamp-pt2.html) and 
[Part 3](http://moam.github.io/2013/06/04/raspberry-pi-lamp-pt3.html)