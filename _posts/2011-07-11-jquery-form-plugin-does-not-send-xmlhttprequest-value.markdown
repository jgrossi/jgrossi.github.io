---
layout: post
status: publish
published: true
title: jQuery Form plugin does not send XMLHttpRequest value

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  This will be a quick post, just for warning in a specific case. When you're using the jQuery Form plugin by <a href="http://jquery.malsup.com/form/" target="_blank">malsup.com</a> you are making an AJAX call, but it does not send some variables that identify a XMLHttpRequest (in my case I'm using the plugin with a upload form).

wordpress_id: 136
wordpress_url: http://www.juniorgrossi.com/?p=136
date: '2011-07-11 13:38:46 -0300'
date_gmt: '2011-07-11 16:38:46 -0300'
categories:
- Javascript
- jQuery
- PHP
- Zend Framework
tags: []
---
<p>Hi all...</p>
<p>Here we are, again.</p>
<p>This will be a quick post, just for warning in a specific case. When you're using the jQuery Form plugin by <a href="http://jquery.malsup.com/form/" target="_blank">malsup.com</a> you are making an AJAX call, but it does not send some variables that identify a XMLHttpRequest (in my case I'm using the plugin with a upload form).</p>
<p>For example, when we call a jQuery AJAX function like...</p>

{% highlight php startinline %}
$(function(){  
    // previous code  
    $.get(url, function(data){  
        // next code  
    });  
});  
{% endhighlight %}

<p>... it sends, for PHP, the <strong>$_SERVER['HTTP_X_REQUESTED_WITH'] = 'XMLHttpRequest'</strong> value as usually. But, if you're using the AJAX Form plugin, it *won't send* that. It will be a AJAX request (YES), like <strong>$.get</strong> but that variable will not sent, just this.</p>
<p>But why it doesn't send that? Because it uses an iframe to send the data to server, so, it's the way :-)</p>
<p>If you're using PHP Frameworks like <a href="http://framework.zend.com">Zend Framework</a> (like me) some previous actions helpers to identify an AJAX request won't work:</p>

{% highlight php startinline %}
// ...  
$this->getRequest()->isXmlHttpRequest();  
$this->_helper->getHelper('AjaxContext'); // and it's own functions won't work  
// ...  
{% endhighlight %}

<p>So, force the <strong>layout helper</strong> to disable layout (if you're using it) and <strong>viewRenderer helper</strong> to disable the view (or not, in some cases).</p>

{% highlight php startinline %}
// in your action function  
$this->_helper->layout->disableLayout();  
$this->_helper->viewRenderer->setNoRender();  
{% endhighlight %}

<p>That's all!</p>
