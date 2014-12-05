---
layout: post
status: publish
published: true
title: Configuring a VPS from scratch at Rackspace

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Last monday I signup a new VPS (Virtual Private Server) at Rackspace. Let's go! Why do I need a VPS? Why not a shared host company? You don't need, but it's better than a shared host. Using a shared host (usually use cPanel/WHM) you have to follow the company's rules. If you want to try some framework, for example, that uses PHP 5.3 and your shared host has PHP 5.2, you are in a dead end. Shared hosts don't allow you change that. Using a VPS you have all control about the server.

wordpress_id: 316
wordpress_url: http://www.juniorgrossi.com/?p=316
date: '2012-01-04 17:46:38 -0200'
date_gmt: '2012-01-04 20:46:38 -0200'
categories:
- Hosting
tags: []
---
<p>Hello everyone...</p>
<p>Last monday I signup a new VPS (Virtual Private Server) at Rackspace. Let's go!</p>
<p><strong>Why do I need a VPS? Why not a shared host company?</strong></p>
<p>You don't need, but it's better than a shared host. Using a shared host (usually use cPanel/WHM) you have to follow the company's rules. If you want to try some framework, for example, that uses PHP 5.3 and your shared host has PHP 5.2, you are in a dead end. Shared hosts don't allow you change that. Using a VPS you have all control about the server.</p>
<p><a id="more"></a><a id="more-316"></a></p>
<h3>Getting a VPS</h3>
<p>I like the Rackspace service. They have a very fast support (live chat 24/7/365) and the host service is very good. I recommend them! But, with these steps I should you can get any VPS you want.</p>
<p>Choose the VPS you want (at Rackspace): <a href="http://www.rackspace.com/cloud/cloud_hosting_products/servers/pricing/">http://www.rackspace.com/cloud/cloud&#95;hosting&#95;products/servers/pricing/</a></p>
<p>Get the VPS and login the Cloud Controle Panel: <a href="https://manage.rackspacecloud.com">https://manage.rackspacecloud.com</a></p>
<ol>
<li>You'll se your main menu in the left side. Let's access: Hosting >> Cloud Servers.</li>
<li>Here you'll se all servers you have at Rackspace. Click "Add Server" to create one</li>
<li>Select the Operational System you want. My choose was Ubuntu 11.10 (Oneiric Ocelot)</li>
<li>Give the server a name, like &#42;<em>&#42;|&#42;server.mydomain.com&#42;</em>&#42;|&#42;, and select the amount of ram and disk you want</li>
<li>Wait for the server creation. After 1, 2 minutes you'll receive an e-mail with your IP and password</li>
</ol>
<p>Let's access your new VPS using <strong>SSH</strong> and type your password (mailed by Rackspace):</p>

{% highlight php startinline %}
ssh root@12.34.567.89
root@12.34.567.89's password: <type your password>
{% endhighlight %}

<h3>Initial configuration</h3>
<p>In my case, I use LAMP with Zend Framework. So let's install PHP, MySQL and Apache.</p>

{% highlight php startinline %}
apt-get update // to refresh your apt repositories
apt-get install apache2
apt-get install mysql-server
apt-get install php5
apt-get install phpmyadmin
apt-get install zend-framework
apt-get install proftpd // access by FTP
{% endhighlight %}

<h3>Webmin - Control Panel</h3>
<p>For Control Panel I prefer <a href="http://www.webmin.com" title="Webmin">Webmin</a>. It's free and VERY VERY good...</p>
<p>Follow the steps at Webmin site: <a href="http://webmin.com/deb.html">http://webmin.com/deb.html</a></p>
<p>Edit the <strong>/etc/apt/sources.list</strong> file and add these lines in the end:</p>

{% highlight php startinline %}
deb http://download.webmin.com/download/repository sarge contrib
deb http://webmin.mirror.somersettechsolutions.co.uk/repository sarge contrib
{% endhighlight %}

<p>Save the file. In the shell get the GPG key...</p>

{% highlight php startinline %}
cd /root
wget http://www.webmin.com/jcameron-key.asc
apt-key add jcameron-key.asc
{% endhighlight %}

<p>Give APT a refresh and install Webmin throw apt-get:</p>

{% highlight php startinline %}
apt-get update
apt-get install webmin
{% endhighlight %}

<h3>Postfix - Sending e-mails</h3>
<p>You'll need a mail server to send e-mails using your applications. Let's install <strong>Postfix</strong>. It's better than <strong>sendmail</strong> (my opinion).</p>

{% highlight php startinline %}
apt-get install postfix
{% endhighlight %}

<p>Follow the step in the <a href="http://www.artigonal.com/programacao-artigos/montagem-e-configuracao-do-servidor-de-e-mail-postfix-2047471.html">Daiana Bueno post</a> (in portugues, but you'll understand) to configure your Postfix server using Ubuntu.</p>
<h3>Your first domain hosting</h3>
<p>Let's host the <strong>mycompany.com</strong> domain.</p>
<p>In your Rackspace Cloud Panel, go to <strong>Hosting >> Cloud Servers >> server.mydomain.com</strong>. On the top of page click <strong>"DNS"</strong> and under <strong>"Domain Management"</strong> click <strong>"Add"</strong>. You'll add a new domain.</p>
<p>Fill with <strong>mycompany.com</strong> and click OK. Your domain will appear in domain grid. Click the domain <strong>mycompany.com</strong> and you'll se the DNS records of your domain.</p>
<p><em>I strongly recommend you to use the Rackspace's DNS records. I tried to install my own DNS Server (Bind) and had a lot of problems. It's not a simple configuration and you have a lot of steps that does not matter.</em></p>
<p>First, let's create basics DNS records. Let's tell the Rackspace DNS server that the mycompany.com domain is hosted by your IP address.</p>
<ul>
<li>Type: A</li>
<li>Name: mycompany.com</li>
<li>Content (IP address): 12.34.567.89</li>
<li>TTL (Seconds): 300</li>
</ul>
<p>After, to ensure access using www.mycompany.com let's create a CNAME record.</p>
<ul>
<li>Type: CNAME</li>
<li>Name: www.mycompany.com</li>
<li>Content: mycompany.com</li>
<li>TTL: 300</li>
</ul>
<h3>Creating the user, uploading a sample website file and creating a Apache Virtual Host</h3>
<p><strong>Creating a new user</strong></p>
<p>Open for the first time your Webmin app. Type <strong>https://12.34.567.89:10000</strong> in your browser. Fill username as root and your root user password.</p>
<p>On left side click <strong>System >> Users and Groups</strong>. Click <strong>Create a new user</strong>. <em>PS: Creating a new user we'll create automatically a FTP account with the username and password provided.</em></p>
<p>Fill these fields:</p>
<ul>
<li>Username: mycompany</li>
<li>User ID: Automatic</li>
<li>Real Name: My Company</li>
<li>Home directory: Automatic (Webmin will create the /home/mycompany path for you)</li>
<li>Shell: /bin/sh</li>
<li>Password: Normal password <type your password></li>
<li>... let next fields as default</li>
<li>On "Group Membership" fieldset click "New group with same name as user" (user mycompany and group mycompany)</li>
<li>... next fields are ok...</li>
<li>Click "Create" button</li>
</ul>
<p>Now we have the <strong>/home/mycompany</strong> directory. Let's create a <strong>public_html</strong> directory inside it (will be our public directory, with website contents).</p>

{% highlight php startinline %}
cd /home/mycompany
mkdir public_html
{% endhighlight %}

<p><strong>A test file</strong></p>
<p>Now create (or upload using FTP) a <strong>index.php</strong> file with this content:</p>

{% highlight php startinline %}
<?php echo 'mycompany.com is working'; ?>
{% endhighlight %}

<p><strong>Creating a Virtual Host in Apache</strong></p>
<p>Now we know that Rackspace's DNS servers knows the domain mycompany.com is hosted in our IP address (at Rackspace's Panel) and the website (index.php) is ok too. So, you can have a lot of domains in the same server. For that you must use Apache's Virtual Host, that tell where is the files for THAT domain you want. We can get something like:</p>
<p>**mycompany.com >> **/home/mycompay/public_html</p>
<p>**yourcompany.com >> **/home/yourcompany/public_html</p>
<p>Let's create a Virtual Host.</p>
<p>Using WEBMIN, click <strong>Servers >> Apache Webserver</strong>. Click now <strong>Create virtual host</strong>. Fill only the fields above (for others fields use the default option):</p>
<ul>
<li>Document Root: /home/mycompany/public_html</li>
<li>Server Name: Other, type mycompany.com</li>
<li>Click "Create Now"</li>
</ul>
<p>Now click on the <strong>Virtual Server</strong> link you created to open it's configurations. Click <strong>Networking and Addresses</strong> and fill the <strong>Alternate virtual server names</strong> field with <strong>www.mycompany.com</strong>. This will allow people to access your site using both <em>mycompany.com</em> and <em>www.mycompany.com</em>.</p>
<p>Click <strong>Apply Changes</strong> (top/right) to restart the Apache server and apply our new configurations.</p>
<p>Ok! It's all configured. Now update your DNS record at your registrar (like GoDaddy, Enom, etc) and wait for the DNS propagation. Rackspace uses these DNS domains: <strong>dns1.stabletransit.com</strong> and <strong>dns2.stabletransit.com</strong>.</p>
<p>Remember! Using a VPS you can install and manage what you want. You can test frameworks, databases, etc. I suggest you to browse <strong>Webmin</strong>. It has a lot of usefull options like database creation, PHP configuration, Apache configuration, and what you want. It's a very powerfull tool.</p>
<p>Enjoy! Thanks!</p>
