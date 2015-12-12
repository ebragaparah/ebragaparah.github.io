---
title: "Hoisting de uma maneira descomplicada"
date:   2015-12-12 15:20:00
description: Neste post vou mostrar como funciona uma das características de Javascript que fazem alguma confusão na cabeça de quem está chegando na linguagem agora. O objetivo do post é ilustrar como o hoisting ocorre.
---

Uma coisa bastante comum a quase todo mundo que está chegando no mundo Javascript é a confusão que é feita com alguns conceitos e/ou características de funcionamento da linguagem. Uma dessas características é o hoisting.

Algumas pessoas são pegas de surpresa quando executam alguns trechos de código, esperando que este trecho retorne um valor, mas por alguma razão, o trecho retorna algo inesperado. Vamos ver um exemplo bastante clássico:

```javascript
var a = 25;

(function foo() {
  console.log(a);
  var a = 34;
  console.log(a);
})();
```

```
> undefined
> 34
```

Ué.

Mas por que a primeira chamada à minha variável não retorna um 25, como eu esperava?

Então, isso é o hoisting acontecendo.

Em outras palavras, em Javascript as declarações de variáveis (e também de funções, mas vamos falar disso em outro post) são elevadas ao topo do escopo. Por isso, nós somos capazes de utilizar a variável ***a*** sem algum ***Uncaught ReferenceError***.

Nesse exemplo em específico não teríamos uma exceção de referência porque existe uma variável **a** sendo declarada fora do escopo da função **foo** e essa variável é herdada pelo escopo aninhado (vamos falar de escopo em outro post, eu prometo).

É **bastante importante** frisar que, apenas a declaração é elevada ao topo do escopo, e não a atribuição de valores. Exatamente por isso temos um retorno ***undefined*** na primeira chamada à variável.

Isso pode ser melhor visto nessa sequência que tem como objetivo ilustrar os passos que ocorrem.

```javascript
//passo 1
var a = 25;

(function foo() {
  console.log(a);
  var a = 34;
  console.log(a);
})();
```

A declaração da variável **a** é levada ao início do escopo da função **foo**, dessa maneira:

```javascript
//passo 2
var a = 25;

(function foo() {
  var a; //agora a função foo tem uma variável a própria
  console.log(a);
  a = 34;
  console.log(a);
})();
```

Eu particularmente acho bastante interessante que Javascript faça hoisting em tempo de compilação. A gente só precisa saber que isso ocorre e precisa estar atento a essas particularidades da linguagem para evitar problemas maiores.

Bom, por hoje é isso. Se algo não ficou claro ou se eu escrevi bobagem, comenta aí. Vamos conversar! :)
