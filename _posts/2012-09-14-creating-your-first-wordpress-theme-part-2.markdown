---
layout: post
status: publish
published: true
title: Creating your first WordPress theme – Part 2

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Continuing with our Wordpress Theme's series, in this post you will learn how to create page templates and how to retrieve posts inside your theme files.
wordpress_id: 453
wordpress_url: http://juniorgrossi.com/?p=453
date: '2012-09-14 21:54:14 -0300'
date_gmt: '2012-09-14 21:54:14 -0300'
categories:
- PHP
- WordPress
tags: []
---
<p>Hello everyone!</p>
<p>As I said, this is our second post about Themes in Wordpress. Today I'll talk about:</p>
<ul>
<li>How to create page templates</li>
<li>How to retrieve posts</li>
</ul>
<p>If you want you can <a href="http://juniorgrossi.com/creating-your-first-wordpress-theme-part-1/">read the first post about Wordpress themes</a>.</p>
<p>Let's GO!</p>
<h2>Page Templates</h2>
<p>You can customize pages the way you want, as you want. If you want to create a "About Us" page with Flash (arg!), images, js, css, etc, etc, etc. You must tell to Wordpress what page is that.</p>
<p>So, lets create the "About Us" page. First you must create the <strong>"about-us.php"</strong> file under <strong>grossi</strong> folder (our folder's theme, remember? It is under /wp-content/themes/grossi). But now we've to give it a name, a <em>Template Name</em>. So... inside this file, let's crete a PHP Comment Block (/* */) and write the follow:</p>

{% highlight php startinline %}
<?php
/*
 * Template Name: About Us
 */
?>
{% endhighlight %}

<p><a id="more"></a><a id="more-453"></a></p>
<p>Well, done! For now, create a new page under your Administration Panel (<em>Pages -> Add New</em>). After you must write a title and a content, just that for now. Take a look in the right side of this page. You have there a option like <strong>Template</strong> or <strong>Template Name</strong>. There you'll find the "About Us" option. Now WordPress knows that the file <strong>about-us.php</strong> is the template for that page you've created.</p>
<p>So visit your new page and you will find a blank page, right? Exactly! Your <strong>about-us.php</strong> file has only the PHP Comment, remember? Let's update it and call there the <em>header</em> and <em>footer</em> parts.</p>

{% highlight php startinline %}
<?php
/*
 * Template Name: About Us
 */
?>

<?php require_once 'header.php'; ?>

<h1>About Us</h1>

<p>Your text content here</p>

<?php require_once 'footer.php'; ?>
{% endhighlight %}

<p>Good! But and my page title and content I've filled before in the Administration Panel? Easy! Let's review some steps:</p>
<ol>
<li>You've created a page</li>
<li>Said Wordpress what is the page's template</li>
<li>Wordpress knows what you want</li>
</ol>
<p>So... inside your <strong>about-us.php</strong> file you have all page information. Let's replace our file with that information:</p>

{% highlight php startinline %}
<?php
/*
 * Template Name: About Us
 */
?>

<?php get_header() ?>

<?php 
    // First you must tell Wordpress that this is THE POST (or page in this case)
    the_post(); // yes, just it!!!!
?>

<h1><?php the_title() ?></h1>

<?php the_content() ?>

<?php get_footer() ?>
{% endhighlight %}

<p>As you saw, <strong>the_title</strong> echo the title for you, ECHO! You can only <em>get</em> the title as follow:</p>

{% highlight php startinline %}
<?php echo get_the_title() ?>

<!-- OR -->

<?php the_title() ?>
{% endhighlight %}

<h2>Retrieving posts</h2>
<p>Let's imagine you must add only the services (titles) that the company does. So we have to <strong>get</strong> that posts, right? Remember, we have a <strong>post type</strong> "Services"! Let's get the services and write them inside the "About Us" page.</p>

{% highlight php startinline %}
<?php 
    // lest reset the query and get new posts        
    wp_reset_query(); 
    query_posts(array(
        "post_type" => "services",
    )); 
?>

<h2>Our Services</h2>

<?php // Now let's loop through the posts (Wordpress call this THE LOOP) and on each iteration say: "This is THE POST"

<?php while (have_posts()) : the_post(); ?>

    <h3><?php the_title() ?></h3>

<?php endwhile; ?>
{% endhighlight %}

<p>So we have now all informations about the company's services. Look, on the same page we got the page informations and the Post Type "Services". As you imagine you can get <strong>WHAT YOU WANT</strong> in the same page, the way you want.</p>
<p>The <strong>query_post</strong> is part of the <a href="http://codex.wordpress.org/Class_Reference/WP_Query">WP_Query</a> class. You have a lot of options like order, posts&#95;per&#95;page, post_status, e much more. Take a look on <a href="http://codex.wordpress.org/Class_Reference/WP_Query">WP_Query</a> and you'll understand a lot of information about getting posts.</p>

<p>In the next post I'll explain other files you can use to make your life easier. See you!</p>
