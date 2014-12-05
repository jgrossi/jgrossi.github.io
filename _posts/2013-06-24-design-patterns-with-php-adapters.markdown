---
layout: post
status: publish
published: true
title: Design Patterns with PHP - Adapters

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Today I am starting a post series about Design Patterns. I have wrote about them a lot but only suggesting you to learn about to be a better developer. I am studying them, so nothing better to write about and improve my knowledges too. I only ask you to read everything to understand the concepts behind the patterns. You have to understand them to know where you can use one or another.

wordpress_id: 630
wordpress_url: http://juniorgrossi.com/?p=630
date: '2013-06-24 03:08:29 -0300'
date_gmt: '2013-06-24 03:08:29 -0300'
categories:
- PHP
- Design Patterns
tags: []
---
<p>Hi all!</p>
<p>Today I am starting a post series about Design Patterns. I have wrote about them a lot but only suggesting you to learn about to be a better developer. I am studying them, so nothing better to write about and improve my knowledges too.</p>
<p>I only ask you to read everything to understand the concepts behind the patterns. You have to understand them to know where you can use one or another.</p>
<p><a id="more"></a><a id="more-630"></a></p>
<h2>The Adapter Pattern</h2>
<p>The Adapter Pattern is exactly what it is, an adapter. It works to allow you to adapt your code to a new requirement that did not exist before "now".</p>
<p>Let's suppose you are working on a project where you have a website and have to allow users to write on the company's Twitter profile. You can have the follow situation:</p>
<ul>
<li><strong>Post.php:</strong> Your post class. This is the Post object, where you have text and a URL, for example.</li>
<li><strong>Twitter.php:</strong> The Twitter class. It can be a class created by you or for example a class you got in Packagist website.</li>
</ul>
<p>Good. So, you could write a code like that:</p>

{% highlight php startinline %}
<?php 

// creating a post and posting to Twitter
$post = new Post();
$post->description = 'My first post to Twitter. Just for fun!';
$post->url = 'http://juniorgrossi.com';
$post->send();

// inside Post class
class Post
{
    // ... code and more code

    public function send()
    {
        $text = $this->description . ' ' . $this->url;
        // some Twitter class
        $twitter = new Twitter();
        // authenticate and more ...
        $twitter->tweet($text);
    }
}
{% endhighlight %}

<p>Ok! That works! Solve your problem perfectly, so it is a very good approach.</p>
<p>But now someone tells you that you have to change the code and choose to post to Twitter or Instagram. Ok! It's easy!</p>

{% highlight php startinline %}
<?php 

// changing the Post class
class Post
{
    public function send($service = 'twitter')
    {
        if ($service == 'twitter') {
            $twitter = new Twitter();
            // ...
            $twitter->tweet();
        } elseif ($service == 'instagram') {
            $instagram = new Instagram();
            // ...
            $instagram->postToInstagram();
        }
    }
}
{% endhighlight %}

<p>Ok! It works! But you can do better than that. Now it is the opportunity to you to use a pattern, the <strong>Adapter Pattern</strong>. You can adapt you code to a generally solution, just changing the service you want. If you have now to include Facebook too, you will do that easy, and not change the <code>send()</code> method again and include one more <code>if</code> condition. Do that is a very bad idea!</p>
<h3>Creating the Service Interface</h3>
<p>Here you have 2 different services: Twitter and Instagram. The first one uses a method to "post" called <code>tweet()</code> and the second another method called <code>postToInstagram()</code>. First you have to create a pattern, with one method that will be responsable to post to the service, don't matter what is. It's the chance you have to create a <strong>Interface</strong>. Take a look!</p>

{% highlight php startinline %}
interface ServiceInterface
{
    public function authenticate(array $options);

    public function post($text, $url);
}
{% endhighlight %}

<p>Just that. Here is the secret of your developer's life! Use <strong>Interfaces</strong> for everything! This interface has, for example, two methods: one to authenticate in the service and another to post the text to the service. Now you will create two more classes (pay attention them implements the interface you've created):</p>

{% highlight php startinline %}
class TwitterService implements ServiceInterface
{
    protected $service;

    public function __construct()
    {
        $this->service = new Twitter();
    }

    public function authenticate(array $options)
    {
        $apiKey = $options['api_key'];
        // ...
        $this->service->authenticateUsingSomeMethod($apiKey);
        // ...
    }

    public function post($text, $url)
    {
        // ...
        $this->service->tweet($text . ' ' . $url);
        // ...
    }
}

class InstagramService implements ServiceInterface
{
    protected $service;

    public function __construct()
    {
        $this->service = new Instagram();
        $this->service->someAnotherMethodYouHaveToCall();
    }

    public function authenticate(array $options)
    {
        // ...
        $this->service->authenticateWithInstagramClass();
        // ...
    }

    public function post($text, $url)
    {
        // ... 
        $this->service->postToInstagram($text);
        // ...
    }
}
{% endhighlight %}

<p>And let's change the <code>Post.php</code> class for the last time :D</p>

{% highlight php startinline %}
class Post
{
    protected $service;

    public function setServiceAdapter(ServiceInterface $service)
    {
        $this->service = $service;
    }

    public function send()
    {
        $this->service->post($this->description, $this->url);
    }
}
{% endhighlight %}

<p>Now, to use the Post class you have to instantiate, set <code>description</code> and <code>url</code> properties, provide the <code>ServiceInterface</code> object and call the <code>send()</code> method:</p>

{% highlight php startinline %}
// creating a post and posting to Twitter, Instagram or Facebook
$post = new Post();

// if want to use Twitter
$post->setServiceAdapter(new TwitterService()); // OR

// if want to use Instagram
$post->setServiceAdapter(new InstagramService()); // OR maybe

// if want to use Facebook or another social network adapter
$post->setServiceAdapter(new FacebookService());

$post->description = 'My first post to Twitter. Just for fun!';
$post->url = 'http://juniorgrossi.com';
$post->send();
{% endhighlight %}

<p>This way you can create new adapters without change your <code>Post</code> code. Your <code>Post</code> class use a <strong>Interface</strong> and only classes that implements that interface will have those methods, the methods that <code>Post</code> class uses to send a post.</p>
<p>That's all! I wish explain a little about the <strong>Adapter Pattern</strong>. If you can improve this post with another example or correcting me with something please tell me. You are welcome to contribute and share!</p>
<p>Thanks for reading!</p>
