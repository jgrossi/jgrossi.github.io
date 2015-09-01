---
layout: post
status: publish
published: true
title: Using Ruby on Rails Ajax adapter with any project, even PHP (like Laravel)
excerpt: |+
  Using Ajax with Ruby On Rails is a very easy task. You can write a link, set "remote true", write a view file with Javascript code and just it. Do you know how Ajax calls work on Rails? Maybe you can use it, inside any project: PHP, Python, you choose. And an easy way!

categories:
- Ruby On Rails
- Ajax
- PHP
- Laravel
---

Hello!!!

Every project you have to make some requests using Ajax, right? I know that [jQuery](http://jquery.com) has done a very good job and almost all project you start it is present there, together with your Javascript files. So you can make Ajax calls any time, where you want, just making the call using Javascript.

I developed some projects using [Ruby On Rails](http://rubyonrails.org) some time ago, and I start to think about using AJAX in a different way. Rails uses a custom jQuery adapter to allow you to do things like that:
	
{% highlight ruby %}
link_to("Destroy", user_path(@user), method: :delete, remote: true)
{% endhighlight %}

We can think that Rails works with Ajax in a different way, but that's not true. What the `link_to` helper function does is to only write HTML code that the jQuery adapter understands. Calling `link_to` function like above will produce the follow HTML code:
	
{% highlight html %}
<a href="/users/1" data-method="delete" data-remote="true">Destroy</a>
{% endhighlight %}

That's not magic, I know. But where's the magic happens so? Just because you have `data-remote="true"` inside the link, this will be an Ajax call in Rails, and you can return Javascript code that will be executed without refreshing the page.

## The Rails jQuery adapter

The jQuery adapter for Rails has a separated [repository](https://github.com/rails/jquery-rails) on [Github](http://github.com). You can think this adapter is complicated and has a lot of files. That's not true, and the magic happens only because one file. Yes, you need just one Javascript file to make the Ajax magic inside any project, like Rails does.

So, what do I have to do? Easy, you have to include just one Javascript file inside your project. It does not matter if it is written in PHP, Python or Perl, you can use the Rails jQuery adapter with it, just including one single JS file.

The repository you can find [here](https://github.com/rails/jquery-rails). The [JS file we need](https://github.com/rails/jquery-rails/blob/master/vendor/assets/javascripts/jquery_ujs.js) is located on the following path: 

	https://github.com/rails/jquery-rails/blob/master/vendor/assets/javascripts/jquery_ujs.js

That's the file we are looking for. Download it an just rename it to `rails.js` or even `ajax.js`, the name does not matter now. Now, include it inside your project, after where you've included jQuery:
	
{% highlight html %}
<script type="text/javascript" src="assets/js/jquery.js"></script>
<script type="text/javascript" src="assets/js/rails.js"></script>
{% endhighlight %}

Ok! What do you have to do now? Just right the HTML code the adapter can understand. If you are working with [Laravel](http://laravel.com), for example, you can do this:
	
{% highlight html %}
<a href="<?php echo url('users/1', ['data-method' => 'delete', 'date-remote' => true]) ?>">Destroy</a>
<!--<a href="/users/1" data-method="delete" data-remote="true">Destroy</a>-->
{% endhighlight %}

### Server response with Javascript, like Rails

Maybe you want to set all Ajax calls to return a Javascript code, like Rails does. By default, the jQuery Rails adapter expect (with Ajax calls) HTML code, but that's easy to change.

What you have to do is to say to jQuery adapter you want to use Javascript type in your server response. So you can just include inside our `rails.js` (or the name you gave to it) the line with `$.ajaxSettings.dataType` like that:
	
{% highlight javascript %}
(function($, undefined) {

/**
 * Unobtrusive scripting adapter for jQuery
 * https://github.com/rails/jquery-ujs
 *
 * Requires jQuery 1.8.0 or later.
 *
 * Released under the MIT license
 *
 */
  
  // ADD THIS LINE
  $.ajaxSettings.dataType = "script";

  // Cut down on the number of issues from people inadvertently including jquery_ujs twice
  // by detecting and raising an error when it happens.
  if ( $.rails !== undefined ) {
    $.error('jquery-ujs has already been loaded!');
  }
  {% endhighlight %}

This will say to the jQuery adapter it must expect Javascript code as response by default and you want to execute that. That's amazing! But if you want some call to return a JSON response for example, just set `data-type="json"` on the link and the adapter will do the job for you. 

-- Get the changed file [here](https://gist.github.com/jgrossi/ba299b6f96383d25242b)!!! --

As you can see on the line #102 (my file), the file will look first for some `data-type` attribute inside your HTML code, and after the `$.ajaxSettings.dataType` we've set.

For example, using Laravel you can return a View file as a Javascript file:
	
	{% highlight php %}
	<?php
// Removing a user inside Controller
$user = User::find(1);
$user->delete();

return view('user.delete', compact('user'));
{% endhighlight %}

Ok, that's nothing new, but the content of the `profile` file now can be Javascript, because our adapter is looking for that type of response:
	
{% highlight php %}
alert("The user <?php echo $user->name ?> was removed from our site.");
{% endhighlight %}

And when you click the "Destroy" link you will have an `alert` with the message as the server response ;-)

### Others goodies

In addition you can use some others goodies the Rails jQuery adapter has, like setting `data-method` to work with Restful applications (get, post, delete, put), the `data-confirm` to show the Javascript dialog asking you to confirm some question, and more, like that:
	
{% highlight html %}
<a href="http://example.com" data-method="delete" data-confirm="Do you want to remove this user?">Destroy</a>
{% endhighlight %}

Thanks for reading! See you!!

