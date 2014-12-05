---
layout: post
status: publish
published: true
title: Creating new Zend Framework project using Zend_Tool

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Another quick tip. To start a new Zend Framework project, the easiest way is using Zend&#95;Tool for that. Please, install first the Zend&#95;Tool follow <a href="http://framework.zend.com/manual/en/zend.tool.usage.cli.html" title="Zend_Tool usage">this link at Zend Framework documentation page</a>.
wordpress_id: 292
wordpress_url: http://www.juniorgrossi.com/?p=292
date: '2011-12-13 15:03:50 -0200'
date_gmt: '2011-12-13 18:03:50 -0200'
categories:
- PHP
- Zend Framework
tags:
- project
- zend framework
- zend_tool
---
<p>Hello.</p>
<p>Another quick tip. To start a new Zend Framework project, the easiest way is using Zend&#95;Tool for that. Please, install first the Zend&#95;Tool follow <a href="http://framework.zend.com/manual/en/zend.tool.usage.cli.html" title="Zend_Tool usage">this link at Zend Framework documentation page</a>.</p>
<h3>Creating new project</h3>
<p>Let's starting using the command line and creating a new ZF project.</p>

{% highlight php startinline %}
zf create project my_project_name
{% endhighlight %}

<h3>Zend_Layout</h3>
<p>Now, get in the project folder and enable Zend_Layout support:</p>

{% highlight php startinline %}
cd my_project_name
zf enable layout
{% endhighlight %}

<p><a id="more"></a><a id="more-292"></a></p>
<h3>Database</h3>
<p>Configuring database support:</p>

{% highlight php startinline %}
zf configure db-adapter "hostname=localhost&amp;adapter=pdo\_mysql&amp;dbname=my\_project&amp;password=12345&amp;username=myuser"  
{% endhighlight %}

<p>And some DbTable:</p>

{% highlight php startinline %}
zf create db-table posts
{% endhighlight %}

<p>Using Zend_Tool we have a lot of options do create files (DbTable, models, controllers, actions, etc) using the command line.</p>
<h3>All options</h3>
<p>Type <strong>zf</strong> and press ENTER.</p>

{% highlight php startinline %}
Zend Framework Command Line Console Tool v1.11.9  
Usage:  
zf \[--global-opts] action-name [--action-opts] provider-name [--provider-opts\] \[provider parameters ...\]  
Note: You may use "?" in any place of the above usage string to ask for more specific help information.  
Example: "zf ? version" will list all available actions for the version provider.

Providers and their actions:  
Version  
zf show version mode[=mini] name-included[=1]  
Note: There are specialties, use zf show version.? to get specific help on them.

Config  
zf create config  
zf show config  
zf enable config  
Note: There are specialties, use zf enable config.? to get specific help on them.  
zf disable config  
Note: There are specialties, use zf disable config.? to get specific help on them.

Phpinfo  
zf show phpinfo

Manifest  
zf show manifest

Profile  
zf show profile

Project  
zf create project path name-of-profile file-of-profile  
zf show project  
Note: There are specialties, use zf show project.? to get specific help on them.

Application  
zf change application.class-name-prefix class-name-prefix

Model  
zf create model name module

View  
zf create view controller-name action-name-or-simple-name module

Controller  
zf create controller name index-action-included[=1] module

Action  
zf create action name controller-name[=Index] view-included[=1] module

Module  
zf create module name

Form  
zf enable form module  
zf create form name module

Layout  
zf enable layout  
zf disable layout

DbAdapter  
zf configure db-adapter dsn section-name[=production]

DbTable  
zf create db-table name actual-table-name module force-overwrite  
Note: There are specialties, use zf create db-table.? to get specific help on them.

ProjectProvider  
zf create project-provider name actions 
{% endhighlight %}

