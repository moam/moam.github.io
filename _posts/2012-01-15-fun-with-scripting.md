---
layout: post
title: "Fun With Scripting"
---

##### SyntaxHighlighter Toolbar Toggle

I like the [SyntaxHighlighter](http://alexgorbatchev.com/SyntaxHighlighter/){:target="_blank"} by Alex Gorbatchev but I prefer the older version with the mini toolbar. It 
was frustrating that there seemed to be no option to hide it. However I have recently seen a couple of examples where the toolbar is revealed when a mouseover event 
occurs. That got me very excited so I did a bit of investigation and found the solution. It wasn't easy as the files that I needed to check were packed but I got there in 
the end.

The first file you need to edit is **shCore.css** and add in the following code

{% highlight css linenos %}
.syntaxhighlighter .bar
	{ display: none !important; }
.syntaxhighlighter .bar.show
	{ display: block !important; }
{% endhighlight %}	

The next step depends on your configuration. The second file you need to edit is **shCore.js**. However the version in {app path}\\scripts is packed and can't be edited. 
So I had to use the version in {app path}\\src then copy it to the scripts folder (since the script entry in my php file is syntaxhighlighter/scripts/shCore.js). You need 
to add the following code around line 1467

{% highlight js linenos %}
div.appendChild(this.bar); // this line exists
var bar = this.bar;
function hide()
	{
	bar.className = bar.className.replace("show", "")
	};
div.onmouseover = function ()
	{
	hide();
	bar.className += " show"
	};
div.onmouseout = function ()
	{
	hide()
	};
{% endhighlight %}

So there you go. The toolbar is now only shown on a mouseover event.

##### Microsoft Command-Based Script Host

A very useful switch to "cscript.exe" = **//T:nn** Time out in seconds:  Maximum time a script is permitted to run.

##### Where has WMIC come from?

I'm getting excited about WMIC. I've been using WMI for a while now in a couple of small apps I've written (in Visual Basic Express) but someone sent me an email about 
WMIC and I though "this is bloody good" so time for a few hours of investigation.

##### Email Link or Contact Me Form

Do I need to code a "Contact Me" form? I have the basics but can't be bothered to work on it plus no one contacts me anyway. So what's the solution - a simple 
"Contact Me" link. But what about SPAM? Well, use this site [The Enkoder Form](http://hivelogic.com/enkoder/){:target="_blank"} to create a scrambled Javascript link to 
your email address.