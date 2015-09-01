---
layout: post
status: publish
published: true
title: Creating your first Composer/Packagist package

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Today I'll write about how you can contribute with PHP community creating packages (or updating your's) using <a href=\"http://getcomposer.org\">Composer</a> and <a href=\"http://packagist.org\">Packagist</a>. First, if you're a PHP developer
  and don't know yet what is Composer, take a look on the post <a href=\"http://grossi.io/2013/why-you-should-use-composer-and-how-to-start-using-it/\">Why you should use Composer and how to start using it</a> to get more information about.

wordpress_id: 590
wordpress_url: http://juniorgrossi.com/?p=590
date: '2013-03-06 21:48:15 -0300'
date_gmt: '2013-03-06 21:48:15 -0300'
categories:
- PHP
- Composer
tags: []
---
<p>Hi everybody! Today I'll write about how you can contribute with PHP community creating packages (or updating your's) using <a href="http://getcomposer.org">Composer</a> and <a href="http://packagist.org">Packagist</a>. First, if you're a PHP developer and don't know yet what is Composer, take a look on the post <a href="http://grossi.io/2013/why-you-should-use-composer-and-how-to-start-using-it/">Why you should use Composer and how to start using it</a> to get more information about.</p>
<h2>Using Composer</h2>
<p>Composer is a package manager for PHP. You can <strong>use</strong> packages the community developed and you can <strong>contribute</strong> with your packages too. Here I'll show how to create a project/package, install Composer inside it and send to Packagist, where others developers can use it inside their projects. <a id="more"></a><a id="more-590"></a></p>
<h2>Creating the Package</h2>
<p>You can create a new project or update one to use Composer. I'll create a hello world class. It's a simple class but you can create complex projects and share them with the others developers. I'll use <strong>"hello-world"</strong> as project's name. Composer work in <strong>"vendor/package"</strong> name format. Here we can set as "vendor" name my name: <em>"juniorgrossi"</em> and as package name <em>"hello-world"</em>, the name of the project.</p>
<h3>Files Structure</h3>
<p>You can put all files inside the main dir, but I strongly recommend to create another dir, as <strong>"src"</strong> to be easier to understand and maintain your code organized. The project structure will start with the follow: * hello-world (root dir) * src * HelloWorld * SayHello.php Our SayHello.php file will have:</p>

{% highlight php startinline %}
<?php 

namespace HelloWorld;

class SayHello
{
    public static function world()
    {
        return 'Hello World, Composer!';
    }
}
{% endhighlight %}

<h3>Starting Composer</h3>
<p>As our project is ready we can "install" Composer inside it. This is only create a <strong>"composer.json"</strong> file inside root dir, but Composer can do that for you. Inside your project root: composer init You'll have the follow Composer response:</p>

{% highlight php startinline %}
macbook:hello-world grossi$ composer init


  Welcome to the Composer config generator  



This command will guide you through creating your composer.json config.

Package name (<vendor>/<name>) [juniorgrossi/hello-world]:
 You can accept the default or customize it like "yourname/hello" or what you want. Complete all Composer questions like: 

Package name (<vendor>/<name>) [juniorgrossi/hello-world]: juniorgrossi/hello-world
Description []: My first Composer project
Author [Junior Grossi <me@juniorgrossi.com>]: Your Name <your@name.com>
Minimum Stability []: dev

Define your dependencies.

Would you like to define your dependencies (require) interactively [yes]? no
Would you like to define your dev dependencies (require-dev) interactively [yes]? no

{
    "name": "juniorgrossi/hello-world",
    "description": "My first Composer project",
    "authors": [
        {
            "name": "Your Name",
            "email": "your@name.com"
        }
    ],
    "minimum-stability": "dev",
    "require": {

    }
}

Do you confirm generation [yes]? yes
 Now you have "composer.json" file saved in your root dir. It's almost ready but we must do some changes: 

{
    "name": "juniorgrossi/hello-world",
    "description": "My first Composer project",
    "authors": [
        {
            "name": "Your Name",
            "email": "your@name.com"
        }
    ],
    "minimum-stability": "dev",
    "require": {
        "php": ">=5.3.0"
    },
    "autoload": {
        "psr-0": {
            "HelloWorld": "src/"
        }
    }
}
{% endhighlight %}

<p>What we did here is add information about PHP 5.3 as minimum requirements (<strong>require</strong> section) and tell Composer to <strong>"autoload"</strong> (using <a href="https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md">PSR-0</a>) all files with "HelloWorld" namespace that are inside "src" dir.</p>
<h3>Testing Package</h3>
<p>Shure we want to do a simple test to verify if our class is working well. You can create a new project and "paste" your classes inside it or test inside your own project, wich is better and easier. We're creating a Composer project so we must have Composer files installed inside our projects. So, install it running <strong>"composer install"</strong> inside your root dir:</p>

{% highlight php startinline %}
composer install 
{% endhighlight %}

<p>As you have only "php >=5.3.0" inside "composer.json", Composer will install only it's own files. With Composer installed create a directory "tests" inside your root dir. Create the "test.php" file inside it with the follow content:</p>

{% highlight php startinline %}
<?php 

require_once __DIR__ . '/../vendor/autoload.php'; // Autoload files using Composer autoload

use HelloWorld\SayHello;

echo SayHello::world();
 Go to the terminal (or create a PHP web server inside "tests" dir) and type: 

php tests/test.php
{% endhighlight %}

<p>You'll get <em>"Hello World, Composer!"</em>. It's working now.</p>
<h3>Sending to Packagist.org</h3>
<p>Now your project is working and you want to send it to Packagist. The easy way is push your project to <a href="http://github.com">Github</a> using Git. Go to Github and create a new public repo called <strong>"helloworld"</strong>, start the Git project inside your root dir and push it:</p>

{% highlight php startinline %}
git init 
git add . 
git commit -m "First commit" 
git remote add origin git@github.com:username/helloworld.git 
git push origin master 
{% endhighlight %}

<p>Now you have your project inside a Github repo and you're ready to send it to Packagist. Go to <a href="http://packagist.org">Packagist web site</a>, create your account, login and <strong>Submit a Package</strong>. Packagist'll ask you for <em>Repository URL (Git/Svn/Hg)</em>. Paste there <strong>git@github.com:username/helloworld.git</strong> and click "Check!". Packagist will check your project and return the project name. If it's correct accept it.</p>
<h3>Packagist Details</h3>
<p>Every time you do a new commit to Github you must update the Packagist. Go to your account, your package and click "Force Update!". Packagist will go to Github and update the sources. You can turn on "auto update" going to your Github repo, clicking "Settings", after "Service Hooks" and click the "Packagist" service. There update with your information, like:</p>
<ul>
<li><strong>User:</strong> your Packagist username, like <em>juniorgrossi</em></li>
<li><strong>Token:</strong> your API token, that you can find inside your Packagist settings link</li>
<li><strong>Domain:</strong> <em>packagist.org</em> Ok! Auto update finished and your package is available to other developers. </li>
</ul>
<p>Our first Composer package is finished, but you can do much more using it. Thanks!</p>
