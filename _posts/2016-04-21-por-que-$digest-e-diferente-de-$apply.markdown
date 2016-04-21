---
title: "Por que $digest é diferente de $apply em AngularJS?"
date:   2016-04-21 01:00:00
description: Funções parecidas podem causar um pouco de confusão, perda de tempo, paciência e performance quando não entendemos partes minuciosas. Vamos conhecer os detalhes de cada uma, para fazer um uso mais preciso.
---

Bastante gente usa, mas muitas vezes sem saber as diferenças sutis entre as duas estruturas, que por sinal, são um pouco semelhantes. A intenção é fazer um post pequeno, mas esclarecedor.

Ambas **$apply** e **$digest** são funções que "confirmam" alterações feitas em escopos, porém, enquanto a primeira inicia sua atuação no $rootScope, – que é o escopo principal e tem todos os outros escopos como filhos – e propaga a alteração para todos os escopos filhos, a segunda inicia sua ação no escopo no qual é invocada e apenas propaga para seus filhos. Confuso? Acho que uma olhada no código ajuda a entender um pouco melhor.

Considerando que nós já tenhamos importado o AngularJS, o código seguinte mostra a criação de um controller qualquer dentro de um módulo. Nele, definimos uma mensagem diretamente no escopo principal e uma mensagem no escopo do nosso controller.

```javascript
'use strict';

angular.module('myApp', [])
  .controller('myCtrl', function($rootScope, $scope) {
    $rootScope.rootMessage = 'Mensagem inicial do rootScope';
    $scope.childMessage = 'Mensagem inicial do childScope';
  });
```

No exemplo abaixo, criamos uma função `changeChildScope` que altera a mensagem que nós inserimos anteriormente no escopo do controller, o escopo filho e em seguida chama a função `$digest`, que por sua vez, vai aplicar a mudança apenas no escopo no qual ela foi chamada e nos filhos deste escopo.

```javascript
'use strict';

angular.module('myApp', [])
  .controller('myCtrl', function($rootScope, $scope) {
    $rootScope.rootMessage = 'Mensagem inicial do rootScope';
    $scope.childMessage = 'Mensagem inicial do childScope';
  });
  
  (function changeChildScopeMessage() {
    $rootScope.rootMessage = 'Nova mensagem do rootScope';
    $scope.childMessage = 'Nova mensagem do childScope';

    $scope.$digest();
  })();
```
Ou seja, o **$rootScope** não vai mudar.


O trecho abaixo é semelhante ao anterior, diferindo apenas no fato de que chamamos a função `$apply` em vez de `$digest`, para que as mudanças desejadas sejam observadas partindo do **$rootScope** para seus filhos.


```javascript
'use strict';

angular.module('myApp', [])
  .controller('myCtrl', function($rootScope, $scope) {
    $rootScope.rootMessage = 'Mensagem inicial do rootScope';
    $scope.childMessage = 'Mensagem inicial do childScope';
  });
  
  (function changeRootScopeAndChildScopeMessages() {
    $rootScope.rootMessage = 'Nova mensagem do rootScope';
    $scope.childMessage = 'Nova mensagem do childScope';

    $scope.$apply();
  })();
```
O que faz que agora as duas mensagens mudem.

Caso você queira observar as mudanças acontecendo, basta criar um esqueleto de HTML como o mostrado em seguida. 

{% raw %}
```html
<body>
    <div ng-app="myApp">
        {{ rootMessage }}
        <div ng-controller="myCtrl">
            {{ childMessage }}
        </div>
    </div>
</body>
```
{% endraw %}

Para poder notar com uma velocidade mais lenta, pode inclusive forçar a chamada das funções **changeChildScopeMessage** e **changeRootScopeAndChildScopeMessages** usando uma **setTimeout** com um tempo de sua preferência.

Fiquei motivado a escrever este post após ter assistido [este vídeo da Toptal](https://www.youtube.com/watch?v=LgDgazlr_Kw), que é bastante sucinto e direto. Os exemplos de código foram retirados de lá.

Isso é tudo, pessoal. Comentem aí, vamos trocar uma ideia.