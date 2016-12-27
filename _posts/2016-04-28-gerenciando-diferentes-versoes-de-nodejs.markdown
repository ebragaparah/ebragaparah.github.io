---
title: "Gerenciando diferentes versões de Node.js"
date:   2016-12-27 09:45:00
description: Gerenciadores de versão são bastante úteis. Quase todo mundo curte e utiliza. Então por que não utilizar essa praticidade para gerenciar suas versões de Node.js?
---

Há um tempo eu experimentado uma ferramenta bastante útil que me permite instalar mais de uma versão de Node.js simultaneamente e alternar entre versões quando eu bem entender, de uma forma muito fácil. O gerenciador que eu uso é o NVM (_Node Virtual Manager_).

A instalação é muito simples e os passos podem ser encontrados na própria [documentação](https://github.com/creationix/nvm).

A utilização do NVM é muito simples, quem já é familiarizado com RVM ou Rbenv vai sentir uma semelhança. Para instalar uma nova versão do Node.js, basta digitar

```
$ nvm install 6.0
```

Se você quiser alternar para uma versão já instalada, só precisa fazer

```
$ nvm use 5.0
```

Ops, mas e se eu me esqueci de quais versões eu tenho instaladas? Moleza, você tem como listar suas versões usando

```
$ nvm list
```

Para desinstalar alguma versão o trabalho também é muito simples, o comando é

```
nvm uninstall 5.0
```

Esses são os comandos mais usuais, eu não precisei de nenhum outro até agora. A documentação é simples e completa. Além disso, dá pra usar 

```nvm help```

caso você precise se lembrar de algum dos comandos e/ou descobrir outros.

Vale a pena experimentar.
