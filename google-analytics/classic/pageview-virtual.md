#Classic Analytics - Pageview Padrão e Virtual

##Pageview Padrão

Segue abaixo exemplo de pageview padrão em Classic Anaytics, com o formato mais simples sem nenhuma customização (tag padrão).

O pageview padrão utilizará a URL path a partir do domínio, com parâmetros de query string, para popular a dimensão 'Página' (parâmetro utmp no disparo do utm.gif)


``` html
<script type="text/javascript">
    var _gaq = _gaq || [];
	_gaq.push(['_setAccount', 'UA-XXXX-X']);
	_gaq.push(['_trackPageview']);

	(function() {
	var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
	ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/u/ga.js';
	var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
	})();
</script>
```

##Pageview Virtual - cenário 1

Para enviar ao Google Analytics uma URL customizada, sobrescrevendo a URL padrão, ou então monitorar interações na página como pageviews, deve-se utilizar o método abaixo, chamado de pageview virtual.

Obs: deve ser iniciado por uma "/"

``` html
<script type="text/javascript">
    var _gaq = _gaq || [];
	_gaq.push(['_setAccount', 'UA-XXXX-X']);
	_gaq.push(['_trackPageview', '/url-customizada/etc']);

	(function() {
	var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
	ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/u/ga.js';
	var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
	})();
</script>
```

Com esta implementação, a dimensão 'Página' irá receber os dados da maneira como foi passado no `_trackPageview`. Porém as dimensões de evento (Categoria do Evento, Ação do Evento, Rótulo do evento), quando cruzadas com Página como dimensão secundária, mostrarão a 'Página' ainda com base na URL original, e não a virtual.

Para fazer com que a 'Página' dos eventos seja também a virtual, ver Cenário 2 abaixo


##Pageview Virtual - Cenário 2

Para fazer com que a 'Página' dos eventos seja também a virtual, deve ser utilizado o método '_set' antes do pageview ou evento:

``` html
<script type="text/javascript">
    var _gaq = _gaq || [];
	_gaq.push(['_setAccount', 'UA-XXXX-X']);
	_gaq.push(['_set', 'page', '/url-customizada/etc']);
	_gaq.push(['_trackPageview']);
	_gaq.push(['_trackEvent', 'categoria', 'acao']);

	(function() {
	var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
	ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/u/ga.js';
	var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
	})();
</script>
```
