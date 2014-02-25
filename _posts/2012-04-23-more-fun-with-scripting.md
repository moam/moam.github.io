---
layout: post
title: "More Fun With Scripting"
---

##### Using WMIC To Get A List Of Installed Applications

I was asked by a colleague if I knew how to obtain a list of installed applications on a remote workstation without having physical access to the machine. I thought I'd 
give WMIC a test drive as it looked the simplest method.

After some investigation I came up with a few options which are shown below. All these commands are run from the CMD prompt. Unfortunately the output only shows you the 
applications that have been installed via an MSI file. Anything installed via say a EXE won't be displayed. For that you need the registry.

{% highlight vbnet linenos %}
/* installed apps on the local machine */
wmic /output:software_%COMPUTERNAME%.htm product get Name, Version /format:htable.xsl

/* installed apps on a single remote machine */
wmic /node:PCNAME /output:software_PCNAME.htm product get Name, Version /format:htable.xsl

/* installed apps on multiple remote machines */
wmic /node:@workstation.txt /output:software.htm product get Name, Version /format:htable.xsl
{% endhighlight %}

In the examples above the output is to an HTML file using the /format switch and the htable.xls (style sheet). There are other options such as csv and xml. In the last 
option the file workstation.txt is a file containing the list of workstations you want to target, either comma separated or 1 per line.

There is a BUG in Windows 7 which may result in you getting an error about not being able to find the XLS file. To correct this you need to add this to the PATH 
variable - **C:\Windows\System32\wbem\en-US** - to allow WMIC to find the XLS files. In Windows XP they're in the **C:\Windows\System32** folder and there should be no 
errors. One other point to note, especially with regards to example 3, is that this method is very slow.

##### Running CMD As Administrator

I found this very small but extremely useful bit of information to allow you to run the CMD prompt as administrator - **runas /user:DOMAINUSERID@DOMAIN cmd** - this has 
saved me on more than one occasion.

##### PHP session_start() Warning Fixed

After a PHP upgrade my site was giving the following error

> Warning: session_start() [function.session-start]: SAFE MODE Restriction in effect. The script whose uid is 795 is not allowed to access /tmp owned by uid 0 in 
/home/foo/bar/index.php on line 9<br />Fatal error: session_start() [function.session-start]: Failed to initialize storage module: files (path: ) in /home/foo/bar/index.php on line 9

I logged a support ticket with my Host however I was able to fix it by adding the following to my custom **php.ini** file

{% highlight vbnet linenos %}
;
; Sessions
session.save_path = "/tmp"
;
{% endhighlight %}