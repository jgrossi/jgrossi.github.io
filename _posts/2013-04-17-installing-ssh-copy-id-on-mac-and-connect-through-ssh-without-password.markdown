---
layout: post
status: publish
published: true
title: Installing ssh-copy-id on MAC and connect through SSH without password

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  I have some servers and always I have to login using SSH, but some passwords is very complex. So, you must copy your public key to the desired server and be happy.

wordpress_id: 607
wordpress_url: http://juniorgrossi.com/?p=607
date: '2013-04-17 18:31:44 -0300'
date_gmt: '2013-04-17 18:31:44 -0300'
categories:
- Mac OSX
tags: []
---
<h2>Update September, 1st 2014</h2>
<p>The newer versions of port does not include the <code>ssh-copy-id</code> package anymore, but you can still use it cloning this repo from Github: <a href="https://github.com/beautifulcode/ssh-copy-id-for-OSX">https://github.com/beautifulcode/ssh-copy-id-for-OSX</a></p>

<p>Hello everybody! I have some servers and always I have to login using SSH, but some passwords is very complex. So, you must copy your public key to the desired server and be happy.</p>
<h2>Installing the command ssh-copy-id</h2>
<p>I used to use <strong>mac ports</strong> to install packages on my Mac, so: sudo port install ssh-copy-id Now you have the command installed on <em>/opt/local/bin/ssh-copy-id</em>. Just create a symbolic link to that: sudo ln -s /opt/local/bin/ssh-copy-id /usr/bin/ssh-copy-id</p>
<p><a id="more"></a><a id="more-607"></a></p>
<h2>Copying your public key to server</h2>
<p>Now you only have to run the command and be happy! The server will ask for password, and this will be the last time you type the code!</p>

{% highlight php startinline %}
ssh-copy-id -i ~/.ssh/id_rsa.pub username@example.com
This way you'll registering your public key for the "username" user at "example.com" server. 
{% endhighlight %}

<h2>Alternative way</h2>
<p>You can too copy your <em>id_rsa.pub</em> file content, login inside the server and append the file content to <em>/home/username/.ssh/authorized_keys</em>. The effect will be the same, but I think using <em>ssh-copy-id</em> is easier and quickly. Now when you try to login to server using ssh, like <em>ssh username@example.com</em> you'll have direct access! Thank you!</p>
<h1>Update Oct 9 2013</h1>
<p>An easy way to do that, without install nothing:</p>
<h2>Check if id_rsa.pub exists</h2>

{% highlight php startinline %}
cd ~/.ssh
ls
{% endhighlight %}

<p>If you found <strong>id_rsa.pub</strong> go to the next topic. If not do the following to create one public key:</p>

{% highlight php startinline %}
cd ~/.ssh 
ssh-keygen -t rsa -C "your_email@example.com" 
{% endhighlight %}

<ul>
<li>Creates a new ssh key, using the provided email as a label </li>
<li>Generating public/private rsa key pair. </li>
<li>Enter file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]</li>
</ul>
<p>After that:</p>

{% highlight php startinline %}
ssh-add id_rsa
{% endhighlight %}

<h2>Copying your public key to remote server</h2>

{% highlight php startinline %}
cat ~/.ssh/id_rsa.pub | ssh user@example.org 'cat >> /home/user/.ssh/authorized_keys'
{% endhighlight %}

<p>Done!</p>
