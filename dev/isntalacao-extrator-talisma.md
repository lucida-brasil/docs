# Extrator Talisma - Instalção

# 1 - Banco de dados

O Extrador Talisma (a partir daqui apenas *extrator*), utiliza um banco de dados MySQL, para gravar os dados consumidos do CRM, suba a instância do MySQL, a base de dados e a tabela de resultados são cridas pela aplicação.

Em servidores Windows recomendo a instalação do XAMPP e utilização do phpMyAdmin para validação, em ambiente de desenvolvimento utilize a ferramenta que achar mais adequada.

## 2 - Repositório

Todo o código fonte do *extrator* está em um reposótorio fechado do GitHub, certifique-se de estar logado com uma conta que tenha acesso ao WorkGroup [lucida-brasil](https://github.com/lucida-brasil) e ao time [Developers](https://github.com/orgs/lucida-brasil/teams/developers) ou [Owners](https://github.com/orgs/lucida-brasil/teams/owners).

Considerando que tenha o GitHub instaldo na máquina (versão [Windows](https://windows.github.com/), [Mac](https://mac.github.com/), ou a versão de [linha de comando](https://git-scm.com/)), clone o repositório através da interface ou pelo comando:

```
git clone https://github.com/lucida-brasil/talisma-coffee-soap
```

A pasta **talisma-coffee-soap** vai ser criada no local onde você executou o comando ou onde sua interface está configurada.

## 3 - Configuração

Todo configuração da aplicação é feita pelo arquivo config.json criado na raiz do projeto, utilize como base o arquivo **config-example.json**:

```json
{
	"mysql" : {
		"host" : "localhost",
		"user" : "root",
		"pass" : "root",
		"db" : {
			"name" : "talisma",
			"table" : "extraction"
		}
	},
	"request" : {
		"url" : "http://200.216.152.138/ReportiService/ReportSvc.asmx?WSDL",
		"action" : "http://www.talisma.com/GetReportForReportId"
	},
	"log" : true,
	"console" : true
}

```

As principais configurações aqui são os dados do MySQL, lembrando que db e table ainda vão ser ciadas, e o endereço do request, os outros dados não precisam ser alterados.

## 4 - Instalação

Baixe o node.js pelo site oficial [nodejs.org](https://nodejs.org/) e instale de acordo com as instruções no próprio site.

Para instalar as dependências do *extrator* entre no diretório do projeto e execute o comando `npm install`.

Esse processo pode demorar um pouco, todos as dependências são baixadas e instaladas na pasta node_modules.

Com as dependências instaldas, utilize o comando `node setup` para criar testar a conexão com o mysql, criar o banco, a tabela e os diretórios de log.

Quando o setup terminar, você pode fazer um teste utilizando o commando `node index -n`, depois verifique os arquivos de log e a tabela no MySQL.

## 5 - Tarefas agendadas

Crie um arquivo **task.bat** com o comando:

```
node caminho\do\projeto\talisma-coffee-soap\index.js -n
```

Com o bat criado, crie uma [tarefa agendada do windows](https://technet.microsoft.com/pt-br/library/cc748993.aspx) para executa-lo algumas vezes ao dia, inicialmente executamos 4 vezes, as 2, 12, 22 e 00hs
