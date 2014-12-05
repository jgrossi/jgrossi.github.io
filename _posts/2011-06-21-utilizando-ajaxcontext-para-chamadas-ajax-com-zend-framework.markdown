---
layout: post
status: publish
published: true
title: Utilizando AjaxContext para chamadas Ajax com Zend Framework

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Esta semana estou trabalhando num projeto onde a utilização de Ajax está sendo muito útil em termos de performance e facilidade na busca de produtos no banco de dados. Como estou utilizando Zend Framework no projeto, vou explicar resumidamente como trabalhar com requisições Ajax no ZF.

wordpress_id: 66
wordpress_url: http://www.jgrossi.com/?p=66
date: '2011-06-21 01:13:07 -0300'
date_gmt: '2011-06-21 04:13:07 -0300'
categories:
- PHP
- Zend Framework
tags:
- ajax
- php
- zend framework
- zf
---
<p>Olá pessoal!</p>
<p>Esta semana estou trabalhando num projeto onde a utilização de Ajax está sendo muito útil em termos de performance e facilidade na busca de produtos no banco de dados.</p>
<p>Como estou utilizando Zend Framework no projeto, vou explicar resumidamente como trabalhar com requisições Ajax no ZF.</p>
<p>Basicamente, o óbvio seria desabilitar a renderização da <em>view</em> por padrão em sua <em>action</em> e também desabilitar o <em>layout</em>, caso esteja usando-o.</p>

{% highlight php startinline %}
// outros códigos do controller

public function testeAction()  
{  
    $this->_helper->viewRenderer->setNoRender();  
    $this->_helper->layout->disableLayout();  
    //..  
} 
{% endhighlight %}

<p>Como já é de se esperar, o ZF possui um <em>Action Helper</em> que facilita muito a vida. Do jeito acima, eu teria que renderizar uma view na mão para retornar alguma coisa pra solicitação Ajax. O jeito do Zend te permite escolher o que irá retornar (html ou json), padronizar o nome do arquivo da *view *que será chamado, e desabilitar o layout pra você.</p>
<p>Vamos colocar no nosso método <strong>preDispatch</strong> a chamada para nosso <em>helper ajaxContext</em>:</p>

{% highlight php startinline %}
public function preDispatch()  
{  
    $ajaxContext = $this->_helper->getHelper('AjaxContext');  
    $ajaxContext->addActionContext('teste', 'html')  
        ->addActionContext('outra-action', 'html')  
        ->initContext();  
}
{% endhighlight %}

<p>Desta forma, eu já estou dizendo pro meu <em>controller</em> que os métodos <strong>ajax</strong> e <strong>outra-action</strong> são acessados por requisições Ajax e retornaram código <strong>HTML</strong>.</p>
<p>Pra facilitar ainda mais nossa vida o Zend já vai chamar automaticamente a view <strong>teste.ajax.phtml</strong> na pasta de views do <em>controller</em>. Isso facilita ainda mais a padronização de arquivos.</p>
<p>Caso não queira retornar <strong>HTML</strong>, posso retornar <strong>JSON</strong> bastando alterar a chamada no <strong>preDispatch</strong>:</p>

{% highlight php startinline %}
public function preDispatch()  
{  
    $ajaxContext = $this->_helper->getHelper('AjaxContext');  
    $ajaxContext->addActionContext('teste', 'json')  
        ->addActionContext('outra-action', 'html')  
        ->initContext();  
}  
{% endhighlight %}

<p>Desta forma minha <em>action</em> <strong>teste</strong> irá me retornar <strong>JSON</strong>. Posso utilizar associações de variáveis nas views normalmente que o Zend transformará o código em <strong>JSON</strong>:</p>

{% highlight php startinline %}
public function testeAction()  
{  
    // demais códigos

    $this->view->valor1 = $valor1;  
    $this->view->valor2 = $valor2;

    // o Zend transformará todas as variáveis da view em JSON  
}
{% endhighlight %}

<p>Em alguns casos, será necessário acrescentar <strong>?format=html</strong> ou <strong>?format=json</strong> à sua URL na chamada do Ajax (jQuery, etc), avisando o Zend o que ele precisa.</p>
<p>Para maiores informações sobre o <em>AjaxContext</em> acesse a <a href="http://framework.zend.com/manual/1.11/en/zend.controller.actionhelpers.html">documentação oficial do Zend Framework</a>.</p>
<p>É isso aí! Ajax com Zend Framework é bem tranquilo de entender e bastante eficiente!</p>
