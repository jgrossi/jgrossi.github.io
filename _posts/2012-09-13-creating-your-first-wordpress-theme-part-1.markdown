---
layout: post
status: publish
published: true
title: Creating your first Wordpress theme - Part 1

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Continuing with our first Wordpress site, now we'll understand how Wordpress allow you to customize themes. If you didn't read the first post about Wordpress <a href="http://juniorgrossi.com/thinking-the-wordpress-way-first-steps/">Thinking the Wordpress Way - First Steps</a> and want to learn how Wordpress works, take a look.

wordpress_id: 397
wordpress_url: http://juniorgrossi.com/?p=397
date: '2012-09-13 19:11:47 -0300'
date_gmt: '2012-09-13 19:11:47 -0300'
categories:
- PHP
- WordPress
tags: []
---
<p>Hello again...</p>
<p>Continuing with our first Wordpress site, now we'll understand how Wordpress allow you to customize themes.</p>
<p>If you didn't read the first post about Wordpress <a href="http://juniorgrossi.com/thinking-the-wordpress-way-first-steps/">Thinking the Wordpress Way - First Steps</a> and want to learn how Wordpress works, take a look.</p>
<p>You don't have to change your layout or using specific techniques to create your own layout. I suggest you create the basic layout the way you want, using CSS (or LESS), Javascript, HTML, images and what you want.</p>
<h2>File Structure</h2>
<p>Our new theme will called <strong>grossi</strong>. Every theme you create will be stored in the <strong>/wp-content/themes/grossi</strong>.</p>
<p>First, you need to create the <strong>grossi</strong> folder and put a <strong>style.css</strong> file inside it with some content. This file is used by Wordpress to see your new theme. After that you'll can activate your new theme in your Administration Panel.</p>
<p><a id="more"></a><a id="more-397"></a></p>
<h3>What should I have to put inside <strong>style.css</strong> file?</h3>
<p>Wordpress will see that file if you write some code inside it, in the CSS comments format.</p>

{% highlight php startinline %}
/*
Theme Name: Grossi
Theme URI: http://juniorgrossi.com
Description: Theme developed for Junior Grossi's blog
Author: Junior Grossi
Version: 0.8
License: GNU General Public License v2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html
Tags: clean, white, small
Text Domain: grossi
*/
{% endhighlight %}

<p>You don't need to fill all data, but at least "Theme Name" and "Description".</p>
<p>After that you can log in your Administration Panel and activate the "Grossi" theme. After activated the new theme if you access your website url you see an empty page, because we don't have anything yet.</p>
<h3>Creating the Home Page file</h3>
<p>Create a <strong>index.php</strong> file inside our <strong>grossi</strong> folder. I'll consider that you already created your own template (the way you want) file, using just a PHP file and images and CSS you want, for example.</p>
<p>You can put all your template file content you created in our <strong>index.php</strong> file. Just pay attention with the paths. To get the path to our <strong>grossi</strong> folder you can use:</p>

{% highlight php startinline %}
<?php echo get_template_directory_uri() ?>
{% endhighlight %}

<p>So, if your CSS file is <strong>application.css</strong> and it is inside the <strong>css</strong> folder you'll have:</p>

{% highlight php startinline %}
<link rel="stylesheet" type="text/css" href="<?php echo get_template_directory_uri() ?>/css/application.css" media="all" />
{% endhighlight %}

<p>You must update all path inside your file and if you visit your website know you'll see what you created before.</p>
<h3>Creating <em>header.php</em> and <em>footer.php</em> files</h3>
<p>This is the way that Wordpress works like (very like) a layout engine. I assume you have parts of your HTML that you want to use a lot of times around your website.</p>
<p>Let's take a <strong>index.php</strong> file example:</p>

{% highlight php startinline %}
<!DOCTYPE HTML>
<html lang="en-US">
<head>
    <meta charset="UTF-8">
    <title>My First Page</title>
    <link rel="stylesheet" type="text/css" href="<?php echo get_template_directory_uri() ?>/css/application.css" media="all">
</head>
<body>

    <div id="header">

        <img src="<?php echo get_template_directory_uri() ?>/img/logo.png" alt="Logo" class="logo">

        <ul class="nav">
            <li>
                <a href="#">About Us</a>
                <a href="#">Customers</a>
                <a href="#">Services</a>
            </li>       
        </ul>

    </div>

    <div id="content">

        <h1>First Title</h1>

        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
        consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>

    </div>

    <div id="footer">
        Created by <a href="http://juniorgrossi.com">Junior Grossi</a>
    </div>

</body>
</html>
{% endhighlight %}

<p>So now we have to put what is header inside the <strong>header.php</strong> and what is footer in the <strong>footer.php</strong> file. Both inside <strong>grossi</strong>, our theme folder.</p>
<p>Now we must call <strong>header.php</strong> and <strong>footer.php</strong> inside our <strong>index.php</strong> file. It's pretty simple. Just use some custom Wordpress functions to get that. Take a look how our <strong>index.php</strong> will contain.</p>

{% highlight php startinline %}
<?php get_header() ?>

<h1>First Title</h1>

<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>

<?php get_footer() ?>
{% endhighlight %}

<p>What we did here is tell to Wordpress "Hey, using <em>get_header()</em> you'll get what exists inside my <strong>header.php</strong> file and using <em>get_footer()</em> what exists inside the <strong>footer.php</strong> file. Just that!</p>
<p>Every time you want to get the <strong>header.php</strong> content you just put <em><?php get_header() ?></em> on the top of the page. Simple like that!</p>
<p>In the second part we will see: <a href="http://juniorgrossi.com/creating-your-first-wordpress-theme-part-2">Go to the second part</a></p>
<ul>
<li>Create Page templates</li>
<li>Retrieve posts to a page or post file</li>
</ul>
