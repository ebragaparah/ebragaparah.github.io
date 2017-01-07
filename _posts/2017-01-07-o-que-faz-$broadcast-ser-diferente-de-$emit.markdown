---
title: "O que faz $broadcast ser diferente de $emit em Angular.js?"
date:   2017-01-07 01:00:00
description: Funções parecidas podem causar um pouco de confusão, perda de tempo, paciência e performance quando não entendemos partes minuciosas. Vamos conhecer os detalhes de cada uma, para fazer um uso mais preciso.
---

Ambas são funções bem conhecidas por pessoas por pessoas que usam Angular.js com uma certa frequência, porém, agem de uma maneira um pouco diferente. Tanto _$broadcast_ quanto _$emit_ são funções que disparam eventos para escopos definidos em outras entidades.

A diferença está em como cada uma delas funciona. Quando usamos _$emit_ para propagar um evento, todos os escopos acima do escopo em que o evento foi disparado recebem o alerta, incluindo _$rootScope_. Quando usamos _$broadcast_, todos os escopos abaixo do escopo em que o evento foi disparado recebem o alerta.

Vamos observar melhor com um exemplo de código. Suponha que tenhamos a seguinte estrutura de controllers.

```html
{% raw %}
<div ng-app="foobar">
  <div ng-controller="MainCtrl as main">
    {{ main.msgFromSecondCtrl }}
    <div ng-controller="FirstCtrl as first">
      {{ first.data }}
      {{ first.msgFromSecondCtrl }}
      <div ng-controller="SecondCtrl as second">
        {{ second.data }}
      </div>
      <div ng-controller="ThirdCtrl as third">
        {{ third.data }}
      </div>
    </div>

    <div ng-controller="FourthCtrl as fourth">
      {{ fourth.data }}
      {{ fourth.msgFromSecondCtrl }}
      <!-- emit do segundo controller nao chega aqui -->
    </div>
  </div>
</div>
{% endraw %}
```

```javascript
var app = angular.module('foobar', []);

app.controller('MainCtrl', MainCtrl);
MainCtrl.$inject = ['$scope'];

function MainCtrl($scope) {
  var vm = this;
  $scope.$on('msgFromSecondCtrl', function(event, data) {
    vm.msgFromSecondCtrl = data;
  });
  $scope.broadcastMainMsg = function() {
    $scope.$broadcast('mainCtrlMsg', [1, 2, 3, 4]);
  };
}

app.controller('FirstCtrl', FirstCtrl);
FirstCtrl.$inject = ['$scope'];

function FirstCtrl($scope) {
  var vm = this;
  $scope.$on('mainCtrlMsg', function(event, data) {
    vm.data = data[0];
  });
  $scope.$on('msgFromSecondCtrl', function(event, data) {
    vm.msgFromSecondCtrl = data;
  });
}

app.controller('SecondCtrl', SecondCtrl);
SecondCtrl.$inject = ['$scope'];

function SecondCtrl($scope) {
  var vm = this;
  $scope.$emit('msgFromSecondCtrl', 'emit do segundo controller');
  $scope.$on('mainCtrlMsg', function(event, data) {
    vm.data = data[1];
  });
}

app.controller('ThirdCtrl', ThirdCtrl);
ThirdCtrl.$inject = ['$scope'];

function ThirdCtrl($scope) {
  var vm = this;
  $scope.$on('mainCtrlMsg', function(event, data) {
    vm.data = data[2];
  });
}

app.controller('FourthCtrl', FourthCtrl);
FourthCtrl.$inject = ['$scope'];

function FourthCtrl($scope) {
  var vm = this;
  $scope.$on('mainCtrlMsg', function(event, data) {
    vm.data = data[3];
  });
  $scope.$on('msgFromSecondCtrl', function(event, data) {
    vm.msgFromSecondCtrl = data;
  });
}

```

De forma bastante resumida, _$emit_ envia uma mensagem para escopos superiores até chegar em _$rootScope_, _$broadcast_ envia uma mensagem para todos os escopos inferiores.

Pela estrutura das tags _html_ acima, a gente consegue perceber que quem deveria receber a mensagem _msgFromSecondCtrl_ vinda de _SecondCtrl_ são os controllers que estão acima deste, _FirstCtrl_ e _MainCtrl_. Controllers que estão no mesmo nível ou num nível abaixo não recebem a mensagem.

Enquanto _MainCtrl_ dispara, com _$broadcast_, um evento em conjunto com um array contendo quatro elementos. Todos os controllers num nível abaixo devem receber o evento ao registrar um _listener_ como _$on_.
