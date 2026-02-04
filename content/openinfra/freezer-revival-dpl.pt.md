---
title: "Saindo do Congelador: Como a Liderança Distribuída está Revivendo um Projeto OpenStack"
date: 2026-02-03T00:00:00-06:00
featured: false
draft: false
comment: false
toc: true
reward: false
pinned: false
carousel: false
categories:
  - openinfra
tags:
  - openstack
  - freezer
  - governança
  - dpl
authors:
  - oss.lat
images:
  - /images/blog/openinfra/freezer-dpl.webp
---

O OpenStack é um vasto ecossistema de projetos, cada um com seu próprio ciclo de vida. Alguns projetos amadurecem e se tornam pilares da plataforma, enquanto outros, com o tempo, podem ver um declínio na atividade. Um desses projetos, o **Freezer**, o serviço OpenStack para backup, restauração e recuperação de desastres, estava nesse caminho, aproximando-se de um estado de obsolescência.

No entanto, um modelo de governança flexível dentro do OpenStack proporcionou uma nova vida ao Freezer, demonstrando como a comunidade se adapta para manter projetos valiosos vivos.

### O Desafio: O Peso da Coroa do PTL

Tradicionalmente, os projetos OpenStack são liderados por um **Líder de Equipe de Projeto (PTL)**. O PTL é um único indivíduo responsável por uma ampla gama de tarefas: gerenciar o desenvolvimento, supervisionar os lançamentos, representar o projeto e muito mais. Este é um papel exigente e, para projetos com um grupo menor de contribuidores ativos, encontrar alguém disposto e capaz de assumir o papel de PTL pode ser um desafio significativo. O fardo do papel de PTL pode ser um gargalo, retardando ou até mesmo interrompendo o progresso de um projeto.

### Um Novo Modelo: Liderança de Projeto Distribuída (DPL)

Para resolver isso, o Comitê Técnico do OpenStack (TC) estabeleceu o modelo de **Liderança de Projeto Distribuída (DPL)**. O DPL é uma alternativa ao PTL único, permitindo que as responsabilidades de liderança sejam distribuídas entre vários contribuidores, conhecidos como **representantes (liaisons)**.

Esses representantes cobrem áreas específicas, como:
*   **Representante de lançamentos (Release liaison):** Gerencia o processo de lançamento.
*   **Representante do TaCT-SIG (TaCT-SIG liaison):** (anteriormente Representante de Infra) - um ponto de contato para questões de CI/CD e infraestrutura.
*   **Representante de segurança (Security liaison):** Lida com questões relacionadas à segurança.
*   **Representante do TC (TC liaison):** Um membro do Comitê Técnico que atua como mentor e ponto de contato.

Este modelo é menos oneroso para um único indivíduo e capacita um grupo mais amplo de contribuidores a assumir a responsabilidade pelo sucesso do projeto.

### O Descongelamento do Freezer: Salvo pelo DPL

O Freezer vinha lutando para manter o ritmo, com seu último grande lançamento datando de vários anos. O projeto corria o risco de ser "congelado" indefinidamente. Ao adotar o modelo DPL, a equipe do Freezer conseguiu distribuir as responsabilidades de liderança, facilitando o gerenciamento do projeto com a comunidade existente. Essa medida foi fundamental para "descongelar" o projeto, permitindo que o desenvolvimento e a manutenção continuassem.

### Uma Escolha Consciente pela Colaboração

Recentemente, o TC do OpenStack implementou uma nova política: o status de DPL para todos os projetos é redefinido no início de cada ciclo de desenvolvimento. Os projetos que desejam continuar com o modelo DPL devem optar ativamente por ele novamente.

Essa política, embora pareça um pequeno passo burocrático, é um movimento significativo. Ela garante que os projetos escolham consciente e deliberadamente sua estrutura de liderança a cada ciclo. Incentiva a participação ativa dos representantes e da equipe do projeto, evitando que o modelo DPL se torne um estado passivo.

Para projetos como o Freezer, isso significa que a comunidade reafirmará regularmente seu compromisso com o projeto e seu modelo de liderança escolhido. É um testemunho da flexibilidade da governança do OpenStack e da dedicação da comunidade em preservar projetos valiosos. O modelo DPL não apenas salvou o Freezer da beira do abismo; ele forneceu um caminho sustentável para o futuro, construído sobre colaboração e responsabilidade compartilhada.
