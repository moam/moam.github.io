---
layout: post
title: "Moving To GitHub And Jekyll"
---

If you've checked out my [About](http://moam.github.io/about.html) page you'll have seen a bit of the history surrounding this site. I case you haven't what are you 
waiting for?

Anyway I've flirted with various hosting solutions but lately I'd been using [Scriptogram](http://scriptogr.am/){:target="_blank"}. Coupled with 
[Dropbox](https://www.dropbox.com/){:target="_blank"} and [Markdown](http://daringfireball.net/projects/markdown/){:target="_blank"} it was very easy to use. **But** 
I'd noticed the distinct absence of any real activity on the site for a few months - their support pages have been down for a while - although I believe there was 
some active on Twitter (I don't use it myself - I just don't get Twitter, it's a load of bollocks). My site was still up and I could still access 
the admin. pages but I started to think about moving the site to something more active.

And up stepped [GitHub](https://github.com/){:target="_blank"} and [Jekyll](http://jekyllrb.com/){:target="_blank"}.

With a little bit of assistance (see links below) I was able to configure [Ruby](https://www.ruby-lang.org/en/){:target="_blank"} and Jekyll locally on my 
Windows 7 Laptop which made a massive difference when developing the site.

* [http://24ways.org/2013/get-started-with-github-pages/](http://24ways.org/2013/get-started-with-github-pages/){:target="_blank"}
* [http://yizeng.me/2013/05/10/setup-jekyll-on-windows/](http://yizeng.me/2013/05/10/setup-jekyll-on-windows/){:target="_blank"}
* [https://learn.andrewmunsell.com/learn/jekyll-by-example/introduction](https://learn.andrewmunsell.com/learn/jekyll-by-example/introduction){:target="_blank"}

However as with many Windows ports of Linux based configurations there were a few issues that cropped up. One in particular which wasn't covered in any of the 
above posts.

> C:/Ruby193/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require': cannot load such file -- wdm (LoadError)

The solution - <kbd>gem install wdm</kbd>

So far I am very impressed. I've tried to keep everything simple so no plugins and fancy [Liquid](http://liquidmarkup.org/){:target="_blank"} coding but that may 
come later.