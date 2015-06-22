# KickOff - Instalação

## 1 - Banco de dados

O *KickOff* utiliza [MongoDb](https://www.mongodb.org/) como base de dados, isntale o banco de acordo a sua [documentação](http://docs.mongodb.org/manual/installation/).

Para testes recomendo a utilização do [Robomongo](http://robomongo.org/).

Nenhuma base ou coleção precisa ser criada, tudo isso será feito pelo setup do *KickOff*.


## 2 - Repositório

Todo o código fonte do *KickOff* está em um reposótorio fechado do GitHub, certifique-se de estar logado com uma conta que tenha acesso ao WorkGroup [lucida-brasil](https://github.com/lucida-brasil) e ao time [Developers](https://github.com/orgs/lucida-brasil/teams/developers) ou [Owners](https://github.com/orgs/lucida-brasil/teams/owners).

Considerando que tenha o GitHub instaldo na máquina (versão [Windows](https://windows.github.com/), [Mac](https://mac.github.com/), ou a versão de [linha de comando](https://git-scm.com/)), clone o repositório através da interface ou pelo comando:

```
git clone https://github.com/lucida-brasil/kickoff.git
```

A pasta **kickoff** vai ser criada no local onde você executou o comando ou onde sua interface está configurada.

## 3 - Configuração

Todo configuração da aplicação é feita pelo arquivo config.json criado na raiz do projeto, utilize como base o arquivo **config-example.json**:

```json
{
	"db" : {
		"connect" : "mongodb://hostname/database"
	},
	"google" : {
		"client_id" : "<google-client-id>",
		"client_secret" : "<google-client-secret>",
		"redirect_url" : "PROTOCOL://HOSTNAME:PORT/google-analytics/callback/"
	},
	"files" : {
		"dest" : "dest"
	},
	"bots" : {
		"scheduling" : {
			"default" : "0 0 1 * * *"
		}
	}
}
```

A chave **db.connect** é o endereço do banco, se for instalado na mesma maquina da aplicação utilize *mongodb://localhost/kickoff*.

O Google Client ID e Google Client Secret, podem ser obtidos pelo [api console](https://console.developers.google.com) a url de redirecionamento representa o endereço do servidor, por padrão seria **http://localhost:3000/google-analytics/callback/** essa URL é utilizada dar autorização de uma conta Google ao KickOff.

Por fim na chave **files.dest** utilize o caminho absoluto do diretório onde os arquivos do KickOff vão ser salvos, no caso do Windows utilize barras duplas, por exemplo: "C:\\kickoff\\destino\\".

> **Importante!**<br/>
> Serviços do Windows tem problemas para acessar diretórios de rede, evite unidades mapeadas e caminhos de rede.

## 4 - Instalação

Baixe o node.js pelo site oficial [nodejs.org](https://nodejs.org/) e instale de acordo com as instruções no próprio site.

Para instalar as dependências do *KickOff* entre no diretório do projeto e execute o comando `npm install`.

Esse processo pode demorar um pouco, todos as dependências são baixadas e instaladas na pasta node_modules.

Com as dependências instaldas, utilize o comando `node setup` para subir a interface web como um serviço do Windows, tente acessar [http://localhost:3000](http://localhost:3000) se não conseguir verifique se o serviço do Windows está ativo.


Isso já permite que o *KickOff* seja utilizado pela interface web, faça uma extração pela interface antes de passar para o próximo passo.

## 5 - Tarefas agendadas

Os *bots* do *KickOff* são os responsáveis pelas extrações manuais (executando o script) ou agendadas, todo bot é dividido em 2 arquivos, *[nome do bot]*.bot.js é o gatilho para execução da tarefa e *[nome do bot]*.engine.js é onde a lógica de extração está escrita.

Crie um arquivo **.bat** na raiz do projeto, nesse arquivo execute o bot, por exemplo para Google Analytics o bat deve conter `node caminho\do\projeto\kickoff\bots\novo.ga.bot.js`.

> **Importante!**<br/>
> Nunca execute mais de um bot por arquivo **.bat**

Com o bat criado, crie uma [tarefa agendada do windows](https://technet.microsoft.com/pt-br/library/cc748993.aspx) para executa-lo diariamente.
