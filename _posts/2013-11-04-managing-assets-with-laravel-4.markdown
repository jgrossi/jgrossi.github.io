---
layout: post
status: publish
published: true
title: Managing assets with Laravel 4
author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Generally assets are stored in your public directory, right? They are public, so anyone can get access to them. But nowadays the performance is a very important factor when deploying a new app. I strongly recommend you to minify and cache your assets, like CSS files, Javascript files and Images.
  
wordpress_id: 749
wordpress_url: http://juniorgrossi.com/?p=749
date: '2013-11-04 16:52:52 -0200'
date_gmt: '2013-11-04 16:52:52 -0200'
categories:
- PHP
- Laravel
- Composer
- Laravel 4
tags: []
---
<p>Generally assets are stored in your public directory, right? They are public, so anyone can get access to them. But nowadays the performance is a very important factor when deploying a new app. I strongly recommend you to minify and cache your assets, like CSS files, Javascript files and Images.</p>
<p>If you are using 11 javascript files in your app you don't have to make 11 requests on the server, one per JS files. You can easily join all files and minify them, so you will have just one minified file. This is easy to do using Laravel 4! I've found four Laravel 4 assets managers:</p>
<ul>
<li><a href="http://github.com/codesleeve/asset-pipeline">codesleeve/asset-pipeline:</a> <strong>My choice!</strong> It is easy to understand and to work with </li>
<li><a href="http://github.com/jasonlewis/basset">jasonlewis/basset:</a> Very good library created by Jason Lewis </li>
<li><a href="http://github.com/teepluss/asset">teepluss/asset:</a> Based on the Laravel 3 Asset class. It does not minify or join your files. </li>
<li><a href="http://github.com/way/guard-laravel">way/guard-laravel:</a> A very good package but you must have Ruby installed </li>
</ul>
<p>We will work with <a href="http://github.com/codesleeve/asset-pipeline">codesleeve/asset-pipeline</a> package. It's easy to use and simple to understand.</p>
<p><a id="more"></a><a id="more-749"></a></p>
<h2>Installation</h2>
<p>Let's suppose you already have a Laravel 4 working. First, using Composer, you have to install the package. So, open your <code>composer.json</code> and add the package information to install it:</p>

{% highlight php startinline %}
{ 
    ... 
    "require": { 
        "laravel/framework": "4.0.*", 
        "codesleeve/asset-pipeline": "dev-master" 
    }, 
    ... 
}
{% endhighlight %}

<p>Now on the terminal update your <code>composer</code>. composer update Now you have to tell Laravel you are using this asset management package. So, open your <code>app/config/app.php</code> and add this line inside the <code>providers</code> block:</p>

{% highlight php startinline %}
'Codesleeve\AssetPipeline\AssetPipelineServiceProvider',
{% endhighlight %}

<h2>Creating the assets directory</h2>
<p>Now, on the terminal type a <code>artisan</code> command (from the package we've installed):</p>

{% highlight php startinline %}
php artisan assets:setup 
{% endhighlight %}

<p>This will create the <code>app/assets</code> folder. Inside it you will find 2 new directories: <code>javascripts</code> and <code>stylesheets</code>. Inside each one you will find a file called "application" (application.css and application.js). These are our <code>manifest</code> files. You will understand what a "manifest" file is later. So we have the follow structure:</p>
<ul>
<li>/app
<ul>
<li>assets
<ul>
<li>javascripts
<ul>
<li>application.js </li>
</ul>
</li>
<li>stylesheets
<ul>
<li>application.css</li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
</ul>
<h2>The package "logic"</h2>
<p>In a easy way what the package does is to get the assets files content, join them, minify them and save the result. So, the package will get your "assets file list", get the content one by one, concatenate them and run a CSS minify or JS minify algorithm, provided by some PHP class.</p>
<h2>The base assets route</h2>
<p>By default the package will use the route <code>http://example.org/assets/application.(js|css)</code>. You can find it in the routes list:</p>

{% highlight php startinline %}
php artisan routes 
{% endhighlight %}

<p>You can find a <code>GET /assets/{path}</code> route. You can change that for what you want. Let's suppose you want <code>/static/application.js</code> for example. You only have to change the package config file. First you have to install this config file, so run the follow <strong>artisan</strong> command:</p>

{% highlight php startinline %}
php artisan config:publish codesleeve/asset-pipeline 
{% endhighlight %}

<p>This will create the <code>app/config/packages/codesleeve/asset-pipeline/config.php</code>. You can change the route you want and other options inside this config file.</p>
<h2>Testing</h2>
<p>Now, go to <code>http://yoursite.org/assets/application.js</code> and see the result. ATTENTION: if you got a 404 error you are doing something wrong, check:</p>
<ol>
<li>
<p>You have a <code>assets</code> directory inside your <code>public</code> dir. If you have one, please, change its name to, for example, <code>assets2</code>, just for now.</p>
</li>
<li>
<p>You are using the PHP 5.4+ built-in server the wrong way. I had a BIG problem with this. You cannot start the PHP server (with Laravel 4) this way</p>
<p><code>php -S localhost:8000 -t public</code>.</p>
</li>
</ol>
<p>You <strong>MUST</strong> use the <code>server.php</code> file that Laravel 4 provides you, so you must start the PHP server this way: <code>php -S localhost:8000 server.php</code>. Now you can see the <code>http://yoursite.org/assets/application.js</code> file, but with the default content.</p>
<h2>Inserting the files inside your HTML</h2>
<p>Now you have everything working you have to include this tags inside your HTML. So insert the above inside your template or view file:</p>

{% highlight php startinline %}
{{ stylesheet_link_tag() }}
{{ javascript_include_tag() }}
{% endhighlight %}

<h2>Copying your assets file you already are using</h2>
<p>You can copy the <code>css</code> and <code>js</code> files you are using to the respective directories inside <code>app/assets</code> folder. Copy them, and open your website and check the code. You will se all the JS and the CSS files. If you are using your environment as "production" you will see all files concatenated in only one. The package show all files for "testing" or "development" environments and just one for the "production" one.</p>
<h2>Setting the file ordering</h2>
<p>We've talking about the MANIFEST file, right? Manifest file is the file that will control the order to load each asset file (JS or CSS). Open the <code>app/assets/javascripts/application.js</code>. You will see something like this:</p>

{% highlight php startinline %}
// This is a manifest file that'll be compiled into application.js, which will include all the files 
// listed below. 
// 
// Any JavaScript/Coffee file within this directory, lib/assets/javascripts, vendor/assets/javascripts, 
// can be referenced here using a relative path. 
// 
// It's not advisable to add code directly here, but if you do, it'll appear in whatever order it 
// gets included (e.g. say you have require_tree . then the code will appear after all the directories 
// but before any files alphabetically greater than 'application.js' 
// 
// The available directives right now are require, require_directory, and require_tree 
//  
//= require_tree . The line where you have
{% endhighlight %}

<p><code>= require_tree .</code> is telling the asset manager to load all files inside the <code>javascripts</code> directory in alphabetic order. If you want to set a custom order you can write something like that:</p>

{% highlight php startinline %}
//= require ./jquery 
//= require ./some-lib 
//= require ./another-lib 
{% endhighlight %}

<p>Now you will have all files you specified inside the MANIFEST file minified and being managed by the package. Now, you can do the same for CSS files. The package has more options like cache, etc. You can check the package's configuration file and found more details there. Be happy!</p>
