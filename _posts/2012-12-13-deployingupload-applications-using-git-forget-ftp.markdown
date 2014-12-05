---
layout: post
status: publish
published: true
title: Deploying/Upload applications using GIT (forget FTP)

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Today I'll talk a practice that changed my life. Since when I work developing web applications I used to work with FTP to send files to server. Forget using FTP for that and welcome to the GIT world. GIT became famous after the launch of GitHub website. GIT is a version control tool used to manage applications (or files) and control users files update and much more.

wordpress_id: 481
wordpress_url: http://juniorgrossi.com/?p=481
date: '2012-12-13 13:26:21 -0200'
date_gmt: '2012-12-13 13:26:21 -0200'
categories:
- Ubuntu
- Git
tags: []
---
<p>Hi!</p>
<p>Today I'll talk a practice that changed my life. Since when I work developing web applications I used to work with FTP to send files to server. Forget using FTP for that and welcome to the GIT world.</p>
<p>GIT became famous after the launch of GitHub website. GIT is a version control tool used to manage applications (or files) and control users files update and much more.</p>
<p>Basically you must have:</p>
<ul>
<li>GIT installed on the remote machine and local machine</li>
<li>Repositories created on both machines</li>
<li>The files you want to deploy/upload</li>
</ul>
<p><a id="more"></a><a id="more-481"></a></p>
<p>For now you'll use Ubuntu Linux on both machines to deploy our sample application. First we need to install GIT:</p>
<h3>Remote Machine</h3>
<ol>
<li>SSH your remote machine: <code>ssh user@yourdomain_or_ip</code></li>
<li>Install GIT on remote machine: <code>sudo apt-get install git-core</code></li>
<li>Go to path where you'll deploy files: <code>cd /home/user/my_project</code></li>
<li>Inside this folder create an empty GIT repository: <code>git init</code></li>
</ol>
<h3>Local Machine</h3>
<ol>
<li>Install GIT if not installed: <code>sudo apt-get install git-core</code></li>
<li>Go to your application path: <code>cd /home/user/project</code></li>
<li>Create an GIT repository with the files you already have: <code>git init</code></li>
</ol>
<p>Ok! We have both GIT repositories created. Assuming you have an application developed (local machine) you want to deploy, we have to add the files to the repository and deploy to the remote GIT repository:</p>
<h3>Local Machine</h3>
<ol>
<li>Add all new/updated files to the local repository: <code>git add .</code> </li>
<li>Do your first commit to repository: <code>git commit -m "first commit"</code> </li>
<li>Now we've to create a new branch (because your remote repository will not allow you to deploy to the master branch): <code>git checkout -b devel</code> </li>
<li>Now we're inside 'devel' branch. So let's register where is our "remote server": <code>git remote add origin user@domain_or_ip:/home/user/my_project</code> </li>
<li>And deploy files: <code>git push origin devel</code> </li>
</ol>
<p>We have now all files deployed on the remote machine, BUT insite the "devel" branch of the created GIT repository. So we have to login on the remote machine and tell the GIT to MERGE the "master" branch with the "devel" branch (that we've deployed now).</p>
<h3>Remote Machine</h3>
<ul>
<li><code>ssh user@domain_or_ip</code></li>
<li><code>cd /home/user/my_project</code></li>
<li><code>git merge devel</code></li>
</ul>
<p>All right! Your files are all them inside the "master" branch on the remote GIT repository. Easy! Note that transfer files using GIT is faster than FTP and much more "professional".</p>
<p>Thanks!</p>
