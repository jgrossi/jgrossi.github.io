---
layout: post
status: publish
published: true
title: Deploying Laravel applications on a shared host

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  <a href="http://laravel.com">Laravel</a> is an awesome PHP framework created by <a href="http://twitter.com/taylorotwell">Taylor Otwell</a>. Actually it is on the third version and it is one of the great PHP frameworks we have today. As a lot of frameworks, we have to create our own Apache Virtual Host to point to the <strong>public</strong> dir to improve security and only allow access to really public files.

wordpress_id: 473
wordpress_url: http://juniorgrossi.com/?p=473
date: '2012-10-11 14:39:45 -0300'
date_gmt: '2012-10-11 14:39:45 -0300'
categories:
- Uncategorized
- PHP
- Laravel
tags: []
---
<p>Hello!</p>
<p><a href="http://laravel.com">Laravel</a> is an awesome PHP framework created by <a href="http://twitter.com/taylorotwell">Taylor Otwell</a>. Actually it is on the third version and it is one of the great PHP frameworks we have today.</p>
<p>As a lot of frameworks, we have to create our own Apache Virtual Host to point to the <strong>public</strong> dir to improve security and only allow access to really public files.</p>
<p>Some people asked me to create a blog post about how to deploy Laravel application to a shared host, like my post about how to <a href="http://juniorgrossi.com/deploying-zend-framework-applications-on-a-shared-host/">Deploying Zend Framework applications on a shared host</a>.</p>
<h2>Laravel file structure</h2>
<p>A basic Laravel application has the same file structure:</p>
<ul>
<li><strong>application/:</strong> your app files, like controllers, routes, models, views, tasks, etc</li>
<li><strong>bundles/:</strong> the bundle's files you've installed for your app</li>
<li><strong>laravel/:</strong> the Laravel's files</li>
<li><strong>public/:</strong> your public files</li>
<li><strong>storage/:</strong> files about cache, session, logs, etc</li>
<li><strong>artisan:</strong> the Laravel command line file</li>
<li><strong>licence.txt:</strong> licence file</li>
<li><strong>paths.php:</strong> file with paths information</li>
<li><strong>readme.md:</strong> just a readme file with Laravel instructions</li>
</ul>
<p><a id="more"></a><a id="more-473"></a></p>
<p>The reasoning is the same the Zend Frameworks' post. I think when you login inside your shared host account you'll see a lot of files and folders. You must save your application files inside a "public" dir. This folder's name is something like <strong>public_html</strong> or <strong>www</strong>.</p>
<p>So, if you create a <em>index.php</em> file with the content "Hi. I'm here" inside the <strong>public_html</strong> (or <em>www</em>), when you visit http://example.com you'll se that message. So, the TIP is: <em>"The public_html folder is your Laravel public directory"</em>.</p>
<h2>Organizing shared host files</h2>
<p>To give your shared host files organized, before the public_html folder (the public) we'll create a folder called <em>my_apps</em> (or what you want). Inside that we'll create another folder called <em>my&#95;first&#95;app</em>. Inside this last folder we'll put all files and folders except the <em>public</em> dir. The <em>public</em> dir content will be placed inside the <em>public_html</em> dir. So we'll have:</p>
<ul>
<li><strong>my&#95;apps/my&#95;first_app/:</strong> here have now <em>application</em>, <em>bundles</em>, <em>laravel</em> and <em>storage</em> directories and <em>artisan</em>, <em>licence.txt</em>, <em>paths.php</em> and <em>readme.md</em> files.</li>
<li><strong>public_html/</strong>: <em>img</em>, <em>js</em>, <em>css</em>, <em>index.php</em> and <em>.htaccess</em></li>
</ul>
<h2>Editing files to say Laravel where is your new files</h2>
<p>We've changed the files paths, so, we must tell Laravel where is that files. Everything you need is tell Laravel where is the <em>paths.php</em> files, just it. So, open the <em>public_html/index.php</em> that is the old <em>public/index.php</em> file. It's content must be something like above (take a look on 24th line):</p>

{% highlight php startinline %}
<?php
/**
 * Laravel - A PHP Framework For Web Artisans
 *
 * @package  Laravel
 * @version  3.2.5
 * @author   Taylor Otwell <taylorotwell@gmail.com>
 * @link     http://laravel.com
 */

// --------------------------------------------------------------
// Tick... Tock... Tick... Tock...
// --------------------------------------------------------------
define('LARAVEL_START', microtime(true));

// --------------------------------------------------------------
// Indicate that the request is from the web.
// --------------------------------------------------------------
$web = true;

// --------------------------------------------------------------
// Set the core Laravel path constants.
// --------------------------------------------------------------
// require '../paths.php'; // CHANGE THIS to above
require '../my_apps/my_first_app/paths.php';

// --------------------------------------------------------------
// Unset the temporary web variable.
// --------------------------------------------------------------
unset($web);

// --------------------------------------------------------------
// Launch Laravel.
// --------------------------------------------------------------
require path('sys').'laravel.php';
{% endhighlight %}

<p>If you visit http://example.com you'll render the <em>index.php</em> file, that was the Laravel <em>public/index.php</em> file. So anyone can access through the http://example.com URL the <em>my_apps</em> files, because them is now outside the <em>public_html</em> dir.</p>
<h2>Summary</h2>
<ol>
<li>Get the Laravel app </li>
<li>The <em>public</em> dir content must be placed inside the <em>public_html</em> or <em>www</em> dir. </li>
<li>Everything else (application, bundle, storage, etc) must be placed outside <em>public_html</em> dir, like <em>my&#95;apps/my&#95;first_app/</em>. </li>
<li>Update the <em>public_html/index.php</em> file point the <em>paths.php</em> to it's right path now.</li>
</ol>
<p>Thanks for everyone. Thanks for reading and I hope the post is useful.</p>
