---
title:  "Construindo uma API RESTful usando Node.js e Express para servir outras aplicações"
date:   2015-09-11 12:00:00
description: Vamos criar uma API RESTful usando Node.js e Express de forma que sejamos capazes de servir outras aplicações. 
---

Oi, pessoal! Hoje eu vou mostrar como é fácil utilizar o Node.js para construir uma API para servir outras aplicações.

Uma API (Application Programming Interface), como o nome diz, são interfaces que servem para conectar uma aplicação à outra. Portas de saída e de entrada de uma aplicação, é como uma aplicação consegue enviar e receber dados de outra. Calma que você já vai entender.

Vamos construir nossa API e para isso vamos utilizar o Node.js e o Express

O primeiro passo seria instalar o [Node](https://www.nodejs.org), usando:

```
$ brew install node
```
Isso já vai instalar o Node e o [npm](https://www.npmjs.com), que é o nosso gerenciador de pacotes Node.

Ou, se você estiver usando Linux:

```
$ sudo apt-get update
$ sudo apt-get install nodejs
$ sudo apt-get install npm
```

Feito isso, vamos ao próximo passo, que é criar nossa estrutura de arquivos. A estrutura que estou usando funciona legal para mim, deixa as coisas um pouco organizadas. Crie um diretório que conterá o nosso projeto:

```
$ mkdir api_node
```

E damos nosso primeiro passo, que é iniciar uma aplicação node dentro do nosso diretório e instalar o express e o body-parser via npm:

```
$ cd api_node
$ npm install --save express
$ npm install --save body-parser
$ npm init
```

O comando npm init vai criar para nós um arquivo package.json, que servirá para listar nossas dependências neste projeto. Assim, quando compartilharmos este projeto, as pessoas não precisarão buscar cada uma das dependências manualmente, esse arquivo vai automatizar a instalação delas. 

Crie um arquivo server.js, que servirá como nosso servidor e conterá todas as nossas definições de configurações e de rotas.

Dentro do arquivo server.js, importe as dependências:

```javascript
var express = require('express');
var bodyParser = require('body-parser');
```

O express é quem vai nos prover toda a infraestrutura de rotas sem com que nós precisemos configurar tudo manualmente. O bodyparser vai nos permitir parsear json e x-www-urlenconded.

O express cria bastantes funcionalidades para nós, mas não configura tudo o que precisa estar pronto para que nossa aplicação rode lisinha. Vamos então criar a nossa app, que vai ser uma espécie de instância do express, configurar a porta que desejamos utilizar e criar o nosso roteador:

```javascript
var app = express();
var port = process.env.PORT || 3000
var router = express.Router();

app.use(bodyParser.urlenconded({ extended: true });
app.use(bodyParser.json());
```
Aproveitando o ritmo de configurações, vamos adicionar a parte em que dizemos para a nossa aplicação em qual porta ela vai ficar escutando e um prefixo que diz qual será a porta de entrada da nossa API:

```javascript
app.use('/api', router);
app.listen(port, function() {
  console.log('Insira cervejas na porta ' + port);
});
```
```javascript
```

Como não estamos usando nenhum banco de dados, vamos armazenar nossos dados como objetos Javascript.

```javascript
var cervejas = [
  { nome: 'Cerveja1' },
  { nome: 'Cerveja2' },
  { nome: 'Cerveja3' }
];
```

Seria legal também que criássemos um watcher para nos mostrar no terminal quando nossa aplicação mudou de estado, quando ocorreu alguma mudança, para isso, fazemos:

```javascript
router.use(function(request, response, next) {
  console.log('Algo mudou aqui...');
  next();
});
```

Já temos configurações suficientes, vamos começar a criar nossas primeiras rotas para ver algo funcionando? Sim, vamos lá.

```javascript
router.get('/', function(request, response) {
  response.json({ message: 'Esta é a rota base da nossa API' });
});
```

Iremos ter que mapear verbos HTTP para cada rota que queiramos tornar válida na nossa API, portanto, uma coisa interessante é criar variáveis para guardar essas rotas que serão mais utilizadas: a rota que traz todas as cervejas, a rota que traz apenas uma cerveja e a rota que nos permitirá editar uma cerveja posteriormente:

```javascript
var cervejasRoute = router.route('/cervejas');
var cervejaRoute = router.route('/cerveja/:id');
var editCervejaRoute = router.route('/cerveja/:id/edit');
```

Vamos então tratar de trazer todas as nossas cervejas:

```javascript
cervejasRoute.get(function(request, response) {
  send.json(cervejas);
});
```

Apenas com esse bloco, já podemos visualizar algum json contendo todas as nossas cervejas. Eu usei o Postman para me auxiliar com essa tarefa, mas, você pode utilizar:

Ficamos livres então para rodar o nosso servidor e ver a mágica acontecendo.

```
node server.js
```
e, em outra aba do Terminal:

```
curl -X GET http://localhost:3000/api/cervejas
```

Se você for utilizar o Postman, vai ser mais fácil, só precisa selecionar as opções de acordo com a sua vontade.

Tudo funcionando? Que tal adicionar as outras actions da nossa API? Já temos **GET** e **POST** para nossa rota /cervejas, que tal se formos capazes de visualizar uma cerveja de cada vez?

Já temos uma rota definida para trazer uma cerveja para nós, basta que façamos uma action **GET** na nossa **cervejaRoute**, certo?

```javascript
cervejaRoute.get(function(request, response) {
  var cerveja = cervejas[response.params.id - 1];
  response.json(cerveja);
});
```

O que está acontecendo aqui é semelhante ao que ocorre no **GET** que fazemos com todas as cervejas, só que aqui precisamos capturar uma cerveja em específico, adicioná-la ao corpo da resposta e mandar renderizar como json.

E para deletar uma cerveja? Fácil também. Usando a rota **cervejaRoute**, vamos chamar o método delete do express e passar como parâmetro a cerveja que desejamos deletar.

```javascript
cervejaRoute.delete(function(request, response) {
  var cerveja = cerveja[response.params.id];
  cervejas.splice(cerveja, 1);
  response.json(true);
});
```
Agora já temos nossa API respondendo aos métodos GET, POST e DELETE.

Bom, por hoje é isso. Em posts futuros eu pretendo falar um pouco mais detalhadamente e unir essa API com algo que possa consumir os dados dela. Persistir esses dados num banco seria interesante também.

Tendo qualquer problema, podem entrar em contato. Até já.
