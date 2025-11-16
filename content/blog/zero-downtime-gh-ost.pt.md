---
title: "Migrações de Banco de Dados sem Tempo de Inatividade com gh-ost: Um Essencial para Produção"
date: 2025-11-18T15:30:00-06:00
featured: true
draft: false
comment: false
toc: true
reward: false
pinned: false
carousel: true
categories:
  - blog
tags:
  - database
  - mysql
  - gh-ost
  - migrations
  - zero-downtime
authors:
  - oss.lat
images:
  - /images/blog/gh-ost.png
---

No mundo sempre ativo de hoje, deixar seu banco de dados de produção offline para uma alteração de esquema é muitas vezes inaceitável. Mesmo alguns segundos de inatividade podem significar perda de receita, usuários frustrados ou serviços interrompidos. Isso é especialmente verdadeiro para bancos de dados MySQL grandes e com tráfego intenso.

As operações tradicionais `ALTER TABLE` no MySQL podem ser bloqueadoras, o que significa que elas bloqueiam a tabela e impedem gravações (e às vezes leituras) durante a operação. Para tabelas grandes, isso pode levar minutos ou até horas.

É aqui que entra o **`gh-ost`** (GitHub Online Schema Transit). Desenvolvido pelo GitHub, o `gh-ost` é uma ferramenta de migração de esquema online baseada em gatilhos e testada em batalha para o MySQL. Ele permite que você execute alterações complexas de esquema em tabelas grandes com **absolutamente zero tempo de inatividade** para sua aplicação.

### Como o gh-ost Atinge Zero Tempo de Inatividade?

O `gh-ost` funciona criando uma cópia da sua tabela, aplicando as alterações de esquema à cópia e, em seguida, sincronizando sutilmente os dados antes de trocar atomicamente as tabelas. Aqui está um resumo simplificado:

1.  **Criar uma Tabela "Fantasma":** O `gh-ost` cria uma *tabela fantasma* vazia com o novo esquema, idêntica à sua tabela original, mas com as modificações desejadas.
2.  **Aplicar Gatilhos:** Em seguida, ele cria **gatilhos** em sua *tabela original* para as operações `INSERT`, `UPDATE` e `DELETE`. Esses gatilhos capturam as alterações feitas na tabela original e as aplicam à tabela fantasma.
3.  **Copiar Dados Existentes:** Em segundo plano, o `gh-ost` copia todos os dados existentes da sua tabela original para a tabela fantasma em pequenos pedaços gerenciáveis. Este processo é controlado para minimizar o impacto em seu banco de dados.
4.  **Sincronização Contínua:** À medida que os dados são copiados, os gatilhos continuam a encaminhar quaisquer novas alterações da tabela original para a tabela fantasma, garantindo que a tabela fantasma permaneça atualizada.
5.  **Corte (Troca Atômica):** Assim que a tabela fantasma estiver totalmente sincronizada, o `gh-ost` executa uma **troca atômica** usando uma operação `RENAME TABLE`. Esta é uma operação extremamente rápida e sem bloqueio que essencialmente renomeia sua tabela original para um nome descartado e renomeia a tabela fantasma para o nome da sua tabela original. Sua aplicação começa a usar a nova tabela imediatamente.
6.  **Limpeza:** A tabela original antiga (agora renomeada) pode então ser descartada.

### Principais Benefícios do gh-ost

*   **Zero Tempo de Inatividade:** Este é seu principal e mais significativo benefício. Sua aplicação permanece totalmente disponível durante toda a migração.
*   **Segurança e Controle:**
    *   **Mecanismos de Aceleração:** O `gh-ost` monitora ativamente a carga do seu banco de dados (atraso de replicação, uso de CPU, etc.) e desacelera suas operações se o banco de dados ficar muito ocupado.
    *   **Ganchos:** Você pode definir ganchos personalizados (scripts) a serem executados em vários estágios da migração, permitindo automação e validação avançadas.
    *   **Cancelamento Gracioso:** Você pode cancelar uma migração com segurança em quase qualquer ponto, revertendo para a tabela original sem perda de dados.
    *   **Somas de Verificação e Verificação:** Ele pode realizar verificações de integridade de dados.
*   **Não Bloqueador:** Evita bloqueios de tabela de longa duração.
*   **Auditável:** Fornece registros detalhados de suas operações.
*   **Testado em Batalha:** Usado pelo GitHub por anos em algumas das maiores implantações de MySQL do mundo.

### Quando Usar o gh-ost

O `gh-ost` é ideal para:
*   Grandes tabelas de produção do MySQL onde um `ALTER TABLE` causaria tempo de inatividade inaceitável.
*   Alterações complexas de esquema (adicionar colunas, alterar tipos de coluna, adicionar/remover índices, etc.).
*   Ambientes onde a implantação contínua e a alta disponibilidade são críticas.

### Começando

O `gh-ost` é uma ferramenta de código aberto, geralmente instalada como um binário Go. Você o executa de uma máquina cliente (ou um servidor de migração dedicado) que tenha acesso de rede ao seu banco de dados MySQL.

```bash
# Exemplo de comando gh-ost para adicionar uma coluna
gh-ost \
  --host=seu_host_de_bd \
  --port=3306 \
  --user=seu_usuario \
  --password=sua_senha \
  --database=seu_banco_de_dados \
  --table=sua_tabela \
  --alter="ADD COLUMN nova_coluna VARCHAR(255) NOT NULL DEFAULT ''" \
  --execute
```

