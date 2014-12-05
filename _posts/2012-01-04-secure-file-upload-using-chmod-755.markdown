---
layout: post
status: publish
published: true
title: Secure file upload using chmod 755

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Today I will talk about secure file upload. Please, do not use chmod 777 on yours upload files. That means everyone can write on your directory and maybe execute that file.

wordpress_id: 309
wordpress_url: http://www.juniorgrossi.com/?p=309
date: '2012-01-04 14:43:50 -0200'
date_gmt: '2012-01-04 17:43:50 -0200'
categories:
- Apache
- Hosting
- Ubuntu
tags: []
---
<p>Hello!</p>
<p>Today I will talk about secure file upload.</p>
<p>Please, do not use chmod 777 on yours upload files. That means everyone can write on your directory and maybe execute that file.</p>
<p>Use chmod 755 and be happy. For that you must have to change the directory's owner to the apache users. In the Ubuntu Linux the apache user is usually <strong>www-data</strong>, but using another versions can be <strong>apache</strong>, <strong>httpd</strong>, <strong>nobody</strong> or just <strong>www</strong>.</p>
<p>So...</p>

{% highlight php startinline %}
chown -R www-data upload_dir  
chown -R 755 upload_dir  
{% endhighlight %}

<p>See you...</p>
