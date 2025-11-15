---
# type: docs
title: How to Set Your Development Enviroment
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

OpenInfraMX's development enviroment.

<!--more-->

# Getting Started

This guide will help you to set your development enviroment, follow this guide in case you want to help OpenInfraMX.

## Linux

### Pre-requisites

| Tool                                                                                                                    | Version                                 | Description                                                           |
| ----------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | --------------------------------------------------------------------- |
| [Git](https://git-scm.com/downloads)                                                                                    |                                         | Version Control System.                                               |
| [Hugo](https://gohugo.io/installation/)                                                                                 | **extended** version `0.97.0` or above. | The theme is built using this.                                        |
| [Node.js](https://nodejs.org/en/download/) and [NPM](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) | NODEJS `16` or above                    | used for installing CSS and JS dependencies, and building the assets. |
| [GO](https://go.dev/doc/install)                                                                                        | `1.12` or above                         | recommended to have when using Hugo                                   |

### Installation

1. Clone the OpenInfraMX's [repository](https://github.com/opensource-latinamerica/openinframx) and its submodules

```bash
git clone --recurse-submodules --depth 1 https://github.com/opensource-latinamerica/openinframx
```

2. Install npm dependencies

```bash
npm install
```

3. Start the server

```bash
hugo server
```

# Final Words

Any contribution is welcome, feel free to reach us through our [GitHub repository](https://github.com/opensource-latinamerica/openinframx). This could be an amazing journey.
