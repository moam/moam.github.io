---
layout: post
title: "Installing Jekyll on Windows 7 Pro"
---

This is a combination of several articles so I can't claim that this is all my own work. For example ...

* [get-started-with-github-pages](http://24ways.org/2013/get-started-with-github-pages/){:target="_blank"}
* [setup-jekyll-on-windows](http://yizeng.me/2013/05/10/setup-jekyll-on-windows/){:target="_blank"}
* [jekyll-by-example](https://learn.andrewmunsell.com/learn/jekyll-by-example/introduction){:target="_blank"}

So let's begin ...

##### Install Ruby

* Go to [http://rubyinstaller.org/downloads/](http://rubyinstaller.org/downloads/){:target="_blank"}
* I chose **rubyinstaller-1.9.3-p545.exe** which works fine on my system
* Once downloaded install it. Try to keep to the default directory which in this case is (**C:\Ruby193**) and make sure you tick **Add Ruby executables to your PATH** checkbox so the PATH environment variable will be updated automatically
* Open up a command prompt window and test if the installation has succeeded or not by typing in the following <kbd>ruby -\-version</kbd>

##### Install the Ruby DevKit

* Go to [http://rubyinstaller.org/downloads/](http://rubyinstaller.org/downloads/){:target="_blank"}
* Download the installer that matches the Windows architecture and the Ruby version just installed. For instance Ruby versions **1.8.6 to 1.9.3** should use the file **DevKit-tdm-32-4.5.2-20111229-1559-sfx.exe**
* Run the installer and extract it to a folder e.g. **C:\RubyDevKit**
* To initialize and create the **config.yml** file open a command prompt window and type in the following
    * <kbd>cd C:\RubyDevKit</kbd>
	* <kbd>ruby dk.rb init</kbd>
	* <kbd>notepad config.yml</kbd>
* In the opened notepad window add a new line **- C:/Ruby193** at the end if it isn't already there, then save and close.
* Back to the command prompt type in the following to review (optional) and install
	* <kbd>ruby dk.rb review</kbd>
	* <kbd>ruby dk.rb install</kbd>

##### Install Jekyll

At the time of writing a default install will give you version 1.4.3 of Jekyll however Windows 7 Pro doesn't seem to like that version so we need to drop back to version 1.4.2.

* Open a command prompt window and type in the following
	* <kbd>gem --version</kbd> (optional to verify that gem has been installed properly)
	* <kbd>gem install jekyll -\-version "=1.4.2"</kbd>

##### Update Ruby Pygments Gem

If you don't plan to show any code blocks on your site then you can skip this step. By now you should have Pygments version 0.5.4 installed (check in **C:\Ruby193\lib\ruby\gems\1.9.1\gems**) however once again Windows 7 doesn't seem to like this version so we must first uninstall version 0.5.4 then install version 0.5.0

* Open a command prompt window and type in the following
	* <kbd>gem uninstall pygments.rb -\-version "=0.5.4"</kbd>
	* <kbd>gem install pygments.rb -\-version "=0.5.0"</kbd>

##### Install Python

* Go to [http://www.python.org/download/](http://www.python.org/download/){:target="_blank"}
* Download appropriate version of Python windows installer e.g. I chose Python 2.7.6 Windows Installer (**python-2.7.6.msi**)
* Install accepting all the defaults
* Add the installation directory (e.g. **C:\Python27**) to the PATH environment variable. You should also add **C:\Python27\Scripts** which is used later.
* Open up a new command prompt window (this will avoid you having to reboot the PC so the PATH changes can be implemented) and test if the installation has succeeded or not by typing in the following <kbd>python -\-version</kbd>

##### Install Python 'Easy Install'

* Go to [https://pypi.python.org/pypi/setuptools#windows-7-or-graphical-install](https://pypi.python.org/pypi/setuptools#windows-7-or-graphical-install){:target="_blank"}
* Download **ez_setup.py** as instructed and place it in the **C:\Python27** folder
* Using the command prompt window you open before type in the following
	* <kbd>cd C:\Python27</kbd>
	* <kbd>ez_setup.py</kbd>
* Once installed add the 'Python Scripts' directory (**C:\Python27\Scripts**) to the PATH environment variable if you didn't do it beforehand

##### Install Python Pygments

* Verify easy\_install is installed properly <kbd>easy\_install -\-help</kbd>
* Install Pygments using easy\_install <kbd>easy\_install Pygments</kbd>

##### Extras

The second link I posted at the top of this article has a few troubleshooting tips. The only error I received wasn't among them but with the power of Google I was able to find a fix.

> C:/Ruby193/lib/ruby/site\_ruby/1.9.1/rubygems/custom\_require.rb:36:in `require':
cannot load such file -- wdm (LoadError)

To fix this issue open a command prompt and type in the following <kbd>gem install wdm</kbd>

The only other option I installed was the use of Kramdown. This is entirely optional but if you want to install it open a command prompt and type in the following <kbd>gem install kramdown</kbd>

##### Testing

OK if you've followed the instructions above hopefully you'll have a fully working environment in which to run a Jekyll site locally which you can use for development / testing prior to uploading to your GitHub site. In my case I'd do the following

* Open Windows Explorer and find the folder containing my site
* Hold down the SHIFT key while clicking the right mouse button then select "Open command prompt here"
* At the command prompt type <kbd>jekyll serve -\-watch</kbd> (the **-\-watch** switch is optional but is useful when making changes as it rebuilds your site on the fly)
* If everything is alright then you should get something similar to this

![Jekyll Command Prompt](/image/jekyll.jpg "Jekyll Command Prompt")

Now open up your browser of choice and enter **http://localhost:4000** into the address bar and you should see your site.



