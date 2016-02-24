---
title: "Qual é a real diferença entre dois iguais e três iguais em Javascript?"
date:   2016-02-20 15:00:00
description: Barulhentos e polêmicos, os operadores == e === causam alguma confusão e indignação pra quem não está tão acostumado a usar Javascript. Mas o que realmente ocorre debaixo dos panos?
---

Com certeza, uma das coisas que chama muito a atenção para quem vem de outra linguagem e chega em Javascript são os operadores **===** e **!==**. Três iguais, como assim? Pra quê?

Servem para diferenciar até mesmo coisas que, para Javascript, são um pouco semelhantes. Vamos lá.

Um monte de gente define os **==** (dois iguais) como operadores que comparam dois valores, mas ignoram o seu tipo. E os **===** (três iguais) como operadores que comparam dois valores, porém levam o tipo em consideração. Esta definição explica de uma maneira reduzida e um pouco incompleta o que de fato ocorre.

O cerne da diferença é a permissividade de se fazer **coerção** ao comparar valores. Coerção é um recurso importante em Javascript, são transformações de tipos que a linguagem faz em tempo de compilação. Na minha opinião, é algo que deveria fazer parte do cinto de utilidades de todo mundo que deseja se aventurar pela linguagem.

Os dois iguais são um pouco mais flexíveis. O operador tenta fazer a conversão dos objetos que estão sendo comparados para que um tenha o tipo do outro, para então compará-los.

Alguns exemplos são

```javascript
'1' == 1 //true
'0' == false //true
'' == false //true
'' == 0 //true
'' == '0' //false
1 == '1' //true
2 == '2' //true
'abc' == new String('abc') //true
```

O que ocorre aqui? O compilador tenta identificar os tipos que estão em jogo para que possa então compará-los. Aqui neste exemplo, nós temos uma *'string'* e um *'number'*.

```javascript
'1' == 1
```

nós temos uma *'string'* e um *'number'*. O compilador identifica esses tipos, tenta transformar a *'string'* em *'number'* e analisa o resultado.
Caso não haja sucesso na conversão de ida ou de volta, o operador retorna *false*...

Já os três iguais são bem menos gentis. São a forma estrita do operador anterior. Enquanto aquele transforma os tipos, estes comparam os objetos diretamente, sem tentar converter tipos. O resultado é mais direto e mais simples de se observar.

```javascript
'1' === 1 //false
'0' === false //false
'' === false //false
'' === 0 //false
'' === '0' //false
2 === '2' //false
'abc' === new String('abc') //false
```

Percebe? Basta notar que os tipos são diferentes e você já consegue imaginar o retorno.

Existe em alguns sites uma tabela enorme que mostra o resultado de comprações feitas com os dois iguais, em comparação ao resultado usando os três iguais.

Javascript possui valores **truthy** e valores **falsy**. Por conta dessas "armadilhas", pode ser interessante que tenhamos um operador que não permita coerção de tipos para realizar a comparação.

O operador === é muito mais preciso, o == é bastante inesperado e sucetível a confusões. Por que a gente não usa === sempre?

Do contrário do que muita gente diz, coerções são coisas legais, é bom saber quando utilizar **dois iguais** ou **três iguais**, dá um maior controle sobre o seu código, você sabe de fato o que está acontecendo e não se torna dependente de *linters*.
