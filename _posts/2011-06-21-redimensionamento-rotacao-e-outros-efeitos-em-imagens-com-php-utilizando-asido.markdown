---
layout: post
status: publish
published: true
title: Redimensionamento, rotação e outros efeitos em imagens com PHP utilizando Asido

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  É muito comum em aplicações web a necessidade de trabalhar com imagens, tanto para redimensionamento, criação de thumbnails, girar, etc. Apresento a <a href="http://asido.info/">Asido</a>, um conjunto de classes para trabalhar com imagens em <strong>PHP</strong>. 

wordpress_id: 106
wordpress_url: http://www.jgrossi.com/?p=106
date: '2011-06-21 12:32:39 -0300'
date_gmt: '2011-06-21 15:32:39 -0300'
categories:
- PHP
tags:
- asido
- gd
- imagemagick
- imagens
- php
---
<p>Boa tarde, pessoal!</p>
<p>É muito comum em aplicações web a necessidade de trabalhar com imagens, tanto para redimensionamento, criação de thumbnails, girar, etc.</p>
<p>Apresento a <a href="http://asido.info/">Asido</a>, um conjunto de classes para trabalhar com imagens em <strong>PHP</strong>. Embora eu não goste muito da forma de organização de <a href="http://pt.wikipedia.org/wiki/Orienta%C3%A7%C3%A3o_a_objetos">OO</a> que a Asido utiliza, é uma boa ferramenta e suporta algumas bibliotecas de imagens (chamadas de <em>drivers</em>) como:</p>
<ul>
<li><a href="http://php.net/manual/en/ref.image.php#image.requirements">GD2</a> - Muito conhecida e muito utilizada em PHP</li>
<li><a href="http://imagemagick.org/script/api.php#php">MagickWand</a> - Uma extensão nativa de PHP para trabalhar com a ImageMagick</li>
<li><a href="http://php.net/manual/en/ref.imagick.php#imagick.requirements">Imagick extension</a> - Idem item anterior, porém é um pouco mais antiga</li>
<li><a href="http://imagemagick.org/script/command-line-tools.php">Imagick shell</a> - Para trabalhos com a CLI do PHP</li>
</ul>
<p><a id="more"></a><a id="more-106"></a></p>
<p>Primeiramente faça o <a href="http://asido.info/download/">download</a> da Asido e coloque em algum local em sua aplicação para que possamos incluir os arquivos.</p>
<p><strong>Detalhe importante: **Quando começar a gerar imagens com a Asido utilizando <em>GD</em> vai notar que a qualidade é <em>péssima</em>. Somente edite a linha **184</strong> do arquivo <strong>class.driver.gd.php</strong> deixando da seguinte maneira:</p>

{% highlight php startinline %}
<?php
    // ...
    $r = imageCopyResampled( // antes era imageCopyResized  
    // ...  
?>
{% endhighlight %}

<p>Isso vai mudar <em>drasticamente</em> a qualidade das imagens. Por <em>default</em> a Asido gera JPG com qualidade 80. Se quiser mudar vá até a linha <strong>17</strong> do mesmo arquivo <strong>class.driver.gd.php</strong> e altere para o número que desejar.</p>

{% highlight php startinline %}
<?php
    // ...
    define('ASIDO_GD_JPEG_QUALITY', 80);  
    // ...  
?>
{% endhighlight %}

<p>Continuando...</p>
<p>A Asido possui diversas funcionalidades que podem ser úteis no dia-a-dia de um desenvolvedor. Além de simples, em poucas linhas de código redimensiona-se uma imagem, rotaciona, etc. Vou destacar as principais funcionalidades (que mais uso) da Asido:</p>
<h3>Watermark</h3>
<p>Cria uma marca d'água sobre imagens. Você informa a imagem da marca dágua que deseja inserir, a imagem de destino e a Asido faz o resto pra você.</p>

{% highlight php startinline %}
<?php
    require_once "asido/class.asido.php";

    asido::driver('gd'); // vamos usar a GD  
    $image = asido::image(  
        'imagem_de_origem.jpg',  
        'imagem_de_destino.png'  
    );  

    asido::watermark($image, 'marca_dagua.png');

    // sobrescreve caso exista o arquivo de destino  
    $image->save(ASIDO_OVERWRITE_ENABLED);  
?>
{% endhighlight %}

<h3>Resize</h3>
<p>Redimensiona uma imagem (criação de thumbnails). Você informa também imagens de origem e destino, as dimensões que quer (pode ser largura e altura, somente altura e somente largura, bem prático) e pronto.</p>

{% highlight php startinline %}
<?php

require_once "asido/class.asido.php";

asido::driver('gd'); // vamos usar a GD  

$image = asido::image(  
    'imagem_de_origem.jpg',  
    'imagem_de_destino.png'  
);

// redimensiona para uma imagem 400x400px proporcionalmente  
asido::resize($image, 400, 400, ASIDO_RESIZE_PROPORTIONAL);

// sobrescreve caso exista o arquivo de destino  
$image->save(ASIDO_OVERWRITE_ENABLED);  

?>
{% endhighlight %}

<p>Somente informando a largura que deseja e ele calcula a altura proporcionalmente:</p>

{% highlight php startinline %}
<?php

// ...
asido::resize($image, 400, 0, ASIDO_RESIZE_PROPORTIONAL); // largura 400px  
// ou  
asido::resize($image, 0, 400, ASIDO_RESIZE_PROPORTIONAL); // altura 400px  
// ...  

?>
{% endhighlight %}

<h3>Fit</h3>
<p>Permite você redimensionar uma imagem para "caber" em um espaço pre-determinado por você <em>(largura X altura px)</em>, ou seja, se a imagem for menor, ele não redimensiona, caso seja maior, ele vai achar o tamanho mínimo ideal para que sua imagem caiba dentro do espaço que você quer.</p>

{% highlight php startinline %}
<?php
// ...
asido::fit($image, 500, 500);  
// ...  
?>
{% endhighlight %}

<h3>Convert</h3>
<p>Converte imagens em outros formatos de arquivo. Tipo <strong>JPG</strong> para <strong>GIF</strong> e por ai vai. Suporta <strong>JPG</strong>, <strong>GIF</strong> e <strong>PNG</strong>.</p>
<h3>Rotate</h3>
<p>Permite rotacionar uma imagem, em qualquer ângulo que desejar.</p>

{% highlight php startinline %}
<?php

// ...

asido::rotate($image, 90);  
asido::rotate($image, 180);  
asido::rotate($image, 270);  

// ...  

?>
{% endhighlight %}

<p>Além do mais, você pode escolher um ângulo bem <em>bizarro</em> e preencher o fundo com uma cor, visto que a imagem vai ficar bem estranha né?</p>

{% highlight php startinline %}
<?php

// ...

// rotacionar 30 graus  
asido::rotate($image, 30, asido::color(39, 107, 20));  

// ...  

?>
{% endhighlight %}

<p><a href="http://www.asido.info/examples/rotate/expected_result_02_thumb.png">Rotação com preenchimento por cor utilizando Asido</a></p>
<ul>
<li><strong>Flip e Flop:</strong> Cria uma imagem espelhada verticalmente e horizontalmente, respectivamente. </li>
<li><strong>Crop:</strong> "Corta" um pedaço da imagem que desejar, passando as coordenadas <strong>x, y</strong>.</li>
<li><strong>Grayscale:</strong> Transforma a imagem em uma imagem preto-e-branco, ou melhor, em escala de cinza.</li>
</ul>
<p>Para maiores detalhes leia a <a href="http://asido.info/documentation/">documentação</a> da Asido ou dê uma olhada rápida em alguns <a href="http://asido.info/about/features/">exemplos</a> para facilitar.</p>
