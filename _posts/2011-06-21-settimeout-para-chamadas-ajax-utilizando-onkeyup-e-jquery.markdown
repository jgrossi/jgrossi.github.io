---
layout: post
status: publish
published: true
title: setTimeOut para chamadas Ajax utilizando onKeyUp e jQuery

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Utilizar <strong>onKeyUp</strong> para fazer chamadas Ajax é bem interessante, melhorando e muito a experiência do usuário com o sistema, de maneira que os resultados da pesquisa vão aparecendo na medida que ele pressiona as teclas no teclado. Porém, imagine, a cada tecla que o usuário pressiona é uma chamada Ajax na aplicação, o que pode reduzir em performance e aumentar a utilização de banda.

wordpress_id: 92
wordpress_url: http://www.jgrossi.com/?p=92
date: '2011-06-21 12:15:05 -0300'
date_gmt: '2011-06-21 15:15:05 -0300'
categories:
- Javascript
tags: []
---
<p>Boa tarde, pessoal!</p>
<p>Utilizar <strong>onKeyUp</strong> para fazer chamadas Ajax é bem interessante, melhorando e muito a experiência do usuário com o sistema, de maneira que os resultados da pesquisa vão aparecendo na medida que ele pressiona as teclas no teclado. Porém, imagine, a cada tecla que o usuário pressiona é uma chamada Ajax na aplicação, o que pode reduzir em performance e aumentar a utilização de banda.</p>
<p>Sendo assim, uma opção é esperar o usuário parar de digitar por um tempinho para então pesquisar via Ajax. Pode parecer voltar no tempo, mas o intervalo é tão pequeno (na ordem de 500 milisegundos) que nem dá pra perceber direito. Além do mais nada impede que coloquemos 250 milisegundos ou menos. Para isso vamos usar as funções <strong>setTimeOut</strong> e <strong>clearInterval</strong>.</p>

{% highlight php startinline %}
var interval = 0; 

$(function()  
{  
    $('#query').keyup(function()  
    {  
        // começa a contar o tempo  
        clearInterval(interval); 

        // 500ms após o usuário parar de digitar a função é chamada  
        interval = window.setTimeout(function(){  
            // sua chamada ajax e demais códigos  
        }, 500);  
    });  
}  
{% endhighlight %}

<p>É isso aí pessoal! Espero ter sisdo claro! Até a próxima!</p>
