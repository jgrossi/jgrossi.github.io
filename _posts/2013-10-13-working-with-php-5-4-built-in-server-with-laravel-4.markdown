---
layout: post
status: publish
published: true
title: Working with PHP 5.4+ built-in server with Laravel 4
excerpt: |+
  One of the best improvements of the PHP 5.4 was the built-in web server. Like in Ruby On Rails, now you do not need Apache or Nginx inside your development machine.
author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
wordpress_id: 756
wordpress_url: http://juniorgrossi.com/?p=756
date: '2013-10-13 23:57:45 -0300'
date_gmt: '2013-10-13 23:57:45 -0300'
categories:
- PHP
- Laravel
- Laravel 4
tags: []
---
<p>One of the best improvements of the PHP 5.4 was the built-in web server. Like in Ruby On Rails, now you do not need Apache or Nginx inside your development machine.</p>
<p>To start a web server is easy:</p>

{% highlight php startinline %}
php -S localhost:8000
{% endhighlight %}

<p>Remember you can choose the port you want. You can use <code>localhost:8000</code> or <code>localhost:8080</code> or what you want.</p>
<p>If you want to start the server for a Laravel 4 app your must set the public dir:</p>

{% highlight php startinline %}
php -S localhost:8000 -t public/
{% endhighlight %}

<p>Ok. This works well. But you can do better than this.</p>
<p>When you use <code>-t public/</code> you are telling the PHP server to start it inside the <code>public</code> dir, more specifically the <code>public/index.php</code> file. Ok, but with Laravel 4 this can bring you some problems of routes. In 99% of the cases this will work well but if you have a route like <code>http://example.org/some/file.html</code> for example, this will not work.</p>
<p>The way Laravel 4 work with the web server is a little different. So, because of that, the Laravel 4 team created a custom <code>server.php</code> file. So you have to start the server with this file:</p>

{% highlight php startinline %}
// inside the Laravel base dir (not public)
php -S localhost:8000 server.php
{% endhighlight %}

<p>Laravel 4 does the rest for you. Now everything will work and you won't have problem with routes. Continue to develop your app!</p>
