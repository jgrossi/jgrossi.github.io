---
layout: post
status: publish
published: true
title: Migrating emails using IMAP (imapsync) to/from (Gmail, Yahoo, etc)

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  I'll try use the new Yahoo! Mail for a while but I need my old emails messages. I am a Gmail user and want to copy everything to Yahoo! Mail. After search a lot at Google I found <strong>imapsync</strong>. It is a Linux program that runs on the command line and can connect to a IMAP server and copy to another one.
  
wordpress_id: 497
wordpress_url: http://juniorgrossi.com/?p=497
date: '2013-01-18 16:46:02 -0200'
date_gmt: '2013-01-18 16:46:02 -0200'
categories:
- Uncategorized
- Linux
- Ubuntu
tags: []
---
<p>Hi all!</p>
<p>Last update <a href="#20140425"><strong>April 25 2014</strong></a>!</p>
<p>I'll try use the new Yahoo! Mail for a while but I need my old emails messages. I am a Gmail user and want to copy everything to Yahoo! Mail. After search a lot at Google I found <strong>imapsync</strong>. It is a Linux program that runs on the command line and can connect to a IMAP server and copy to another one.</p>
<h1>Linux Machine</h1>
<p>I am a MAC user, so I don't have Linux machines except servers I don't want to install things I'll use just once. So I created a Ubuntu 12.10 64 bits droplet at <a href="http://digitalocean.com">Digital Ocean</a> and install dependencies and start to sync my emails messages. First we must install dependencies and after install the <strong>imapsync</strong>. Take a look:</p>

{% highlight php startinline %}
apt-get update
apt-get install libdigest-hmac-perl libdigest-hmac-perl libterm-readkey-perl libterm-readkey-perl libdate-manip-perl libdate-manip-perl libmail-imapclient-perl
{% endhighlight %}

<p><a id="more"></a><a id="more-497"></a></p>
<p>Now lets install <strong>imapsync</strong> downloading the .deb file (version 1.315):</p>

{% highlight php startinline %}
wget http://old-releases.ubuntu.com/ubuntu/pool/universe/i/imapsync/imapsync_1.315+dfsg-1_all.deb
dpkg -i imapsync_1.315+dfsg-1_all.deb
{% endhighlight %}

<p>As I want to backup Gmail messages into Yahoo! Mail I must do the follow:</p>

{% highlight php startinline %}
imapsync --host1 imap.gmail.com --user1 username@gmail.com --password1 ******** --host2 imap.mail.yahoo.com --user2 username@yahoo.com --password2 ******* --syncinternaldates --ssl1 -ssl2 --noauthmd5 --split1 100 --split2 100 --port1 993 --port2 993 --exclude "All Mail|Spam|Trash" --allowsizemismatch
{% endhighlight %}

<h2>Gmail to Google Apps</h2>
<p>If you want to migrate a Gmail account to a Google Apps account this will solve your problem. It worked like a charm for me:</p>

{% highlight php startinline %}
imapsync --host1 imap.gmail.com --port1 993 --user1 you@gmail.com --password1 ****** --ssl1 --host2 imap.gmail.com --port2 993 --user2 you@domain.com --password2 ****** --ssl2 --syncinternaldates --split1 100 --split2 100 --authmech1 LOGIN --authmech2 LOGIN --allowsizemismatch --useheader Message-ID
{% endhighlight %}

<p>Comments about some parameters:</p>
<ul>
<li><strong>split1 100 and split2 100:</strong> Number of messages requests. The default value is 1000 but 100 is a good choice.</li>
<li><strong>allowsizemismatch:</strong> This options ignore different messages size. Is not present a lot os messages will be ignored because different size between both mail servers.</li>
<li><strong>useheader Message-ID:</strong> This option is necessary when transferring large folders. If not present the process will be killed and won't be completed. Now you must wait. If you want you can copy the entire command to another one, like </li>
</ul>
<p><strong>run_imapsync</strong> and give it <em>execute</em> privilegies.</p>
<p>If you also want to let the process running, just run the following and be happy!</p>

{% highlight php startinline %}
./run_imapsync >> /dev/null &amp;
{% endhighlight %}

<p>The <strong>&amp;</strong> will run the process in background mode.</p>
<h1>UPDATE Jan 20, 2013</h1>
<p>Previously on this post I was using the version 1.315 of <em>imapsync</em>. But there is newer versions that are better than 1.315, that crashes sometimes without reason. Today (Jan 20, 2013), I found the version 1.518 that has some improves. You'll find the <strong>imapsync</strong> in this site: <a href="http://imapsync.lamiral.info/">http://imapsync.lamiral.info/</a>, but you must to pay to download the newer version. BUT <strong>imapsync</strong> is for free in GitHub. Lets install the newer version (1.518). First we have to install another dependencies in Ubuntu (my Linux server):</p>

{% highlight php startinline %}
apt-get install makepasswd rcs perl-doc libmail-imapclient-perl make
{% endhighlight %}

<p>After you must clone the GIT repository from Github. The URL is https://github.com/imapsync/imapsync. First you have to install git (if not already installed):</p>

{% highlight php startinline %}
apt-get install git-core git-doc git-svn git-gui gitk
{% endhighlight %}

<p>Now we can clone the GIT repository:</p>

{% highlight php startinline %}
git clone git://github.com/imapsync/imapsync.git
{% endhighlight %}

<p>After that, enter inside the <strong>imapsync</strong> dir created and run <strong>make install</strong>:</p>

{% highlight php startinline %}
cd imapsync
make install
{% endhighlight %}

<p>Ok! You'll se something like this:</p>

{% highlight php startinline %}
root@server:~/imapsync# make install
perl -c imapsync
imapsync syntax OK
pod2man imapsync > imapsync.1
mkdir -p /usr/bin
install imapsync /usr/bin/imapsync
chmod 755 /usr/bin/imapsync
mkdir -p /usr/share/man/man1
install imapsync.1 /usr/share/man/man1/imapsync.1
chmod 644 /usr/share/man/man1/imapsync.1
{% endhighlight %}

<p>Now you have the newer version installed (1.518):</p>

{% highlight php startinline %}
imapsync -v
{% endhighlight %}

<h1 id="20140425">UPDATE Apr 25, 2014</h1>
<p>Today I started a migration from Google Apps to Gmail and had some problems with my own post :-)</p>
<p>I created a Digital Ocean droplet with Ubuntu 12.04 64 bits and I installed everything using <code>apt-get</code> (check all needed packages on this hole post). After that I've cloned the GitHub repo and run <code>make install</code>. That's the problem. I have a problem like:</p>

{% highlight php startinline %}
Can't locate File/Copy/Recursive.pm in @INC (@INC contains: ./W/Mail-IMAPClient-3.35/lib /etc/perl /usr/local/lib/perl/5.14.2 /usr/local/share/perl/5.14.2 /usr/lib/perl5 /usr/share/perl5 /usr/lib/perl/5.14 /usr/share/perl/5.14 /usr/local/lib/site_perl .) at ./imapsync line 568.
BEGIN failed--compilation aborted at ./imapsync line 568.
{% endhighlight %}

<p>So, we need here the 'file-copy-recursive' from Perl. It's easy to install it:</p>

{% highlight php startinline %}
apt-get install libfile-copy-recursive-perl
{% endhighlight %}

<p>Just a tip! Let's check if your Perl is with all packages Imapsync need:</p>

{% highlight php startinline %}
perl -c imapsync
{% endhighlight %}

<p>If you see the follow result everything is ok:</p>

{% highlight php startinline %}
imapsync syntax OK
{% endhighlight %}

<p>Now you can go inside the imapsync folder (cloned from GitHub), and run:</p>

{% highlight php startinline %}
make install
{% endhighlight %}

<p>Thanks for reading!</p>
