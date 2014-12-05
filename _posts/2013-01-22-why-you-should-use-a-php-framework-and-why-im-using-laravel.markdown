---
layout: post
status: publish
published: true
title: Why you should use a PHP Framework and why I'm using Laravel

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Use or not to use a PHP Framework on your new projects? Here I'll talk about my personal opinion about that and I wish help you. First, I have a simple concept about languages and respectives frameworks: a framework must make the development simpler. Every software has a single purpose: work! This is the main goal of every project, do that to simplify the life of someone.
  
wordpress_id: 513
wordpress_url: http://juniorgrossi.com/?p=513
date: '2013-01-22 13:22:44 -0200'
date_gmt: '2013-01-22 13:22:44 -0200'
categories:
- PHP
- Laravel
- Composer
tags: []
---
<p>Hello everyone!</p>
<p>Use or not to use a PHP Framework on your new projects? Here I'll talk about my personal opinion about that and I wish help you.</p>
<p>First, I have a simple concept about languages and respectives frameworks: a framework must make the development simpler. Every software has a single purpose: work! This is the main goal of every project, do that to simplify the life of someone.</p>
<p>Talking about PHP, it is a very simple language. It was projected to be simple and fast. If you want a language complicated or filled of dependencies, PHP is not your best choice.</p>
<p>PHP has the power to do a lot of things in a single line, so, if your framework of choice do the same thing in 3 or 4 lines, use the native PHP way. But you'll ask me your framework do the same job but much better, using OO, or with beautiful code. Remember you must do the thing and make your project work. Of course the OO programming is the recommended way and you must use it, to make you like easier. PHP is moving everything to OO way and soon the <strong>-></strong> will be replace with <strong>.</strong> and everything will be OO, like:</p>

{% highlight php startinline %}
String.length()
{% endhighlight %}

<p>What I want to say is you should use the simpler way, to make the development faster and better. Choose a framework that help you to do the job faster, and use PHP for that. Exists some frameworks that you must learn another language almost. For me a framework must be easy to learn and with minutes you will be developing using it and make money.</p>
<p><a id="more"></a><a id="more-513"></a></p>
<p>I have some experience in <a href="http://rubyonrails.org">Ruby on Rails</a> and the reason a lot of PHP programmers moved to RoR is because it is simple, just that. You write code as you talk, but you can do the same thing using PHP, hitting the right framework for you. If you feel better using architetures like Java, take a look at <a href="http://framework.zend.com">Zend Framework</a>. If you want something newer, 100% OO but easier than <a href="http://framework.zend.com">Zend</a>, use <a href="http://symfony.com">Symfony 2</a>. If you are a Rails fan and want something more directly you can go with <a href="http://laravel.com">Laravel Framework</a> or <a href="http://cakephp.org">CakePHP</a>.</p>
<p>I really like the Ruby on Rails way, this is why I use <a href="http://laravel.com">Laravel</a> for developing. It is a very good framework where you can do what you want. You can learn it fast and just reading the docs. You can go with Laravel creating a simple API or developing a complex application.</p>
<p>Why you must create 3 or 4 classes to get only your active users? It must be simple, like Laravel and its Eloquent ORM:</p>

{% highlight php startinline %}
User::where('status', '=', 'active')->get(); // all active users
User::find(2)->status; // get the status of user_id 2
{% endhighlight %}

<p>You only have to read the code and you know what it's doing. The code tell you everything you want, like talking. You want to render a <em>view</em> or use it as a <em>mail</em> message body:</p>

{% highlight php startinline %}
$viewContent = View::make('users.welcome')->with('name', 'Grossi'); // just that
{% endhighlight %}

<p>The new Laravel 4 is <em>composer based</em>, as Zend Framework 2 and Symfony 2 are. So, if you want use its components you can use <a href="http://getcomposer.org">composer</a> for that. I use a lot of ZF components inside Laravel and they work well.</p>
<p>Resuming, you should use a PHP Framework to improve your development, but if you can do the same thing in pure PHP, do that, because you're developing with PHP and not the framework language.</p>
<p>I'm a Zend Framework developer too and every project is different. But, take a look in <a href="http://laravel.com">Laravel</a> framework, and give it a try. It is a very good project developed by a fantastic team and with some very known developers like <a href="http://twitter.com/taylorotwell">Taylor Otwell</a> and <a href="http://twitter.com/daylerees">Dayle Rees</a>.</p>
<p>I wish help you. Thanks.</p>
