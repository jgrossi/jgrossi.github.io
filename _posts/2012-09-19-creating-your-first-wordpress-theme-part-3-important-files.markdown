---
layout: post
status: publish
published: true
title: Creating your first WordPress theme – Part 3 - Important files

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Let's continue with our Wordpress series. Today I'll write about the main files in a Wordpress theme, what you have to know and how them work in a Wordpress environment.

wordpress_id: 464
wordpress_url: http://juniorgrossi.com/?p=464
date: '2012-09-19 12:43:50 -0300'
date_gmt: '2012-09-19 12:43:50 -0300'
categories:
- PHP
- WordPress
tags: []
---
<p>Hello all!</p>
<p>Let's continue with our Wordpress series. Today I'll write about the main files in a Wordpress theme.</p>
<h2>First files</h2>
<p>Above we have some files that are basic for your new theme:</p>
<h3>index.php</h3>
<p>This file is the root file, the home page. When you open your Wordpress website it'll get this file content. Is can also be renamed to <strong>home.php</strong>, not problem. Wordpress will understand that.</p>
<h3>header.php</h3>
<p>As we saw in the <a href="http://juniorgrossi.com/creating-your-first-wordpress-theme-part-1/">first post about Wordpress themes</a>, this is the file you'll have your header content. Things like navigation, logo, users welcome messages and more, you'll find in this file.</p>
<p><a id="more"></a><a id="more-464"></a></p>
<h3>footer.php</h3>
<p>The same think with header.php file, but with footer content. Here you get store signatures, quick links, policy terms and more.</p>
<h3>style.css</h3>
<p>Do you remember this file? This file is used to tell to Wordpress about your theme, like name, description, author details and more. You can get some example about fill this file in the <a href="http://juniorgrossi.com/creating-your-first-wordpress-theme-part-1/">first post</a>.</p>
<h2>Post Files</h2>
<p>As said Wordpress is based on posts and pages, so everything you need to manage is one of this 2 types.</p>
<h3>single.php</h3>
<p>This is the basic file that will be renderer if you are talking about 1 (just one) post. For example, if you have a post called "My first post", when is visit it the Wordpress will render the "single.php" file. Inside it you will work with some functions we already know, like:</p>

{% highlight php startinline %}
<?php the_post() ?>
<h1><?php the_title() ?></h1>
<p><?php the_content() ?></p>
{% endhighlight %}

<h3>single-<em>post-type</em>.php</h3>
<p>As we saw on the <a href="http://juniorgrossi.com/creating-your-first-wordpress-theme-part-1/">first post</a> about Wordpress themes, we can create custom <strong>Post Types</strong>. For example, we can create a custom post type called "Services", to list all services the company does. So, if our custom post type is called "Services" and we want to show one service we can create the <strong>single-services.php</strong> file.</p>
<p>If you want to show all services the company does we can do that where you want. For example, we can have the "About Us" page, with one title and content. In the page template we can call the <em>post type services</em> and list all of them that.</p>
<h2>Page Files</h2>
<p>As posts, pages have their own files to render too. You can customize files or use the default.</p>
<h3>page.php</h3>
<p>This file will be called if you don't have more specific page files. Let's go back to the "About Us" page. If we don't insert the <em>Template Name</em> inside some file, Wordpress will search for the <strong>page.php</strong> file.</p>
<h3><em>custom</em>.php</h3>
<p>As we saw, you can add the code above inside a file (you choose the filename) to be linked to a specific page.</p>

{% highlight php startinline %}
<?php
/*
 * Template Name: About Us
 */
?>
{% endhighlight %}

<h3>page-<em>id</em>.php and page-<em>slug</em>.php</h3>
<p>You can link a file to a page you've created using other 2 ways. You can get the page ID and create a file using it, or you can use the page slug for that too.</p>
<p>Using our "About Us" page. Let's assume it has the ID 67 and the slug "about-us". We can reference this page two ways: creating <strong>page-67.php</strong> or creating <strong>page-about-us.php</strong>. I don't like to use this last named style. A slug can change and your work will increase. I prefer use "Template Name" way or "page-ID.php".</p>
<h2>Searching with Wordpress</h2>
<p>Wordpress has a built in search function. What you must have is a <em>text input tag</em> named "s", the <em>form action</em> to the home page and the <em>form method</em> to GET. Just that.</p>
<h3>searchform.php</h3>
<p>To do the search operation you can put the form where you want. Using best practices, we use to create a new PHP file named <strong>searchform.php</strong>. Inside it we can insert, for example, the follow (you can customize too):</p>

{% highlight php startinline %}
<?php // searchform.php ?>    
<form method="get" action="<?php echo home_url('/') ?>">
    <input type="text" placeholder="Procurar" name="s" id="query" />
    <input type="submit" value="Search" />
</form>
{% endhighlight %}

<p>Ok, but how can I insert the form in my <strong>header.php</strong> for example? You can <em>require</em> or <em>include</em> it but there is the function called <strong>get&#95;search&#95;form</strong> that ECHO the form inside the <strong>searchform.php</strong> file.</p>

{% highlight php startinline %}
<?php // Inside the header.php or the file you want ?>
<?php get_search_form() ?>
{% endhighlight %}

<h3>Search results and <em>search.php</em></h3>
<p>To show the search results is easy. You must only create the <strong>search.php</strong> file and work as a page file or custom post file. Let's write one file example.</p>

{% highlight php startinline %}
<?php get_header() ?>

<h1>Search</h1>
<h2>Search results for "<?php echo get_search_query() ?>":</h2>
<?php while (have_posts()) : the_post() ?>
    <h3><a href="<?php the_permalink() ?>"><?php the_title() ?></a></h3>
    <a href="<?php the_permalink() ?>">Go to post</a>
<?php endwhile; ?>

<?php get_footer() ?>
{% endhighlight %}

<h2>404 Error File and Others</h2>
<p>You can customize your 404 error too. It's simple, just creating the <strong>404.php</strong> file and customize it the way you want.</p>
<p>Wordpress is a very large CMS tool and you can do even more with it. There are a lot of files we didn't write about in this post. You can get more information about them in the "Wordpress Template Hierarchy" page, that I'm talking about above.</p>
<p>These non covered files are about categories, taxonomy, tags, authors, dates, archives, attachments, comments and more. I hope write a post about comments in Wordpress and will update the link here.</p>
<h2>Wordpress Template Hierarchy</h2>
<p>Wordpress has a very good page to explain about theme's files. I suggest you read the <a href="http://codex.wordpress.org/Template_Hierarchy">Wordpress Template Hierarchy</a> page for more information. This page has a very good graph that I reference above.</p>
<p>You can get a snapshot about Wordpress themes files in the image below.</p>
<p><img src="http://codex.wordpress.org/images/1/18/Template_Hierarchy.png" alt="Wordpress Template Hierarchy" /></p>
<p>Thanks for reading!</p>
