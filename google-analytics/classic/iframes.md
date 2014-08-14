#Acompanhamento de iFrames com linker

##Passo 1 - Código de acompanhamento

O primeiro passo para o acompanhamento de iframes em domínios terceiros é alterar o código de acompanhamento do Google Analytics, essa configuração muda de acordo com a versão do GA.

Todas as chamadas da tag do GA devem contem o parâmetro `_gaq.push(['_setAllowLinker', true]);` que permite a continuidade da visita, também conhecida como cópia de cookies, entre domínios.

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

##Passo 2 - Criação do iframe

O `iframe` pode ser inserido normalmente na página, desde que esteja disponível antes da tag padrão do GA e tenha um `ID` unico na página.

``` html
<iframe id="myframe" name="myframe" src="http://example.com"></iframe>
```

##Passo 3 - Script para cópia de cookies

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

##Exemplo completo

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

#Cabeçalho P3P

P3P é a sigla para *Platform for Privacy Preferences*, um padrão desenvolvido para comunicar de forma rápida e automática a política de privacidade de um site ao usuário e permitir que ele a compare com suas preferências e acesso aos cookies.

Se esse cabeçalho não for presente na página o Internet Explorer pode bloquear a utilização de cookies, principalmente em iframes.

O usuário é informado do bloqueio com um ícone na barra de status do navegador conforme a imagem abaixo.

<img src="../../images/p3p_cookie_bloqueado.jpg" alt="P3P ícone de cookies bloqueados"/>

#Como definir o cabeçalho P3P

O cabeçalho P3P é uma definição feita do lado do servidor, abaixo exemplos de algumas formas para adicionar o P3P a sua aplicação, de acordo com a linguagem ou servidor.

###ASP

``` ASP

HttpContext.Current.Response.AddHeader("p3p", "CP=\""PSA CONo OUR ONL NOI BUS\""")
``` 

###PHP

``` PHP

header('P3P:CP="PSA CONo OUR ONL NOI BUS"');
``` 

###JSP

``` JSP

response.setHeader("P3P","CP='PSA CONo OUR ONL NOI BUS'")
``` 

###ColdFusion

``` ColdFusion

<cfheader name="P3P" value="CP='PSA CONo OUR ONL NOI BUS'" />
```

###nginx

Adicione a linha abaixo no arquivo nginx.conf

```
add_header P3P 'policyref="/w3c/p3p.xml", CP="PSA CONo OUR ONL NOI BUS", CP="CAO PSA OUR"'
```
