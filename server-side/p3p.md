#Cabeçalho P3P

P3P é a sigla para *Platform for Privacy Preferences*, um padrão desenvolvido para comunicar de forma rápida e automática a política de privacidade de um site ao usuário e permitir que ele a compare com suas preferências e acesso aos cookies.

Se esse cabeçalho não for presente na página o Internet Explorer pode bloquear a utilização de cookies, principalmente em iframes.

O usuário é informado do bloqueio com um ícone na barra de status do navegador conforme a imagem abaixo.

<img src="../images/p3p_cookie_bloqueado.jpg" alt="P3P ícone de cookies bloqueados"/>

##Como definir o cabeçalho P3P

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
