---
layout: post
status: publish
published: true
title: Tradução usando gettext (*.mo) no Poedit

author_login: grossi
author_email: juniorgro@gmail.com
author_url: http://grossi.io
excerpt: |+
  Indo direto ao assunto, sempre tive dúvidas quanto a usar o Poedit para traduzir <em>strings</em>, motivo pelo qual usava mais <em>arrays</em> em PHP mesmo. Porém, mais cedo ou mais tarde, necessitamos evoluir e correr atrás do tempo perdido. Sempre achei o <a href="http://www.poedit.net/">Poedit</a> chato e nunca conseguia fazê-lo funcionar. Após umas fuçadas na net e acertar o link correto, consegui (e entendi :-)) seu funcionamento.

wordpress_id: 31
wordpress_url: http://jgrossi.com/blog/?p=31
date: '2011-06-16 15:44:16 -0300'
date_gmt: '2011-06-16 18:44:16 -0300'
categories:
- i18n
- Poedit
tags:
- gettext
- i18n
- php
- poedit
- zend framework
---
<p>Olá pessoal.</p>
<p>Indo direto ao assunto, sempre tive dúvidas quanto a usar o Poedit para traduzir <em>strings</em>, motivo pelo qual usava mais <em>arrays</em> em PHP mesmo. Porém, mais cedo ou mais tarde, necessitamos evoluir e correr atrás do tempo perdido.</p>
<p>Sempre achei o <a href="http://www.poedit.net/">Poedit</a> chato e nunca conseguia fazê-lo funcionar. Após umas fuçadas na net e acertar o link correto, consegui (e entendi :-)) seu funcionamento.</p>
<ul>
<li>File -> New catalog</li>
</ul>
<p>Na aba <em>Project info</em> você preenche os dados que deseja. Não esqueça de preencher o idioma (que está traduzindo) e o país, além de nome do projeto e e-mail, que é sempre bom preencher.</p>
<p>Feito isso, vem a configuração das próximas 2 abas: <em>Paths</em> e <em>Keywords</em>. Aí que está o segredo que até então eu desconhecia:</p>
<ul>
<li><em>Paths:</em> Aqui você vai adicionar seu diretório padrão. O Poedit vai vasculhar todos seus arquivos, hierarquicamente, a partir deste diretório. Dica: coloque <em>.</em> (ponto), pois irá salvar seu arquivo .po (Poedit) na pasta que deseja ser a principal, de maneira que ele vai vasculhar a partir daí. Não se esqueça de clicar em <em>New item</em> e colocar o <em>.</em>, senão não vai funcionar.</li>
<li><em>Keywords:</em> Aqui você fala pro Poedit qual a função que ele deve procurar por strings sem tradução. Pra quem usa Zend Framework vai colocar <strong>$this->translate</strong> [<a href="http://framework.zend.com/manual/en/zend.translate.html">link</a>] e no caso de Wordpress vai colocar 2 funções: <strong>__</strong> e <strong>_e</strong> [<a href="http://codex.wordpress.org/I18n_for_WordPress_Developers">link</a>]. Isso vai dizer ao Poedit pra procurar em todos os arquivos abaixo do seu diretório padrão essas funções, e vai localizar a *string* que você já colocou nas *views*. </li>
</ul>
<p><em>Sua view já deve ter sido preparada para receber as traduções. Vou fazer um post ainda sobre alguns probleminhas que ocorreram comigo e como os solucionei.</em></p>
<p>Quando clicar em OK o Poedit irá perguntar onde irá salvar o arquivo e qual nome dará a ele. O nome pouco importa aqui neste momento, mas a pasta que irá salvar é de extrema importância, pois a partir dela que o programa irá procurar pelas <em>keywords</em> (as funções de tradução que configuramos) nos arquivos.</p>
<p>Salve seu arquivo no local escolhido. O programa irá analisar todas as <em>strings</em> ainda pendentes de tradução e mostrará pra você as novas, que ainda não foram traduzidas (neste caso todas). Ao confirmar ele irá abrir a tela principal do Poedit já com as strings a serem traduzidas. Aí é mole: clique em uma frase e no canto inferior digite a frase correspondente na língua que quer a tradução.</p>
<p>Feitas as traduções, salve seu arquivo <strong>.mo</strong> e copie-o na pasta da sua aplicação destinada a este fim (depende do seu framework ou como organizou suas estrutura de pastas). Se precisar alterar alguma coisa (ou adicionar alguma nova string nas <em>views</em>) abra o arquivo <strong>.po</strong> e clique em <em>Update</em> para analisar nos arquivos se existe alguma nova frase ainda não traduzida. Aí é só repetir os procedimentos acima.</p>
<p>A partir daí é correr pro abraço! Valeu!</p>
