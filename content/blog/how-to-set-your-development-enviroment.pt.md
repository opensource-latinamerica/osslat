---
# type: docs
title: Como Configurar Seu Ambiente de Desenvolvimento
date: 2023-11-06T17:38:08-06:00
featured: false
draft: false
comment: false
toc: true
reward: false
pinned: false
carousel: false
categories:
  - blog
tags:
  - development
authors: 
  - alejandro.delafuente
images:
  [
    "/images/blog/Open_Source_Software.jpg"
  ]
---

Ambiente de desenvolvimento do OpenInfraMX.

<!--more-->

# Começando

Este guia irá ajudá-lo a configurar seu ambiente de desenvolvimento, siga este guia caso queira ajudar o OpenInfraMX.

## Linux

### Pré-requisitos

| Ferramenta                                                                                                              | Versão                                  | Descrição                                                              |
| ----------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | ---------------------------------------------------------------------- |
| [Git](https://git-scm.com/downloads)                                                                                    |                                         | Sistema de Controle de Versão.                                         |
| [Hugo](https://gohugo.io/installation/)                                                                                 | versão **extended** `0.97.0` ou superior. | O tema é construído usando isso.                                       |
| [Node.js](https://nodejs.org/en/download/) e [NPM](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) | NODEJS `16` ou superior                 | usado para instalar dependências de CSS e JS, e construir os assets. |
| [GO](https://go.dev/doc/install)                                                                                        | `1.12` ou superior                      | recomendado ter ao usar o Hugo                                         |

### Instalação

1. Clone o [repositório](https://github.com/opensource-latinamerica/openinframx) do OpenInfraMX e seus submódulos

```bash
git clone --recurse-submodules --depth 1 https://github.com/opensource-latinamerica/openinframx
```

2. Instale as dependências do npm

```bash
npm install
```

3. Inicie o servidor

```bash
hugo server
```

# Palavras Finais

Qualquer contribuição é bem-vinda, sinta-se à vontade para nos contatar através do nosso [repositório no GitHub](https://github.com/opensource-latinamerica/openinframx). Esta pode ser uma jornada incrível.
