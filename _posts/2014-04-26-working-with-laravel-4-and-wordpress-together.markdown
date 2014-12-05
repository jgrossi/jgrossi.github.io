---
layout: post
status: publish
published: true
title: Working with Laravel 4 and Wordpress together
excerpt: |+
  Currently I am working on a project where I had to make some choices about technologies and how work with them together. First this project will be developed using Wordpress only. It's a College Group site, where we had to work with 13 schools around the world and each one must has the control of your own content.

wordpress_id: 833
wordpress_url: http://www.grossi.io/?p=833
date: '2014-04-26 19:53:02 -0300'
date_gmt: '2014-04-26 19:53:02 -0300'
categories:
- PHP
- WordPress
- Laravel
- Web Server
- Laravel 4
tags: []
---
<p>Hi everybody!</p>
<p><a href="#using-wordpress-corcel">Updated May 11th 2014</a></p>
<p>Currently I am working on a project where I had to make some choices about technologies and how work with them together. First this project will be developed using Wordpress only. It's a College Group site, where we had to work with 13 schools around the world and each one must has the control of your own content.</p>
<p>This could be made with Wordpress, but I think when the site is not so small maybe you can use another CMS or Framework, because I particularly prefer to work with MVC. So because some decisions inside the company we decided to use Wordpress Admin Panel, that is a very good, use its architecture and its database. So Wordpress will be used to the application back-end, with user control, user permission, etc.</p>
<p>To the front-end we decided to work with Laravel. To query information from the Wordpress database we've used the wordpress functions inside Laravel, so it's much better to work with a MVC Wordpress.</p>

<h2>Installing Wordpress and Laravel</h2>
<p>Let's install Laravel first using Composer. This will take some while.</p>

{% highlight bash %}
composer create-project laravel/laravel
{% endhighlight %}

<p>So we have now our Laravel folder structure with everything installed. Now let's install a fresh installation of Wordpress inside Laravel. Here you have to options:</p>
<ol>
<li>Install Wordpress as a sub-dir of <code>public</code> Laravel folder, like <code>/public/wordpress</code>. So to access your Wordpress Admin you have to go to something like <code>http://example.com/wordpress/wp-admin</code>.</li>
<li>Install Wordpress as a sub-dir of the Laravel's root, like <code>/wordpress</code>. So you will have <code>/app</code> and <code>/wordpress</code> in the same position. For this you have to create another VirtualHost to point to Wordpress installation. You can setup a subdomain lik <code>wp.example.com</code> and poit it to <code>/laravel/folder/wordpress</code>. This way you can access the Admin going to <code>http://wp.example.com/wp-admin</code>.</li>
</ol>
<h3>Removing the Wordpress front-end</h3>
<p>For security issues go to <code>/wordpress/index.php</code> file and put this first, to redirect to the Admin login page:</p>

{% highlight php startinline %}
header("Location: ./wp-admin");
exit();
{% endhighlight %}

<p>So, when you visit <code>http://wp.example.com</code> or <code>http://example.com/wordpress</code> you are redirected to the Wordpress Admin login page.</p>
<h2>Connect Laravel to Wordpress</h2>
<p>Now we have to include Wordpress inside Laravel. It's easier than you think. Just edit the <code>public/index.php</code> Laravel file and include the follow <strong>before anything</strong>:</p>

{% highlight php startinline %}
/*
|--------------------------------------------------------------------------
| Wordpress
|--------------------------------------------------------------------------
|
| Integrate Wordpress with Laravel core
|
*/

define('WP_USE_THEMES', false);
require __DIR__.'/../wordpress/wp-blog-header.php';
{% endhighlight %}

<p>Now you can use Wordpress functions with Laravel. Yes, like a piece of cake!!!</p>
<h3>Querying Posts</h3>
<p>Let's suppose you need to get some posts and show to the view file:</p>

{% highlight php startinline %}
// app/controllers/SchoolController.php

class SchoolController extends BaseController
{
    public function index()
    {
        $query = new WP_Query(array(
            'post_type' => 'school',
            'posts_per_page' => 20,
            'order' => ASC,
            'orderby' => 'post_title',
        ));

        $posts = $query->get_posts();

        return View::make('school.index')->with('schools', $posts);
    }
}
{% endhighlight %}

<p>Maybe you need some Wordpress features but you have to use it from your theme's folder. If you don't know how to create a Wordpress Theme, check <a href="http://grossi.io/2012/creating-your-first-wordpress-theme-part-1/">this post</a>.</p>
<p>For example, in my project I had to create a "About" page for each school. So I create pages with taxonomies called "SchoolName". After, I create a page with the "About" title and select the "São Paulo's School" term in my taxonomy "SchoolName". But all schools will have the same page structure, right? So I have to create a template page inside my Wordpress theme's folder. This way I can have custom fields to this page and all schools will have the About page looking like the same.</p>
<p>So, create a custom theme, adding a <code>style.css</code> file and a <code>index.php</code> file and activating it from the Wordpress panel. After that create a new file called <code>about.php</code>. Inside it put:</p>

{% highlight php startinline %}
<?php /* Template Name: About page */ ?>
{% endhighlight %}

<p>So I created a Template called "About page" and when creating the "About" page I select the "About page" template name too. Now my page has a custom taxonomy and a template name.</p>
<p>Now let's query some pages.</p>
<h3>Getting Pages</h3>
<p>Now if you have to get information about some page, like "About" page. Remember here that the permalinks from Wordpress won't be used. We are going to use only the Laravel routes and URL's. We are using Wordpress only to fill the database and use its Admin Panel.</p>

{% highlight php startinline %}
/**
 * Getting page from Wordpress
 */

// Getting page by ID
$page = get_post(1234);

// Getting page by slug
$query = new WP_Query(array(
    'post_type' => 'page',
    'posts_per_page' => 1,
    'name' => 'about', // here the 'about' is the page slug you stored in wordpress when creating the page
));
$page = array_shift($query->get_posts()); // first post returned

// Getting page by template name
$templateFileName = 'about.php'; // here you must use the PHP FILE we've created in the theme folder
$query = new WP_Query(array(
    'post_type' => 'page',
    'my-taxonomy-name-here' => 'my-term-slug-here',
    'posts_per_page' => 1,
    'meta_key' => '_wp_page_template',
    'meta_value' => $templateFileName,
));
$page = array_shift($query->get_posts());
{% endhighlight %}

<p>So, that's it. Now you can use the power of Laravel with the facilities of Wordpress. So, if you're creating a website and need more organisation to your files you have use both to make your life easier.</p>
<p>You can too use custom database tables to work only with Laravel, using Models, etc. You can join both worlds and have a better final result.</p>
<h1 id="using-wordpress-corcel">Using Wordpress Corcel</h1>
<p>Some time ago I started to create a project based on Eloquent ORM (from Laravel) and Wordpress. The project is hosted at Github and you can <a href="https://github.com/jgrossi/corcel">get more info here</a>.</p>
<p>You can install it using Composer. Just add <code>jgrossi/corcel</code> in your <code>composer.json</code> file and be happy.</p>
<p>There are much work to do but currently you can use some cool features, like:</p>
<h2>Getting posts</h2>

{% highlight php startinline %}
// All published posts
$posts = Post::published()->get();
$posts = Post::status('publish')->get();

// A specific post
$post = Post::find(31);
echo $post->post_title;
{% endhighlight %}

<h2>Working with custom post types</h2>

{% highlight php startinline %}
// using type() method
$videos = Post::type('video')->status('publish')->get();

// using your own class
class Video extends Corcel\Post
{
    protected $postType = 'video';
}
$videos = Video::status('publish')->get();
{% endhighlight %}

<h2>Custom post type and custom fields (meta data)</h2>

{% highlight php startinline %}
// Get 3 posts with custom post type (store) and show its title
$stores = Post::type('store')->status('publish')->take(3)->get();
foreach ($stores as $store) {
    $storeAddress = $store->address;
}
{% endhighlight %}

<h2>Pages</h2>

{% highlight php startinline %}
// Find a page by slug
$page = Page::slug('about')->first(); // OR
$page = Post::type('page')->slug('about')->first();
echo $page->post_title;
{% endhighlight %}

<p>If you want to contribute with this project you're very welcome. Ideas are welcome too.</p>
<p>Thank you for reading.</p>
