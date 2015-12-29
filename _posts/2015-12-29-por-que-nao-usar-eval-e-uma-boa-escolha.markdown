---
title: "Por que não usar eval é uma boa escolha?"
date:   2015-12-29 18:55:00
description: Vamos falar sobre o outro lado da nossa querida função eval, um recurso que bastante gente curte usar, mas que não faz tão bem assim para a performance do nosso código.
---

Embora seja conhecida como uma forma fácil e prática de avaliar expressões, criar variáveis e funções em tempo de execução, a função **eval** também é tida como uma maneira deselegante de ~~macetar~~ modificar o escopo léxico (prometo mais uma vez que vou falar detalhadamente sobre escopo num post futuro).

Javascript é uma linguagem de escopo léxico, ou seja, a primeira parte da etapa de compilação é o *tokenizing*, um recurso que as linguagens utilizam para transformar a string do código fonte em *tokens*.

O escopo léxico é definido na parte léxica da compilação. Parece redundante, mas é isso mesmo. É nele que ficam as variáveis e funções definidas pelo desenvolvedor.

Acontece que a mesma função **eval** que torna algumas coisas mais simples e dinâmicas pode ser uma grande vilã. Ela é capaz de modificar o escopo léxico após a fase de *tokenizing*, ou seja, pode inserir coisas que o compilador vai ter trabalho de saber lidar bem. Vamos ver um exemplo.

O que acontece aqui?

```javascript
function foo(str, a) {
  eval(str);
  console.log(a, b);
}

var b = 2;

foo("var b = 3;", 1);
```

A função **foo** espera dois parâmetros. O primeiro é uma string que ela utiliza na chamada da função **eval**, que recebe uma expressão e a avalia, tornando-a código.

Nesse exemplo existe uma variável **b** sendo declarada no escopo global, variável esta que não será utilizada, pois a expressão que estamos passando como parâmetro para a função **foo** é justamente uma nova declaração de uma variável **b** que fará parte do escopo de **foo**.

Mas o que isso traz de ruim, se agora eu consigo criar variáveis e funções, injetando coisas no escopo léxico em tempo de execução? Performance! Essa é a palavra chave aqui. A compilação de Javascript tende a otimizar o código. Faz *hoisting*, busca e verifica a existência de variáveis e funções em escopos aninhados e utiliza desse recurso para saber quais decisões tomar quando encontra declarações e/ou utilizações de variáveis ao longo do código. Ela segue esses procedimentos para garantir que a execução seja rápida.

Então quando você usa **eval**, a *Engine* não vai saber se aquele código Javascript que você passou como parâmetro foi gerado pelo compilador ou se foi injetado (e alterou o escopo léxico). O que pode trazer lentidão e algumas dores de cabeça.

No *strict-mode*, você não é capaz de alterar o escopo léxico da função que engloba **eval**.

```javascript
function foo(str) {
  "use strict";
  eval(str);
  console.log(a); // ReferenceError: a is not defined
}

foo("var a = 2");
```

A expressão que vem como parâmetro de **eval** declara uma variável **a**. Dentro da função foo ela não existe, porque o *strict-mode* limita essas alterações de escopo.

Bastantes desenvolvedores Javascript aconselham que o uso de **eval** seja evitado, mesmo em *strict-mode*.
