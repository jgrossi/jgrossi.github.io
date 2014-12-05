---
layout: post
status: publish
published: true
title: Thinking the Wordpress Way - First Steps

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  I work with PHP since 8 years and I never took a look on Wordpress. In the start of this year a customer requires me to develop his next website, but using the Wordpress. So I had to search some tutorials on the Internet and join informations to make my own way to think using Wordpress.

wordpress_id: 394
wordpress_url: http://juniorgrossi.com/?p=394
date: '2012-09-13 18:37:13 -0300'
date_gmt: '2012-09-13 18:37:13 -0300'
categories:
- PHP
- WordPress
tags: []
---
<p>Hello everybody!</p>
<p>I work with PHP since 8 years and I never took a look on Wordpress. In the start of this year a customer requires me to develop his next website, but using the Wordpress. So I had to search some tutorials on the Internet and join informations to make my own way to think using Wordpress.</p>
<p>In this first post I'll explain how to THINK in the Wordpress way, the files you need to customize, create and how to get posts in yours next website pages.</p>
<h2>How Wordpress works</h2>
<p>The first question I had is how to imagine a new website in the Wordpress way. It's easy, because it has only 2 ways of save data: Posts and Pages.</p>
<p>It's simple to understand. Let's take a sample website content to explain better:</p>
<ul>
<li>About Us</li>
<li>Customers</li>
<li>Services</li>
</ul>
<p><a id="more"></a><a id="more-394"></a></p>
<h3>What's Page and what's is Posts?</h3>
<p>My way of think using Wordpress is: What you have more than one item is Post and what you have just one item is Page. For example:</p>
<ul>
<li>About Us: Only a single page with some text information about the company <strong>PAGE</strong></li>
<li>Customers: Here we have 1, 2, 3 ... N customers, so we have a lot os "Post type Customers" <strong>POST</strong></li>
<li>Services: This example company do 5 types of services, so here we have "Post type Services" <strong>POST</strong></li>
</ul>
<h2>Custom Post Type</h2>
<p>Since Wordpress 3.0 we can create our own post types. There are a lot of plugins that help you to create custom post type. It's simple and easy to use. Those plugins will create a new item on the admin navigation (on the left) with the post type you want. For example: for the <strong>customers</strong> post type we'll have a new topic on the navigation called <strong>Customers</strong> and there we'll add new posts (<em>customers type</em>). You can create as many Customers you want, and we'll explain how to retrive them later.</p>
<h2>Custom Fields</h2>
<p>In Wordpress we can also create <strong>custom fields</strong> for pages and post types. So, if you want to add a image to your Customers you can do that. There is too some plugins that make this a bit simple, where you can choose the field name and the field type, like: text, textarea, file, image, radio, checkbox, etc...</p>
<p>The interesting thing is you can add custom fields to <strong>pages</strong> too. You can, for example, add a "company mission" field on the <em>About Us</em> page. What you must do is just add a field called <em>company_misson</em> in that page. Resuming, you can add what you want to pages and posts.</p>

<p>In the <a href="http://juniorgrossi.com/creating-your-first-wordpress-theme-part-1/">next post</a> I'will explain how you can start creating your first Wordpress theme, for this new website we're creating...</p>
<p>Regards.</p>
