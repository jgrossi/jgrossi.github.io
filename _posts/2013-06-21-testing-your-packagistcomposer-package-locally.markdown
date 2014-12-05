---
layout: post
status: publish
published: true
title: Testing your Packagist/Composer package locally

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  I am developing a project that uses a package I'm developing too. So, it is a real time test project. I find a new way to do something and write it inside my package and uses that. But sometimes this is a boring work. I am sharing my package in the Packagist website using Composer, and everything I change I have to commit, push to GitHub and update my project with Composer, to get the new code I've wrote. That's a very very bad idea.

wordpress_id: 625
wordpress_url: http://juniorgrossi.com/?p=625
date: '2013-06-21 15:15:19 -0300'
date_gmt: '2013-06-21 15:15:19 -0300'
categories:
- PHP
- Composer
tags: []
---
<p>Hello again! It's my second post today! I'm electric!</p>
<p>I am developing a project that uses a package I'm developing too. So, it is a real time test project. I find a new way to do something and write it inside my package and uses that.</p>
<p>But sometimes this is a boring work. I am sharing my package in the Packagist website using Composer, and everything I change I have to commit, push to GitHub and update my project with Composer, to get the new code I've wrote. That's a very very bad idea.</p>
<p>Yesterday I was talking with my friend <a href="http://twitter.com/diegoholiveira">@diegoholiveira</a> about to test the package locally and we've found a easy solution. You can change the package you are developing and use that in the project that uses it. We were trying to find an easy way to do that using Composer, but the solution was easier than that. Just use symbolic links.</p>
<p><a id="more"></a><a id="more-625"></a></p>
<h2>Structure</h2>
<p>Ok. Let's assume you are developing a website. This website have to generate passwords with some particularities. So you decide to develop a package to provide this password generation and use it inside the website project. So you have 2 distinct projects:</p>

{% highlight php startinline %}
/home/me/projects/website
/home/me/projects/password-generator
{% endhighlight %}

<p>Let's assume your <strong>password-generator</strong> is a package inside the "yourname" vendor. So the package name will be:</p>

{% highlight php startinline %}
yourname/password-generator
{% endhighlight %}

<p>First you have to have installed at least a basic version of your package. You can submit a very basic package to Packagist, where you will install from. You add the package information inside your <code>composer.json</code> file and <code>composer install</code> or <code>composer update</code>.</p>
<p>Good. If you were installing this package inside your <strong>website</strong> project you will have the follow structure using Composer:</p>

{% highlight php startinline %}
/home/me/projects/website/vendor/myname/password-generator/*.*
{% endhighlight %}

<p>But you already have you code inside:</p>

{% highlight php startinline %}
/home/me/projects/password-generator
{% endhighlight %}

<p>So, create a symbolic link from your package to your website:</p>

{% highlight php startinline %}
ln -s /home/me/projects/password-generator /home/me/projects/website/vendor/myname/password-generator
{% endhighlight %}

<p>After that you must update the Composer autoload to find that new files. Inside your /website folder:</p>

{% highlight php startinline %}
composer dump-autoload
{% endhighlight %}

<p>Ready! Everything you change in the package <strong>password-generator</strong> you will can use inside your <strong>website</strong> project. If you created some new class, maybe you must run <code>composer dump-autoload</code> again to update files.</p>
<p>When you want, just commit, push your package to GitHub and share!</p>
