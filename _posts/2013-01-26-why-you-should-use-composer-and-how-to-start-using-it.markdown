---
layout: post
status: publish
published: true
title: Why you should use Composer and how to start using it

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  One of the most big changes happening in PHP world is the <a href="http://getcomposer.org">Composer</a>. I'm shure you heard about it but maybe you don't know why you should use it and how much it is good for you and your projects.

wordpress_id: 546
wordpress_url: http://juniorgrossi.com/?p=546
date: '2013-01-26 09:25:16 -0200'
date_gmt: '2013-01-26 09:25:16 -0200'
categories:
- PHP
- Laravel
- Composer
tags: []
---
<p>Hello!</p>
<p>One of the most big changes happening in PHP world is the <a href="http://getcomposer.org">Composer</a>. I'm shure you heard about it but maybe you don't know why you should use it and how much it is good for you and your projects.</p>
<h2>What is Composer?</h2>
<p>When you need some specific code in PHP you can go to <a href="http://phpclasses.org">PHP Classes</a> and search for what you need. For example, you need a class to connect to your Twitter account and get the last updates in your timeline. Ok, you'll find it easy. And what you have to do after? You download the classe and copy to some place inside your project, and call it <em>including</em> or <em>requiring</em> it inside your PHP code, right?</p>
<p>Well, you can use <a href="http://pear.php.net">Pear</a> to. You must install it on your server and install some components that you can use like a package manager. This work too but nothing is installed inside your project structure, but in your machine.</p>
<p>Ok. <em>Composer</em> came to solve all this questions. Together with <em>Composer</em> we have some rules, making all PHP project with the same structure, using <em>namespaces</em>, the same code style, etc. This simplifies when you have to include some third-party code or project. This rules you can find at <a href="http://www.php-fig.org/">PHP Fig (PHP Framework Interop Group)</a>. I really suggest you to take a look at <a href="http://phptherightway.com">phptherightway.com</a>. There you'll find a lot of information to made you a good PHP developer using what we have of better in PHP world.</p>
<p><a id="more"></a><a id="more-546"></a></p>
<p>Let's take a example. You created a new project using the framework <a href="http://laravel.com">Laravel 4</a> and you have to use the <a href="http://delicious.com">Amazon</a> API. But <a href="http://framework.zend.com">Zend Framework</a> has done that for you. They have classes that do the job, and you don't need to do everything again, just reuse the code that is already developed. You can use its packages to include the Zend Framework's Amazon API inside your project.</p>
<p>OK! But they are different frameworks! You're right, but both Laravel and Zend Framework support <em>Composer</em> and you can use it to include sources inside your project. So it's right! You can add <strong>only</strong> <em>ZendService\Amazon</em> inside your Laravel project, because Zend Framework was developed as many projects inside one, and all them are developed using Composer too. Autoload will be already configured for you, so, you don't have to include files, nothing, just call them and PHP take care of the rest.</p>
<h2>Comparative</h2>
<p><em>Composer</em> works like <a href="http://rubygems.org">Ruby Gems</a>. You have a program that runs in the command line and you can install what you want. You tell <em>Composer</em> to install <em>ZendService\Amazon</em> and it will copy and configure it for you inside your <em>vendor</em> directory. Easy like that.</p>
<h2>Installing Composer</h2>
<p>Installing Composer is easy. You can get information about Composer installing at <a href="http://getcomposer.org">getcomposer.org</a>. Go to your new project folder and do the follow:</p>

{% highlight php startinline %}
curl -s https://getcomposer.org/installer | php
{% endhighlight %}

<p>This will install Composer for you. Now you have a <em>composer.phar</em> file inside your project main dir. Ok! But this way you'll can use Composer only for this project. I prefer to move this <em>composer.phar</em> to <em>/usr/bin/</em> and make Composer globally.</p>

{% highlight php startinline %}
sudo mv composer.phar /usr/bin/composer
{% endhighlight %}

<p>Now you have the <strong>composer</strong> command and you can use it where you want.</p>
<h2>Instaling Laravel using Composer</h2>
<p>Now you must create our Laravel 4 Beta project. Inside the folder you'll clone the "develop" branch of Laravel 4 repository:</p>

{% highlight php startinline %}
git clone -b develop git://github.com/laravel/laravel.git
Cloning into 'laravel'...
remote: Counting objects: 19925, done.
remote: Compressing objects: 100% (6296/6296), done.
remote: Total 19925 (delta 14021), reused 19135 (delta 13339)
Receiving objects: 100% (19925/19925), 4.45 MiB | 322 KiB/s, done.
Resolving deltas: 100% (14021/14021), done.
//  listing
ls
laravel
cd laravel
ls
CONTRIBUTING.md  app  artisan  bootstrap  composer.json  phpunit.xml  public  readme.md  server.php  start.php
{% endhighlight %}

<p>Well. Now we have Laravel structure copied to our project folder. Now we have to install dependencies for Laravel, using Composer. This will install everything you need to run Laravel.</p>

{% highlight php startinline %}
composer install
{% endhighlight %}

<p>You said Composer to install everything this project need. In other words you said it to install Laravel framework, that is a lot of small projects. You can see that exists a <strong>composer.json</strong> file inside this folder. There you can see everything this project (Laravel) needs to run. Composer will look inside this file and install everything it needs. You have a output like that:</p>

{% highlight php startinline %}
Loading composer repositories with package information
Installing dependencies
  - Installing ircmaxell/password-compat (1.0.x-dev v1.0.0)
    Cloning v1.0.0

  - Installing symfony/translation (dev-master v2.2.0-BETA2)
    Cloning v2.2.0-BETA2

  - Installing psr/log (1.0.0)
    Loading from cache

  - Installing symfony/routing (dev-master de9d647)
    Cloning de9d64780254f45cfa9d9e49602d0a63acf45f75

  - Installing symfony/process (dev-master v2.2.0-BETA2)
    Cloning v2.2.0-BETA2

  - Installing symfony/http-foundation (dev-master v2.2.0-BETA2)
    Cloning v2.2.0-BETA2

  - Installing symfony/event-dispatcher (dev-master v2.2.0-BETA2)
    Cloning v2.2.0-BETA2

  - Installing symfony/http-kernel (dev-master 7d704da)
    Cloning 7d704da42ca56c3f58530a0023a2d532d67005d1

  - Installing symfony/finder (dev-master v2.2.0-BETA2)
    Cloning v2.2.0-BETA2

  - Installing symfony/dom-crawler (dev-master v2.2.0-BETA2)
    Cloning v2.2.0-BETA2

  - Installing symfony/css-selector (dev-master v2.2.0-BETA2)
    Cloning v2.2.0-BETA2

  - Installing symfony/console (dev-master fdb2d93)
    Cloning fdb2d9320106926266a0f7a5a97aab7213e11ad6

  - Installing symfony/browser-kit (dev-master v2.2.0-BETA2)
    Cloning v2.2.0-BETA2

  - Installing swiftmailer/swiftmailer (dev-master 3aec9a3)
    Cloning 3aec9a30346ebf54100a38ba7c71fb8aafae3e9a

  - Installing monolog/monolog (1.3.1)
    Loading from cache

  - Installing laravel/framework (dev-master b3e7df7)
    Cloning b3e7df7894161dcc78d9f2dc11f33f3249c4fdbb

symfony/translation suggests installing symfony/config (2.2.*)
symfony/translation suggests installing symfony/yaml (2.2.*)
symfony/routing suggests installing doctrine/common (>=2.2,<2.4-dev)
symfony/routing suggests installing symfony/config (2.2.*)
symfony/routing suggests installing symfony/yaml (2.2.*)
symfony/event-dispatcher suggests installing symfony/dependency-injection (2.2.*)
symfony/http-kernel suggests installing symfony/class-loader (2.2.*)
symfony/http-kernel suggests installing symfony/config (2.2.*)
symfony/http-kernel suggests installing symfony/dependency-injection (2.2.*)
monolog/monolog suggests installing mlehner/gelf-php (Allow sending log messages to a GrayLog2 server)
monolog/monolog suggests installing raven/raven (Allow sending log messages to a Sentry server)
monolog/monolog suggests installing doctrine/couchdb (Allow sending log messages to a CouchDB server)
monolog/monolog suggests installing ext-amqp (Allow sending log messages to an AMQP server (1.0+ required))
Writing lock file
Generating autoload files
{% endhighlight %}

<p>Good. If you take a look in the output, you can see Laravel uses some components of <a href="http://symfony.com">Symfony2</a>, that is another excellent framework that uses Composer too. This show you how Composer is powerful and how you can add inside your project everything using Composer.</p>
<p>If you want to use ZendFramework's Amazon API (ZendService\Amazon) you can edit the <strong>composer.json</strong> file and add a new line, saying Composer you'll need that library too. After that just run <em>composer install</em> again and it will download and install <em>ZendService\Amazon</em> for you. Zend Framework packages can be found in <a href="http://packages.zendframework.com">http://packages.zendframework.com</a>.</p>

{% highlight php startinline %}
"require": {
    "zendframework/zendservice-amazon": "2.0.*",
},
{% endhighlight %}

<p>To use ZF2 packages you must add a repository too (<em>composer.json</em>):</p>

{% highlight php startinline %}
"repositories": [
    {
        "type": "composer",
        "url": "https://packages.zendframework.com/"
    }
],
{% endhighlight %}

<h3>Checking everything installed</h3>
<p>Now inside your project folder go to <em>vendor</em> directory and there you'll see a lot of third-party sources, like <em>symfony</em>, <em>swiftmailer</em> and more. You can see too a <em>autoload.php</em> file. That allow you to call these third-party sources using autoload, following the PHP-FIG rules.</p>
<h2>Available Packages</h2>
<p>As you saw inside <em>composer.json</em> this file contains everything you need to run the project, doing a simple reference like <strong>vendor/package</strong>. But where are these sources?</p>
<p>Every packages you can use with Composer are stored in <a href="https://packagist.org/">Packagist</a> site. There you can browse packages and submit yours too. Simple like that. You have a website where you have a lot of good PHP sources you can use where you want just using Composer for that, and they were developed following the same rules and recommendations.</p>
<h2>Conclusion</h2>
<p>Composer is the future of PHP. PHP is undergoing good changes to improve the language. Composer is the Dependency Manager was missing, and is here to stay.</p>
<p>I think I could give you a little explanation what you can do with Composer and how it works. If you'll start a new project, considere using frameworks that use Composer to smooth your third-party sources and make your project organized, like <a href="http://laravel.com">Laravel</a>, <a href="http://framework.zend.com">Zend Framework 2</a> or <a href="http://symfony.com">Symfony</a>.</p>
<p>Thanks for reading.</p>
