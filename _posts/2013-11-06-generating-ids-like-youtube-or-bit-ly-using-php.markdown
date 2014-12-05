---
layout: post
status: publish
published: true
title: Generating IDs like Youtube or Bit.ly using PHP
excerpt: |+
  Let's supose you want to develop your own URL shortener, like <a href="http://bit.ly">Bit.ly</a> for example. You can, of course, use the ID as a integer, like, 1, 2, 3, etc. If you have 12.345 rows in your database table, you will need 5 digits, like <strong>http://example.com/12345</strong>. Large applications like Youtube, have much more entries, so, to use numbers, the URL will be very long, like <code>http://youtube.com/watch?v=231268318276783</code>.

wordpress_id: 714
wordpress_url: http://juniorgrossi.com/?p=714
date: '2013-11-06 13:38:10 -0200'
date_gmt: '2013-11-06 13:38:10 -0200'
categories:
- PHP
tags: []
---
<p>Let's supose you want to develop your own URL shortener, like <a href="http://bit.ly">Bit.ly</a> for example.</p>
<p>You can, of course, use the ID as a integer, like, 1, 2, 3, etc. If you have 12.345 rows in your database table, you will need 5 digits, like <strong>http://example.com/12345</strong>. Large applications like Youtube, have much more entries, so, to use numbers, the URL will be very long, like <code>http://youtube.com/watch?v=231268318276783</code>.</p>
<p>Because that, these websites, like <a href="http://youtube.com">YouTube</a>, <a href="http://t.co">t.co</a>, <a href="http://bitly.com">bitly.com</a> or even <a href="http://vine.co">vine.co</a>, are using a generated ID using uppercase letters, lowercase letters, digits and sometimes underscore (_) and hyphen (-). You can check that given a YouTube video URL where you will find something like <code>http://www.youtube.com/watch?v=2Z4m4lnjxkY</code>. You can see they're using the "2Z4m4lnjxkY" as ID.</p>
<p><a id="more"></a><a id="more-714"></a></p>
<h2>The Math - Base 10</h2>
<p>You have the ID in pure digits (base 10, or decimal), where you have 10 options for each "position": 0, 1, 2, 3, 4, 5, 6, 7, 8 and 9. So, with 5 positions you can theoretically represent 10&#42;10&#42;10&#42;10&#42;10 (100.000) IDs. Of course you won't represent the number 45 as 00045 or even will have the 00000 ID, but they are mathematics options. :D</p>
<h3>Base 32</h3>
<p>So, to improve the number of options you can add more chars, like lowercase letter and digits. Basically you can use a <strong>base32</strong> converter. The <strong>base32</strong> will convert a decimal number to a string using digits and lowercase letters, like <code>u63j8d</code>:</p>

{% highlight php startinline %}
<?php
// Converting the number 328743826 from base 10 to base 32 
echo base_convert(328743826, 10, 32);
{% endhighlight %}

<p>This will produce the result "9pgesi". Cool, we could reduce a lot the number of characters. But we can do better.</p>
<h3>Base 62</h3>
<p>With <strong>base32</strong> you have all digits and all lowercase letters. With <strong>base62</strong> we have also uppercase letters, generating IDs like "8H9j8sD79". So using a <strong>base62</strong> convertor we have much more options than <strong>base32</strong>. For example, with 5 positions we can have 62&#42;62&#42;62&#42;62&#42;62 = 916.132.832 options.</p>
<p>With PHP you cannot use the <code>base_convert</code> function because it only works from base 2 to base 36 and we need more, we need 62, so we have some already coded sources to do that.</p>
<p>This class was sent to me by <a href="http://twitter.com/taylorotwell">Taylor Otwell</a>, the creator of the <a href="http://laravel.com">Laravel Framework</a>. It works very well and will solve your problem to generate <strong>base62</strong> strings. You can get the code <a href="https://gist.github.com/jgrossi/a4eb21bbe00763d63385">here</a>!</p>
<h2>Example</h2>
<p>This is how you can use this class to generate your <strong>base62</strong> strings:</p>

{% highlight php startinline %}
<?php 
$my_id = 23435; // get the ID from MySQL for example;
$base62 = Math::to_base($my_id, 62);
{% endhighlight %}

<p>To get the decimal ID from the <strong>base62</strong> string you have to do:</p>

{% highlight php startinline %}
<?php
$base62 = 'b6H8Jk2';
$decimal = Math::to_base_10($base62, 62);
{% endhighlight %}

<p>You must add a new column on your database table called, for example, "base62". So every time you insert a new item you get the ID, generate the base62 string and save.</p>
<p>Be happy!</p>
