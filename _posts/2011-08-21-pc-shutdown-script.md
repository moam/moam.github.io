---
layout: post
title: "PC Shutdown Script"
---

The task was simple enough; write a script that would enable the remote shutdown of multiple workstations. The basic script should consist of:

* Read in the workstation name from a file
* PING the workstation, if it replies carry on to next step, if not then move to next workstation
* Send the "shutdown" command

{% highlight vbnet linenos %}
Set objShell = CreateObject("WScript.Shell")
Set objExec = objShell.Exec("ping -n 1 -w 1000 " & strWSNAME)
strPCON = LCase(objExec.StdOut.ReadAll)
If InStr(strPCON, "reply from") Then ...
	Shut the machine down (see below for the code)
Else ...
	The machine is switched off (or at least no reply was received)
End If
{% endhighlight %}

Here we can see that the script sets up the shell object, then we issue the PING command against the workstation. Next we capture the output (StdOut.ReadAll) 
into a variable and check what it says. If it says "reply from" then we assume that it replied and we can shut it down. Anything else we assume it's switched off 
and move onto the next workstation.

{% highlight vbnet linenos %}
Set objExec = objShell.Exec("shutdown -m \\" & strWSNAME & " -s -f -t 60")
strSHUTDOWNMESSAGE = LCase(objExec.StdOut.ReadAll)
strSHUTDOWNERROR = LCase(objExec.StdErr.ReadAll)
If Len(strSHUTDOWNMESSAGE) > 0  Or Len(strSHUTDOWNERROR) > 0 Then ...
	The was an error (or informative message) and it failed
Else ...
	The request was sent
End If
{% endhighlight %}

We issue the SHUTDOWN command against the workstation. Next we capture the output (StdOut.ReadAll & StdErr.ReadAll) into variables and check what they say. The 
command has no success output so if these variables are blank then we assume the command has been sent successfully and has worked. Otherwise there was a 
problem and it failed.

Alternatively if you'd like to warn an end user (should there be a possibility that they'll still be logged in) that their workstation is about to be shutdown 
you could use this version. It relies on you either training the end user to issue the cancellation command (shutdown -a) or adding a shortcut (to a batch file 
with the same command) on the user's desktop.

{% highlight vbnet linenos %}
Set objExec = objShell.Exec("shutdown -m \\" & strWSNAME & " -s -f -t 60 -c ""Warning! This machine will now be shutdown. You can cancel this by using the shortcut on your desktop""")
{% endhighlight %}

The only 2 errors I have seen output are

* Access Denied - I believe this is due to the account running the script not having the necessary privileges to issue the command on the workstation but this 
is not confirmed
* Network Path Not Found - I believe that this is due to file and print sharing not being enabled on the target workstation but once again this is not 
confirmed

Because there is no success output from the "shutdown command" you may have to carry out further activities to check that it has actually worked. Some solutions 
I have seen run a second script to PING all workstations once again but this isn't a requirement for my script so I haven't implemented it.

To finish off here are a couple of links to help you out.

* [Shutdown Command Reference](http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/shutdown.mspx?mfr=true){:target="_blank"}
* [The Script](/docs/autoshutdown.txt){:target="_blank"} (to avoid any security 
issues etc. I have saved it as a .txt file. Please change extension to .vbs for correct usage).