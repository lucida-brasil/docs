#Acompanhamento de iFrames com linker

##Código de acompanhamento

O primeiro passo para o acompanhamento de iframes em domínios terceiros é alterar o código de acompanhamento do Google Analytics, essa configuração muda de acordo com a versão do GA.

###Google Analytics Classic

Todas as chamasdas da tag do GA devem contem o parâmetro `_gaq.push(['_setAllowLinker', true]);` que permite a continuidade da visita, também conhecida como cópia de cookies, entre domínios.

<!-- Tag Base -->
``` html
<script type="text/javascript">
    var _gaq = _gaq || [];
	_gaq.push(['_setAccount', 'UA-XXXX-X']);
	_gaq.push(['_setAllowLinker', true]); // permite a copia de cookies entre dominios
	_gaq.push(['_setDomainName', 'domain.com']);
	
	_gaq.push(['_trackPageview']);

	(function() {
	var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
	ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/u/ga.js';
	var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
	})();
</script>
```

O `iframe` pode ser inserido normalmente na página, desde que esteja disponível antes da tag padrão do GA e tenha um `ID` unico na página.

``` html
<iframe id="myframe" name="myframe" src="http://example.com"></iframe>
```

Após a chamada do Google Analytics deve ser inserido o script abaixo, que atualiza o parâmetro `src` do `iframe` incluindo os parametros `__utm` necessário para a cópia de cookies.

``` html
<script type="text/javascript">
	_gaq.push(function() {
		var pageTracker = _gat._getTrackerByName(),
			iframe = document.getElementById('myframe');
		
		iframe.src = pageTracker._getLinkerUrl(iframe.src);
	});
</script>
```

Exemplo completo:

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
	<title>Document</title>
</head>
<body>

	<iframe id="myframe" name="myframe" src="http://example.com"></iframe>

	<script type="text/javascript">
		var _gaq = _gaq || [];
		_gaq.push(['_setAccount', 'UA-XXXX-X']);
		_gaq.push(['_setAllowLinker', true]);
		_gaq.push(['_setDomainName', 'domain.com']);
		
		_gaq.push(['_trackPageview']);

		(function() {
		var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
		ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/u/ga.js';
		var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
		})();

		_gaq.push(function() {
			var pageTracker = _gat._getTrackerByName(),
				iframe = document.getElementById('myframe');
			
			iframe.src = pageTracker._getLinkerUrl(iframe.src);
		});
	</script>
</body>
</html>
```

###Google Universal Analytics

Todas as chamasdas da tag do GA devem contem o parâmetro `{'allowLinker': true}` junto da definição do domínio, que permite a continuidade da visita, também conhecida como cópia de cookies.

``` html
<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-XXXX-X', 'domain.com', {'allowLinker': true}); //permitir a copia de cookies
  ga('send', 'pageview');

</script>
```

O `iframe` deve ser inserido dinâmicamente na página, para que isso seja feito da maneira correta, adicione um `div` com `ID` unico na página onde o `iframe` precisa ser criado.

**Nota:** recomendamos que todo estilo do `iframe` seja feito por um arquivo `CSS` externo. 

``` html
<div id="myiFrame"></div>
``` 

Crie a função `addiFrame`, conforme o código abaixo, esta função recebe o `I	D` do `div` onde o iframe deve ser criado e url que se tornara o `src` do `iframe`.

``` html
<script type="text/javascript">
//definição da função addiFrame
var addiFrame = function(divId, url, opt_hash) {
	ga(function(tracker) {
		window.linker = window.linker || new window.gaplugins.Linker(tracker);
		var iFrame = document.createElement('iframe');
		iFrame.src = window.linker.decorate(url, opt_hash);
		document.getElementById(divId).appendChild(iFrame);
	});
};

//Adicionando o iframe na página
addiFrame('myiFrame', 'http://example.com');

</script>
```

Exemplo Completo:

``` html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>

<div id="myiFrame"></div>

<script>
	(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
	(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
	m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
	})(window,document,'script','//www.google-analytics.com/analytics.js','ga');

	ga('create', 'UA-XXXX-X', 'domain.com', {'allowLinker': true}); //permitir a copia de cookies
	ga('send', 'pageview');

	//definição da função addiFrame
	var addiFrame = function(divId, url, opt_hash) {
		ga(function(tracker) {
			window.linker = window.linker || new window.gaplugins.Linker(tracker);
			var iFrame = document.createElement('iframe');
			iFrame.src = window.linker.decorate(url, opt_hash);
			document.getElementById(divId).appendChild(iFrame);
		});
	};

	//Adicionando o iframe na página
	addiFrame('myiFrame', 'http://example.com');

</script>
</body>
</html>
```


