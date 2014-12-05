---
layout: post
status: publish
published: true
title: Pre and post database operations with Zend Framework using Zend_Db_Table_Row

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Today I have a quick tip for beginners using Zend Framework.\n\n\n\nDo not insert pre and post code (for database) in your
  Controller. The Zend&#95;Db&#95;Table_Row is for that. 
  
wordpress_id: 281
wordpress_url: http://www.juniorgrossi.com/?p=281
date: '2011-12-13 14:41:39 -0200'
date_gmt: '2011-12-13 17:41:39 -0200'
categories:
- Database
- PHP
- Zend Framework
tags:
- database
- zend framework
- zend_db
- zend_db_table
- zend_db_table_row
---
<p>Hi all.</p>
<p>Here I am again.</p>
<p>Today I have a quick tip for beginners using Zend Framework.</p>
<p>Do not insert pre and post code (for database) in your Controller. The Zend&#95;Db&#95;Table_Row is for that.</p>
<p>Lets create our DatabaseTable class for Posts:</p>

{% highlight php startinline %}
/**  
* Located in .../models/DbTable/Posts.php  
*/  
class Posts extends Zend\_Db\_Table_Abstract  
{
    protected $_primary = 'id';  
    protected $_name = 'posts';  
    protected $_rowClass = 'Post'; // here we linked the Post class above  
}
{% endhighlight %}

<p><a id="more"></a><a id="more-281"></a></p>
<p>Now, we have to create Post class using Zend&#95;Db&#95;Table_Row:</p>

{% highlight php startinline %}
/**  
* Located in .../models/Post.php  
*/  
class Post extends Zend\_Db\_Table\_Row\_Abstract  
{

    protected function _insert() // before insert  
    {  
        $this->postDate = date('c');  
        // ...  
    }

    protected function _postInsert() // after insert  
    {  
        // send mail, for example  
    }

    protected function _update() // before update  
    {  
        // I can access a database field  
        // for example -> if the user changed the title using a form  
        // I can access the old and new title...  
        $oldTitle = $this->_cleanData['title']; // what I have in the database  
        $newTitle = $this->title; // sent by user  
    }

    // we also have

    protected function _postUpdate(); // post update  
    protected function _delete(); // before delete  
    protected function _postDelete(); // after delete  
}
{% endhighlight %}

<p>And... in your controller:</p>

{% highlight php startinline %}
$postsDT = new Posts();  
$newRow = $postsDT->createRow($form->getValues());  
$newRow->save(); // add the postDate field using Zend\_Db\_Table_Row -> Post.php  
{% endhighlight %}

<p>That's all. Thanks!</p>
