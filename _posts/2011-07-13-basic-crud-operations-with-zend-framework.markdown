---
layout: post
status: publish
published: true
title: Basic CRUD operations with Zend Framework

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Here I am again. Yesterday a friend asked me how to create the basics <a href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete">CRUD</a> (Create, Release, Update, Delete) operations using <a href="http://framework.zend.com">Zend Framework</a>. As everybody now, a basic CRUD it's a good way to understand some framework's resources to a new ZF developer. As he is a newbie with Zend Framework, the example will be simpler, but complete.

wordpress_id: 166
wordpress_url: http://www.juniorgrossi.com/?p=166
date: '2011-07-13 18:57:45 -0300'
date_gmt: '2011-07-13 21:57:45 -0300'
categories:
- PHP
- Zend Framework
tags:
- crud
- php
- zend framework
- zf
---
<p>Hi everyone,</p>
<p>Here I am again. Yesterday a friend asked me how to create the basics <a href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete">CRUD</a> (Create, Release, Update, Delete) operations using <a href="http://framework.zend.com">Zend Framework</a>. As everybody now, a basic CRUD it's a good way to understand some framework's resources to a new ZF developer. As he is a newbie with Zend Framework, the example will be simpler, but complete.</p>
<p>I think this example resumes a lot of things of a basic usage of Zend Framework, like controllers, models - not exactly ;-), views and the magic Zend_Form, with form validation, filters and more.</p>
<p>Our application context will be a simple Blog using <a href="http://www.mysql.com">MySQL</a> as database. Why blog again? Because it's simple to understand, just it. Our blog application will have just the <strong>posts</strong> table, to show only the CRUD funcionallity. Comments and files can be reason to a future post.</p>
<p><a id="more"></a><a id="more-166"></a></p>
<p>We won't worry about CSS or visual effects, even less Javascript or Ajax . It's a simple post to explain a basic operation with the framework. Another resources will be subject to a new post. Let's go!</p>
<h3>Creating the Blog project</h3>
<p>We'll create the new blog project using the <strong>zf</strong> command line. For details using the <strong>command line</strong> see <a href="http://framework.zend.com/manual/en/zend.tool.usage.cli.html">Using Zend_Tool On The Command Line</a>.</p>

{% highlight php startinline %}
zf create project blog  
{% endhighlight %}

<p><a href="http://framework.zend.com/download/latest">Download Zend Framework</a> and copy the <strong>ZendFramework-1.11.8-minimal/library/Zend</strong> folder to your <strong>library</strong> folder, inside the Blog project.</p>
<p>You can share a Zend Framework folder just adding a new <strong>include_path</strong> inside <strong>index.php</strong>.</p>
<h3>Configuring the database on the <strong>Bootstrap.php</strong> file</h3>
<p>I don't like much configuration files. So, I prefer to setup the database on my <strong>Bootstrap.php</strong> file. Let's go.</p>
<p>PS: <a href="http://framework.zend.com/manual/en/zend.application.theory-of-operation.html">More about Bootstrap.php file</a>.</p>

{% highlight php startinline %}
<?php

/**
* application/Bootstrap.php  
*/  
class Bootstrap extends Zend_Application_Bootstrap_Bootstrap  
{  

    protected function _initDatabase()  
    {  
        $adapter = Zend_Db::factory('pdo\_mysql', array(  
            'host' => 'localhost',  
            'username' => 'root',  
            'password' => '',  
            'dbname' => 'blog',  
            'charset' => 'utf8'

            /**  
            * Some options  
            * 'port' => '3307',  
            *'unix_socket' => '/tmp/mysql2.sock'  
            */  
        ));  
        Zend_Db_Table_Abstract::setDefaultAdapter($adapter); // setting up the db adapter to DbTable  
    }  

} 
{% endhighlight %}

<h3>Creating the Post DbTable (like a model)</h3>
<p><a href="http://framework.zend.com/manual/en/zend.db.table.html">What is DbTables and how it works?</a></p>

{% highlight php startinline %}
junior-mb:basic_crud juninhogr$ zf create db-table posts  
Please provide a value for $actualTableName  
zf> posts  
Note: The canonical model name that is used with other providers is "Posts"; not "posts" as supplied  
Creating a DbTable at /Library/WebServer/Documents/blog_posts/basic_crud/application/models/DbTable/Posts.php  
Updating project profile '/Library/WebServer/Documents/blog_posts/basic_crud/.zfproject.xml'  
{% endhighlight %}

<h3>Setting up AutoLoader and some IncludePaths</h3>
<p>To load DbTable (models) easier, let's configure the ZF AutoLoader and add the models folder to the PHP <strong>include_path</strong>.</p>

{% highlight php startinline %}
<?php

/**
 * application/Bootstrap.php  
 */  

class Bootstrap extends Zend\_Application\_Bootstrap_Bootstrap  
{  
    public function _initAutoloader()  
    {  
        $loader = Zend\_Loader\_Autoloader::getInstance();  
        $loader->setFallbackAutoloader(true);  
    }

    protected function _initDatabase()  
    {  
        // ...  
    }  
}  
{% endhighlight %}

<p>Take a look at 12 and 17 lines.</p>

{% highlight php startinline %}
<?php

/**
 * public/index.php  
 */

// Define path to application directory  
defined('APPLICATION_PATH') || define('APPLICATION_PATH', realpath(dirname(__FILE__) . '/../application'));

// Define application environment  
defined('APPLICATION_ENV') || define('APPLICATION_ENV', (getenv('APPLICATION_ENV') ? getenv('APPLICATION_ENV') : 'development')); // change to development now

// Ensure library/ is on include_path  
set_include_path(implode(PATH_SEPARATOR, array(  
    realpath(APPLICATION_PATH . '/../library'),  
    realpath(APPLICATION_PATH . '/models/DbTable'), // we will just call 'new Posts()'  
    get_include_path(),  
)));

/** Zend_Application */  
require_once 'Zend/Application.php';

// Create application, bootstrap, and run  
$application = new Zend_Application(  
    APPLICATION_ENV,  
    APPLICATION_PATH . '/configs/application.ini'  
);  

$application->bootstrap()->run();
{% endhighlight %}

<p>Let's change our Post class name to just Posts, at line 5.</p>

{% highlight php startinline %}
<?php

/**
 * application/models/DbTable/Posts.php  
 */  

class Posts extends Zend_Db_Table_Abstract  
{  
    protected $_name = 'posts';  
    /**  
    * Our posts' primary key is just 'id'.  
    * If you have another name to that field just use this line below:  
    * protected $_primary = 'post_id';  
    */

} 
{% endhighlight %}

<h3>SQL Dump</h3>
<p>First we created the database named 'blog'. The SQL code below is the result of a <a href="http://www.sequelpro.com">Sequel Pro</a> exportation feature.</p>

{% highlight php startinline %}
# Dump of table posts  
# ------------------------------------------------------------

CREATE TABLE `posts` (  
    `id` int(11) unsigned NOT NULL AUTO_INCREMENT,  
    `title` varchar(120) DEFAULT NULL,  
    `category` varchar(100) DEFAULT NULL,  
    `body` text,  
    `created` datetime DEFAULT NULL,  
    `updated` datetime DEFAULT NULL,  
    PRIMARY KEY (`id`)  
) ENGINE=InnoDB DEFAULT CHARSET=latin1; 
{% endhighlight %}

<h3>Getting the Post form using Zend_Form</h3>
<p>Let's create a Zend_Form to represent our <em>Posts</em> model.</p>

{% highlight php startinline %}
<?php

/**
 * controllers/PostsController.php  
 */  

class PostsController extends Zend_Controller_Action  
{  
    public function getForm()  
    {  
        $title = new Zend_Form_Element_Text('title');  
        $title->setLabel('Title')  
            ->setDescription('Just put the post title here')  
            ->setRequired(true) // required field  
            ->addValidator('StringLength', false, array(10, 120)) // min 10 max 120  
            ->addFilters(array('StringTrim'));

        $category = new Zend_Form_Element_Select('category');  
        $category->setLabel('Category')  
            ->setDescription('Select the post category')  
            ->setRequired(true)  
            ->setMultiOptions(array(  
                '' => ':: Select a category',  
                'php' => 'PHP',  
                'database' => 'Database',  
                'zf' => 'Zend Framework'  
                // ... more categories if you want  
            ))  
            ->addFilters(array('StringToLower', 'StringTrim')); // force to lowercase and trim

        $body = new Zend_Form_Element_Textarea('body');  
        $body->setLabel('Post')  
            ->setRequired(true)  
            ->setDescription('Your text. HTML tags aren
<h3>Starting to create the <em>Add</em> feature</h3>
<p>Let's first show the form in view file.</p>

{% highlight php startinline %}
<?php 

/**
 * controllers/PostsController.php  
 */  

class PostsController extends Zend_Controller_Action  
{  
    public function addAction()  
    {  
        $form = $this->getForm(); // getting the post form  
        $this->view->form = $form; // assigning the form to view  
    }

    // ... public function getForm()  
}  

<!-- the views/scripts/posts/add.phtml file -->
<?php echo $this->form ?>
{% endhighlight %}

<p>Let's create the <em>add action</em> and redirect to <em>index action</em>, our posts list, after insert. After do this, add some posts into your own application to test the results. In your browser go to something like this: <strong>http://localhost/blog&#95;app&#95;path/public/posts/add</strong>.</p>

{% highlight php startinline %}
<?php

/**
 * controllers/PostsController.php  
 */  

class PostsController extends Zend_Controller_Action  
{  
    public function init() // called always before actions  
    {  
        $this->Posts = new Posts(); // DbTable  
    }

    public function addAction()  
    {  
        $form = $this->getForm(); // getting the post form

        if ($this->getRequest()->isPost()) { //is it a post request ?  
            $postData = $this->getRequest()->getPost(); // getting the $_POST data  
            if ($form->isValid($postData)) {  
                $formData = $form->getValues(); // data filtered  
                // created and updated fields  
                $formData += array('created' => date('Y-m-d H:i:s'), 'updated' => date('Y-m-d H:i:s'));  
                $this->Posts->insert($formData); // database insertion  
            }  
            else $form->populate($postData); // show errors and populate form with $postData  
        }

        $this->view->form = $form; // assigning the form to view  
    }

    public function indexAction()  
    {  
        // get all posts - the newer first  
        $this->view->posts = $this->Posts->fetchAll(null, 'created desc');  
    }

    public function getForm()  
    {  
        // function logic  
    }  
} 
{% endhighlight %}

<p>The <strong>posts/index.phtml</strong> file, showing the posts list.</p>

{% highlight php startinline %}
<!-- application/views/scripts/posts/index.phtml -->

<h1>Posts</h1>

<table>
    <tr>
        <th>Title</th>
        <th>Created</th>
        <th></th>
    </tr>

    <?php foreach ($this->posts as $key => $post): ?>

    <tr>
        <td>
            <a href="<?php echo $this->baseUrl('/posts/show/id/' . $post->id) ?>">
                <?php echo $post->title ?>
            </a>
        </td>

        <td>
            <?php echo $post->created ?>
        </td>

        <td>
            <a href="<?php echo $this->baseUrl('/posts/edit/id/' . $post->id) ?>">Edit</a>
            <a href="<?php echo $this->baseUrl('/posts/del/id/' . $post->id) ?>">Delete</a>
        </td>
    </tr>

    <?php endforeach ?>

</table>
{% endhighlight %}

<h3>Release, Update, Delete features</h3>
<p>Now let's create the actions <em>show</em>, <em>edit</em>, and <em>del</em>...</p>

{% highlight php startinline %}
<?php

/**
 * controllers/PostsController.php  
 */  

class PostsController extends Zend_Controller_Action  
{  
    // init(), addAction(), indexAction() 

    public function showAction()  
    {  
        $id = $this->getRequest()->getParam('id');  
        if ($id > 0) {  
            $post = $this->Posts->find($id)->current(); // or $this->Posts->fetchRow("id = $id");  
            $this->view->post = $post;  
        }  
        else $this->view->message = 'The post ID does not exist';  
    }

    public function editAction()  
    {  
        $form = $this->getForm();  
        $id = $this->getRequest()->getParam('id');

        if ($id > 0) {  
            if ($this->getRequest()->isPost()) { // update form submit  
                $postData = $this->getRequest()->getPost();  
                if ($form->isValid($postData)) {  
                    $formData = $form->getValues();  
                    $formData += array('updated' => date('Y-m-d H:i:s'));  
                    $this->Posts->update($formData, "id = $id"); // update  
                    $this->_redirect('/posts/index');  
                }  
                else $form->populate($postData);  
            }  
            else {  
                $post = $this->Posts->find($id)->current();  
                $form->populate($post->toArray()); // populate method parameter has to be an array

                // add the id hidden field in the form  
                $hidden = new Zend_Form_Element_Hidden('id');  
                $hidden->setValue($id);

                $form->addElement($hidden);  
            }  
        }  
        else $this->view->message = 'The post ID does not exist';

        $this->view->form = $form;  
    }

    public function delAction()  
    {  
        $id = $this->getRequest()->getParam('id');  
        if ($id > 0) {  
            // option 1  
            /*$post = $this->Posts->find($id)->current();  
            $post->delete();*/

            // option 2  
            $this->Posts->delete("id = $id");
            $this->_redirect('/posts/index');  
        }  
    }

    // getForm()

}  
{% endhighlight %}

<p>... and the views <em>show.phtml</em> and <em>edit.phtml</em>. The <strong>delAction</strong> redirects to indexAction after insert, so it's not necessary to create a view file to it.</p>

{% highlight php startinline %}
// application/views/scripts/posts/show.phtml

<h1>Show Post</h1>

<?php if ($this->post): ?>

    <table>
        <tr>
            <th>ID</th>
            <td><?php echo $this->post->id ?></td>

            <th>Title</th>
            <td><?php echo $this->post->title ?></td>

            <th>Body</th>
            <td><?php echo $this->post->body ?></td>

            <th>Post Date</th>
            <td><?php echo $this->post->created ?></td>

            <th>Updated Date</th>
            <td><?php echo $this->post->updated ?></td>
        </tr>
    </table>

<?php else: ?>

    <?php echo $this->message ?>

<?php endif ?>


// application/views/scripts/posts/edit.phtml


<h1>Editing Post</h1>

<?php if ($this->message): ?>

    <?php echo $this->message ?>

<?php endif ?>


<?php echo $this->form ?>
{% endhighlight %}

<p>After that, test your posts CRUD and customize it according your needs.</p>
<p>Well, that's all. Use Zend Framework it's not hard and you have only understand the framework's concepts. After that, search and study some components you like in the <a href="http://framework.zend.com/manual/en/">documentation</a> and make miracles with this powerfull framework.</p>
<p>That is just an example what you can do with ZF. You can create a BaseController class and make this CRUD functionality extensible to a lot of DbTables and Controllers, using the <a href="http://en.wikipedia.org/wiki/Object-oriented_programming">OOP</a>.</p>
<p>To get this application zipped file <a href="http://juniorgrossi.com/wp-content/uploads/2011/07/basic_crud.zip">click here</a>.</p>
<p>Thanks a lot! See you...</p>
t avaiable')  
            ->addFilters(array('HtmlEntities')); // remove HTML tags

        $submit = new Zend_Form_Element_Submit('submit');  
        $submit->setLabel('Post to Blog') // the button's value  
            ->setIgnore(true); // very usefull -> it will be ignored before insertion

        $form = new Zend_Form();  
        $form->addElements(array($title, $category, $body, $submit));  
            // ->setAction('') // you can set your action. We will let blank, to send the request to the same action

        return $form; // return the form  
    }  
}
{% endhighlight %}

<h3>Starting to create the <em>Add</em> feature</h3>
<p>Let's first show the form in view file.</p>

{% highlight php startinline %}
<?php 

/**
 * controllers/PostsController.php  
 */  

class PostsController extends Zend_Controller_Action  
{  
    public function addAction()  
    {  
        $form = $this->getForm(); // getting the post form  
        $this->view->form = $form; // assigning the form to view  
    }

    // ... public function getForm()  
}  

<!-- the views/scripts/posts/add.phtml file -->
<?php echo $this->form ?>
{% endhighlight %}

<p>Let's create the <em>add action</em> and redirect to <em>index action</em>, our posts list, after insert. After do this, add some posts into your own application to test the results. In your browser go to something like this: <strong>http://localhost/blog&#95;app&#95;path/public/posts/add</strong>.</p>

{% highlight php startinline %}
<?php

/**
 * controllers/PostsController.php  
 */  

class PostsController extends Zend_Controller_Action  
{  
    public function init() // called always before actions  
    {  
        $this->Posts = new Posts(); // DbTable  
    }

    public function addAction()  
    {  
        $form = $this->getForm(); // getting the post form

        if ($this->getRequest()->isPost()) { //is it a post request ?  
            $postData = $this->getRequest()->getPost(); // getting the $_POST data  
            if ($form->isValid($postData)) {  
                $formData = $form->getValues(); // data filtered  
                // created and updated fields  
                $formData += array('created' => date('Y-m-d H:i:s'), 'updated' => date('Y-m-d H:i:s'));  
                $this->Posts->insert($formData); // database insertion  
            }  
            else $form->populate($postData); // show errors and populate form with $postData  
        }

        $this->view->form = $form; // assigning the form to view  
    }

    public function indexAction()  
    {  
        // get all posts - the newer first  
        $this->view->posts = $this->Posts->fetchAll(null, 'created desc');  
    }

    public function getForm()  
    {  
        // function logic  
    }  
} 
{% endhighlight %}

<p>The <strong>posts/index.phtml</strong> file, showing the posts list.</p>

{% highlight php startinline %}
<!-- application/views/scripts/posts/index.phtml -->

<h1>Posts</h1>

<table>
    <tr>
        <th>Title</th>
        <th>Created</th>
        <th></th>
    </tr>

    <?php foreach ($this->posts as $key => $post): ?>

    <tr>
        <td>
            <a href="<?php echo $this->baseUrl('/posts/show/id/' . $post->id) ?>">
                <?php echo $post->title ?>
            </a>
        </td>

        <td>
            <?php echo $post->created ?>
        </td>

        <td>
            <a href="<?php echo $this->baseUrl('/posts/edit/id/' . $post->id) ?>">Edit</a>
            <a href="<?php echo $this->baseUrl('/posts/del/id/' . $post->id) ?>">Delete</a>
        </td>
    </tr>

    <?php endforeach ?>

</table>
{% endhighlight %}

<h3>Release, Update, Delete features</h3>
<p>Now let's create the actions <em>show</em>, <em>edit</em>, and <em>del</em>...</p>

{% highlight php startinline %}
<?php

/**
 * controllers/PostsController.php  
 */  

class PostsController extends Zend_Controller_Action  
{  
    // init(), addAction(), indexAction() 

    public function showAction()  
    {  
        $id = $this->getRequest()->getParam('id');  
        if ($id > 0) {  
            $post = $this->Posts->find($id)->current(); // or $this->Posts->fetchRow("id = $id");  
            $this->view->post = $post;  
        }  
        else $this->view->message = 'The post ID does not exist';  
    }

    public function editAction()  
    {  
        $form = $this->getForm();  
        $id = $this->getRequest()->getParam('id');

        if ($id > 0) {  
            if ($this->getRequest()->isPost()) { // update form submit  
                $postData = $this->getRequest()->getPost();  
                if ($form->isValid($postData)) {  
                    $formData = $form->getValues();  
                    $formData += array('updated' => date('Y-m-d H:i:s'));  
                    $this->Posts->update($formData, "id = $id"); // update  
                    $this->_redirect('/posts/index');  
                }  
                else $form->populate($postData);  
            }  
            else {  
                $post = $this->Posts->find($id)->current();  
                $form->populate($post->toArray()); // populate method parameter has to be an array

                // add the id hidden field in the form  
                $hidden = new Zend_Form_Element_Hidden('id');  
                $hidden->setValue($id);

                $form->addElement($hidden);  
            }  
        }  
        else $this->view->message = 'The post ID does not exist';

        $this->view->form = $form;  
    }

    public function delAction()  
    {  
        $id = $this->getRequest()->getParam('id');  
        if ($id > 0) {  
            // option 1  
            /*$post = $this->Posts->find($id)->current();  
            $post->delete();*/

            // option 2  
            $this->Posts->delete("id = $id");
            $this->_redirect('/posts/index');  
        }  
    }

    // getForm()

}  
{% endhighlight %}

<p>... and the views <em>show.phtml</em> and <em>edit.phtml</em>. The <strong>delAction</strong> redirects to indexAction after insert, so it's not necessary to create a view file to it.</p>

{% highlight php startinline %}
// application/views/scripts/posts/show.phtml

<h1>Show Post</h1>

<?php if ($this->post): ?>

    <table>
        <tr>
            <th>ID</th>
            <td><?php echo $this->post->id ?></td>

            <th>Title</th>
            <td><?php echo $this->post->title ?></td>

            <th>Body</th>
            <td><?php echo $this->post->body ?></td>

            <th>Post Date</th>
            <td><?php echo $this->post->created ?></td>

            <th>Updated Date</th>
            <td><?php echo $this->post->updated ?></td>
        </tr>
    </table>

<?php else: ?>

    <?php echo $this->message ?>

<?php endif ?>


// application/views/scripts/posts/edit.phtml


<h1>Editing Post</h1>

<?php if ($this->message): ?>

    <?php echo $this->message ?>

<?php endif ?>


<?php echo $this->form ?>
{% endhighlight %}

<p>After that, test your posts CRUD and customize it according your needs.</p>
<p>Well, that's all. Use Zend Framework it's not hard and you have only understand the framework's concepts. After that, search and study some components you like in the <a href="http://framework.zend.com/manual/en/">documentation</a> and make miracles with this powerfull framework.</p>
<p>That is just an example what you can do with ZF. You can create a BaseController class and make this CRUD functionality extensible to a lot of DbTables and Controllers, using the <a href="http://en.wikipedia.org/wiki/Object-oriented_programming">OOP</a>.</p>
<p>To get this application zipped file <a href="http://juniorgrossi.com/wp-content/uploads/2011/07/basic_crud.zip">click here</a>.</p>
<p>Thanks a lot! See you...</p>
