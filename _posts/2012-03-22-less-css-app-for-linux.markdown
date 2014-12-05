---
layout: post
status: publish
published: true
title: LESS CSS App for Linux

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Here I am again. Today I will talk about a LESS CSS App for Linux OS. I use both Mac and Linux machines. Using Mac we have the excellent app http://incident57.com/less/. For Linux, we have a executable file (SH commands) that wait for file update and run the <strong>lessc</strong> command to compile the less file.

wordpress_id: 346
wordpress_url: http://www.juniorgrossi.com/?p=346
date: '2012-03-22 18:55:52 -0300'
date_gmt: '2012-03-22 21:55:52 -0300'
categories:
- General
tags: []
---
<p>Hello!</p>
<p>Here I am again. Today I will talk about a LESS CSS App for Linux OS.</p>
<p>I use both Mac and Linux machines. Using Mac we have the excellent app http://incident57.com/less/. For Linux, we have a executable file (SH commands) that wait for file update and run the <strong>lessc</strong> command to compile the less file.</p>
<p>The Linux file located in this link. Just download it. http://code.krml.fr/less.app</p>
<p>This file uses the <strong>inotify-tools</strong> for Linux. So you have to install it too. Now I am using Fedora 16, so just type <strong><em>as root</em></strong> &#42;<em>&#42;|&#42;yum install inotify-tools&#42;</em>&#42;|&#42; and Yum will install it. Ubuntu users can use <strong>apt-get</strong> for that too.</p>
<p><a id="more"></a><a id="more-346"></a></p>

{% highlight php startinline %}
yum install inotify-tools  
{% endhighlight %}

<p>I suggest you create a symbolic link to this file. You can create a dir like /usr/share/lessapp and put the file inside it. So create a symbolic link to your bin folder:</p>

{% highlight php startinline %}
ln -s /usr/share/lessapp/less.app /usr/bin/lessapp  
{% endhighlight %}

<p>Easy! So now you can run the LESS App with all your projects. For example, in the your project root path:</p>

{% highlight php startinline %}
lessap mylessfolder mycssfolder  
{% endhighlight %}

<p>The script will compile all less files inside the less folder to css files inside css folder.</p>
<p>I hope help!</p>
