---
# type: docs
title: Como configurar tu entorno de desarrollo
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

Ambiente de desarrollo OpenInfraMX.

<!--more-->

# Introduccion

Esta guía te va a ayudar a configurar tu entorno de desarrollo; sigue esta guía para ayudar a OpenInfraMX.

## Linux

### Pre-requisitos

| Herramienta                                                                                                             | Version                                 | Descripcion                                                                |
| ----------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | -------------------------------------------------------------------------- |
| [Git](https://git-scm.com/downloads)                                                                                    |                                         | Systema de control de versiones.                                           |
| [Hugo](https://gohugo.io/installation/)                                                                                 | **extended** version `0.97.0` or above. | El tema esta construido con esto.                                          |
| [Node.js](https://nodejs.org/en/download/) and [NPM](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) | NODEJS `16` or above                    | usado para instalar CSS y dependencias de JS, y para construir los assets. |
| [GO](https://go.dev/doc/install)                                                                                        | `1.12` or above                         | se recomienda tenerlo al usar Hugo                                         |

### Instalación

1. Clona el [repositorio](https://github.com/opensource-latinamerica/openinframx) de OpenInfraMX y los submodulos necesarios

```bash
git clone --recurse-submodules --depth 1 https://github.com/opensource-latinamerica/openinframx
```

2. Instala dependencias de npm

```bash
npm install
```

3. Inicia el servidor

```bash
hugo server
```

# Palabras finales

Si seguiste los pasos hasta aqui, ve al [github](https://github.com/opensource-latinamerica/openinframx) y ayuda en lo que gustes. se puede crear algo divertido.
