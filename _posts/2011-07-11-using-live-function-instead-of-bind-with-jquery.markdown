---
layout: post
status: publish
published: true
title: Using live function instead of bind with jQuery

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Sometimes a big part of an application uses AJAX to get content. When getting new content we get new events, already declared in the webapp.

wordpress_id: 126
wordpress_url: http://www.juniorgrossi.com/?p=126
date: '2011-07-11 11:55:04 -0300'
date_gmt: '2011-07-11 14:55:04 -0300'
categories:
- Javascript
- jQuery
tags: []
---
<p>Hi all,</p>
<p>Sometimes a big part of an application uses AJAX to get content. When getting new content we get new events, already declared in the webapp. For example:</p>

{% highlight php startinline %}
$(function(){  
    $('a.class1').bind('click', function(){  
        // code here  
    });  
}); 
{% endhighlight %}

<p>If the new content (via AJAX) has a link with <em>class1</em> class, it won't call this javascript code, because the <strong>click</strong> event was previously declared.</p>
<p>So, the solution is use <strong>live</strong> function instead of <strong>bind</strong>. When using the live function, the new inserted content will be updated to use the declared javascript events. So it's better to use <strong>live</strong>.</p>

{% highlight php startinline %}
$(function(){  
    $('a.class1').live('click', function(){  
        // code here  
    });  
});  
{% endhighlight %}

<p>Thanks!</p>
