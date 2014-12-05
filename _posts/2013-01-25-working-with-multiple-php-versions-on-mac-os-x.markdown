---
layout: post
status: publish
published: true
title: Working with multiple PHP versions on MAC OS-X

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Today I had to update a project that was developed using Wordpress and PHP 5.3. Today I have PHP 5.4 installed on my machine and this newer version abandoned some old features, and you have some <strong>Fatal errors</strong> like <em>Call-time pass-by-reference has been removed</em>. The solution was go back to PHP 5.3 and do the updates on my the project, because the production server is with PHP 5.3 too.

wordpress_id: 534
wordpress_url: http://juniorgrossi.com/?p=534
date: '2013-01-25 14:43:53 -0200'
date_gmt: '2013-01-25 14:43:53 -0200'
categories:
- Apache
- PHP
tags: []
---
<p>Hello!</p>
<p>Today I had to update a project that was developed using Wordpress and PHP 5.3. Today I have PHP 5.4 installed on my machine and this newer version abandoned some old features, and you have some <strong>Fatal errors</strong> like <em>Call-time pass-by-reference has been removed</em>. The solution was go back to PHP 5.3 and do the updates on my the project, because the production server is with PHP 5.3 too.</p>
<p>The cleaner solution is using <a href="http://www.vagrantup.com/">Vagrant</a>, that allow you install only what you want for a specific project, leaving your machine cleaner. But was not my case at the moment.</p>
<h2>Macports</h2>
<p>I've used <strong>macports</strong> to install PHP in my machine. I think <em>macports</em> is a very good way to organize all sources in the same place. More information you can find <a href="http://www.macports.org/">here</a>.</p>
<p>You can install PHP 5.3 and 5.4 in the same machine and decide what you'll use just updating your <em>Apache httpd.conf</em> file.</p>
<p><a id="more"></a><a id="more-534"></a></p>
<h2>Installing PHP versions</h2>
<p>First let's install PHP 5.4 using <em>macports</em>:</p>

{% highlight php startinline %}
sudo port install php54
{% endhighlight %}

<p>You can search for packages. Let's search for PHP 5.4 Mysql driver:</p>

{% highlight php startinline %}
port search php54-mysql*
{% endhighlight %}

<p>And you'll get:</p>

{% highlight php startinline %}
php54-mysql @5.4.9 (lang, php, www, databases)
    a PHP interface to MySQL databases, including the mysql, mysqli and pdo_mysql extensions
{% endhighlight %}

<p>So now you can install PHP 5.4 MySQL driver:</p>

{% highlight php startinline %}
sudo port install php54-mysql
{% endhighlight %}

<p>Well, this way you can install what you want, like extensions and drivers, like <em>pdo</em>, etc. For example, you must install the <em>apache2 handler</em> too. So:</p>

{% highlight php startinline %}
port search php54-apache*
php54-apache2handler @5.4.9 (lang, php, www)
    php54 Apache 2 Handler SAPI

sudo port install php54-apache2handler
{% endhighlight %}

<p>Now you have PHP 5.4 installed. The same way you will install PHP 5.3, for example, like:</p>

{% highlight php startinline %}
sudo port install php53
sudo port install php53-mysql
sudo port install php53-apache2handler
// and more
{% endhighlight %}

<h2>Updating Apache configuration file</h2>
<p>You can install <em>Apache</em> by <em>macports</em> too. Just search for it and install. Now, I'm using the original <em>Apache2</em> version that comes with the <em>MAC OS-X Mountain Lion</em>. So my <em>httpd.conf</em> (apache file configuration) is located inside <em>/etc/apache2/httpd.conf</em>.</p>
<p>Open this file and make some changes:</p>

{% highlight php startinline %}
sudo nano /etc/apache2/httpd.conf
{% endhighlight %}

<p>Now you have your <em>Apache2</em> configuration file for editing. What you must do is change de <strong>php5_module</strong> file to point to the version you want. For default <em>macports</em> installs everything under <em>/opt/local</em> directory. So you can find the <strong>php5_module</strong> file under <em>/opt/local/apache2/modules/</em>. There you have <strong>mod_php54</strong> and <strong>mod_php53</strong>.</p>
<h3>Changing PHP versions</h3>
<p>If you want to use now PHP 5.3 you just have to change the <em>httpd.conf</em> file line to:</p>

{% highlight php startinline %}
LoadModule php5_module /opt/local/apache2/modules/mod_php53.so
{% endhighlight %}

<p>Or if you want PHP 5.4:</p>

{% highlight php startinline %}
LoadModule php5_module /opt/local/apache2/modules/mod_php54.so
{% endhighlight %}

<h2>PHP.ini configuration file</h2>
<p>By default <em>macports</em> store <em>php.ini</em> files under <em>/opt/local/etc/php54/</em>. There you can find 2 <em>.ini</em> files:</p>

{% highlight php startinline %}
cd /opt/local/etc/php54/
ls 
php.ini-development    php.ini-production
{% endhighlight %}

<p>Just rename the development file to <em>php.ini</em> to be your default PHP 5.4 ini file.</p>

{% highlight php startinline %}
sudo mv php.ini-development php.ini
{% endhighlight %}

<p>And restart Apache:</p>

{% highlight php startinline %}
sudo apachectl restart
{% endhighlight %}

<p>Now you can use the version you selected in <em>httpd.conf</em> file.</p>
<h2>Using PHP on Command Line (CLI)</h2>
<p>You can change the php command line version too. By default <em>macports</em> installs the <em>bin</em> files under <em>cd /opt/local/bin/</em>. What you must do is create a <em>symbolic link</em> to the PHP version you want. So if you want to use php command line 5.3 you run:</p>

{% highlight php startinline %}
sudo rm /usr/bin/php // remove /usr/bin/php first
sudo ln -s /opt/local/bin/php53 /usr/bin/php // pointing to php53
php -v // get version
PHP 5.3.19 (cli) (built: Nov 26 2012 12:44:33) 
Copyright (c) 1997-2012 The PHP Group
{% endhighlight %}

<p>Or using PHP 5.4:</p>

{% highlight php startinline %}
sudo rm /usr/bin/php // remove /usr/bin/php first
sudo ln -s /opt/local/bin/php54 /usr/bin/php // pointing to php54
php -v // get version
PHP 5.4.9 (cli) (built: Nov 26 2012 12:40:37) 
Copyright (c) 1997-2012 The PHP Group
{% endhighlight %}

<p>That it! I hope help you! Thanks for reading.</p>
