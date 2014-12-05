---
layout: post
status: publish
published: true
title: Deploying Zend Framework applications on a shared host

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Usually I have direct access (shell) in all the servers that host my Zend Framework applications, but, some clients already have a shared host, so I have to deploy the application there too. I was thinking how can I never thought this before! There are a lot of ways to deploy a Zend Framework application, but in a shared host we can't have access to create Virtual Hosts on Apache.

wordpress_id: 237
wordpress_url: http://www.juniorgrossi.com/?p=237
date: '2011-07-19 21:20:46 -0300'
date_gmt: '2011-07-20 00:20:46 -0300'
categories:
- Hosting
- PHP
- Zend Framework
tags:
- hosting
- php
- zend framework
- zf
---
<p>Hello everybody!</p>
<p>Usually I have direct access (shell) in all the servers that host my Zend Framework applications, but, some clients already have a shared host, so I have to deploy the application there too.</p>
<p>I was thinking how can I never thought this before! There are a lot of ways to deploy a Zend Framework application, but in a shared host we can't have access to create Virtual Hosts on Apache.</p>
<p><a id="more"></a><a id="more-237"></a></p>
<h2>Folder structure</h2>
<p>On a shared host you have your own folder, which you can upload your PHP files. That's my client's folder structure:</p>
<p><img src="http://grossi.local/wp-content/uploads/2011/07/folder_structure.gif" alt="" title="Folder Structure" class="aligncenter size-full wp-image-244" /></p>
<p>Take a look on the <strong>public_html</strong> folder. It's the folder where usually we upload our PHP files. Some hosts call it <strong>htdocs</strong> too.</p>
<h3>Zend Framework's structure</h3>
<p>Basically, the default ZF's structure has 5 folders: <strong>application</strong>, <strong>docs</strong>, <strong>library</strong>, <strong>public</strong> and <strong>tests</strong>.</p>
<h2>Uploading</h2>
<p>Inside ZF's <strong>public</strong> folder we have images, <strong>javascript</strong> files, <strong>css</strong> files, etc, all that have direct access through url. In other words, we put only the <strong>public</strong> folder's content in the <strong>public_html</strong> folder on our shared host.</p>
<p>Let's create another folder and let's call it <strong>php_apps</strong>. Inside it let's create another folder, to host our application files (<strong>app1</strong> folder). That folder will receive all the ZF's folders (except <strong>public</strong> folder, of course).</p>
<p>So let's put our <strong>application</strong>, <strong>docs</strong>, <strong>library</strong> and <strong>tests</strong> folder inside it.</p>
<p><img src="http://grossi.local/wp-content/uploads/2011/07/all_folder.gif" alt="" title="new_structure" class="aligncenter size-full wp-image-258" /></p>
<p>New, all we need is tell the <strong>index.php</strong> to access the new folder. Let's edit it (line 4):</p>

{% highlight php startinline %}
<?php

// Define path to application directory  
defined('APPLICATION_PATH') || define('APPLICATION_PATH', realpath(dirname(__FILE__) . '/../php_apps/app1/application'));

// Define application environment  
defined('APPLICATION_ENV') || define('APPLICATION_ENV', (getenv('APPLICATION_ENV') ? getenv('APPLICATION_ENV') : 'production'));

// Ensure library is on include_path  
set_include_path(implode(PATH_SEPARATOR, array(  
    realpath(APPLICATION_PATH . '/../library'),  
    get_include_path(),  
)));

/** Zend_Application */  
require_once 'Zend/Application.php';

// Create application, bootstrap, and run  
$application = new Zend_Application(  
    APPLICATION_ENV,  
    APPLICATION_PATH . '/configs/application.ini'  
);  

$application->bootstrap()->run();
{% endhighlight %}

<p>Now, accessing your <strong>public_html</strong> folder (or http://www.domain.com) we'll call the <strong>index.php</strong> file, that calls our <strong>application</strong> folder (inside <strong>php_apps/app1</strong>).</p>
<p>To work with images, css, etc, in your view files, is exactly the same:</p>

{% highlight php startinline %}
<?php echo $this->baseUrl('/img/my_image.gif'); ?>
{% endhighlight %}

<p>That's all! Thanks!</p>
