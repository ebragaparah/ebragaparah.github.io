---
title: "Qual é a real diferença entre dois iguais e três iguais em Javascript?"
date:   2016-02-28 21:00:00
description: Barulhentos e polêmicos, os operadores == e === causam alguma confusão e indignação pra quem não está tão acostumado a usar Javascript. Mas o que realmente ocorre debaixo dos panos?
---

Com certeza, uma das coisas que chama muito a atenção para quem vem de outra linguagem e chega em Javascript são os operadores **===** e **!==**. Três iguais, como assim? Pra quê?

Eles servem para diferenciar até mesmo coisas que, para Javascript, são um pouco semelhantes. Vamos lá.

Um monte de gente define os **==** (dois iguais) como operadores que comparam dois valores, mas ignoram o seu tipo. E os **===** (três iguais) como operadores que comparam dois valores, porém levam o tipo em consideração. Esta definição explica de uma maneira reduzida e um pouco incompleta o que de fato ocorre.

O cerne da diferença é a permissividade de se fazer coerção ao comparar valores. Coerção são transformações de tipos feitas em tempo de compilação, que na minha opinião, é algo que deveria fazer parte do cinto de utilidades de todo mundo que deseja se aventurar pela linguagem.

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

O que ocorre aqui? O compilador tenta identificar os tipos dos dois operandos para que possa então avaliá-los.

```javascript
'1' == 1
```

Após identificar os tipos, o compilador tenta fazer uma combinação de transformações, convertendo um tipo no outro para que possa comparar os valores. No nosso caso, *'string'* para *'number'*, obtendo dois *'number'* ou *'number'* para *'string'*, obtendo duas *'string'*. Uma aproximação do que é feito pode ser vista no bloco de código abaixo.

```javascript
'1' == String(1) //transformando um number numa string para comparar
Number('1') == 1 //transformando uma string num number para comparar
```

Caso seja possível comparar os valores apenas com a primeira tentativa de conversão, o compilador não tentará a segunda. Esta só feita quando a primeira falha.

Caso os resultados após a coerção retornem *true*, então a comparação retorna *true*. Em caso contrário o operador retorna *false*.

Os três iguais são bem menos gentis. São a forma estrita do operador anterior. Enquanto aquele transforma os tipos, este compara os objetos diretamente, sem tentar converter tipos. O resultado é mais franco e mais simples de se observar.

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

Existe em alguns sites uma [tabela](http://dorey.github.io/JavaScript-Equality-Table/) um pouco confusa que mostra o resultado de comprações feitas com os dois iguais e resultado de comparações feitas com os três iguais. Essa tabela é grande porque Javascript possui valores que classifica como **truthy** e **falsy**. Em algumas situações pode ser interessante que tenhamos um operador que não permita coerção de tipos e em outras, o contrário.

De encontro à opinião de muita gente, coerção é algo legal, se bem utilizada. É bom saber quando utilizar dois iguais ou três iguais, dá uma maior sensação de controle sobre o seu código, você sabe de fato o que está acontecendo e não se torna dependente de *linters*.

Fico por aqui. Comenta aí, conta um pouco sobre suas experiências.
