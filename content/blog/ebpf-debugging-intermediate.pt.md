---
title: "Depuração com eBPF na Prática: Técnicas Intermediárias"
author: oss.lat
date: 2026-01-27
description: "Leve suas habilidades de depuração com eBPF para o próximo nível com as ferramentas BCC e scripts bpftrace mais avançados."
images:
    - images/blog/ebpf-debugging-intermediate.jpeg
tags:
    - eBPF
    - Depuração
    - Linux
    - Rastreamento
    - BCC
    - Intermediário
series: ["Depuração com eBPF"]
---

Bem-vindo de volta à nossa série de depuração com eBPF! No primeiro post, introduzimos os conceitos básicos de eBPF e `bpftrace`. Agora, é hora de mergulhar mais fundo e explorar as poderosas ferramentas oferecidas pela BPF Compiler Collection (BCC), bem como técnicas mais avançadas de `bpftrace`.

### Apresentando a BPF Compiler Collection (BCC)

BCC é um rico conjunto de ferramentas que fornece um conjunto de ferramentas de linha de comando prontas para uso para análise de desempenho e depuração. Essas ferramentas são construídas sobre o eBPF e fornecem uma interface amigável para cenários complexos de rastreamento.

A maioria das ferramentas BCC segue uma convenção de nomenclatura semelhante, terminando com `-bpfcc`. Vamos explorar algumas das mais úteis.

**1. `execsnoop-bpfcc`:**
Esta ferramenta é perfeita para entender quais processos estão sendo executados em seu sistema. É semelhante ao one-liner `bpftrace` que vimos no post anterior, mas fornece uma saída mais detalhada.

Para rastrear todos os novos processos executados pelo usuário `root`, você pode executar:
```bash
sudo execsnoop-bpfcc -Uu root -T
```
A flag `-T` adiciona um timestamp, o que é muito útil para correlacionar eventos.

**2. `opensnoop-bpfcc`:**
Se você precisa saber quais arquivos um processo está abrindo, `opensnoop-bpfcc` é a ferramenta para o trabalho. Isso é incrivelmente útil para depurar problemas de permissão ou entender os padrões de acesso a arquivos de um aplicativo.

Para rastrear todos os arquivos abertos por um processo chamado `qemu-system-x86`, você pode usar:
```bash
sudo /usr/sbin/opensnoop-bpfcc --full-path --name qemu-system-x86
```

**3. `tcplife-bpfcc`:**
Para depuração relacionada à rede, `tcplife-bpfcc` fornece um resumo das sessões TCP, incluindo o ID do processo, endereços de origem e destino e a duração da sessão.

Para rastrear todas as sessões TCP, basta executar:
```bash
sudo tcplife-bpfcc
```

### Scripts `bpftrace` Avançados

Embora as ferramentas BCC sejam ótimas para cenários comuns, às vezes você precisa escrever sua própria lógica personalizada. `bpftrace` permite que você escreva scripts mais complexos para fazer exatamente isso.

**Exemplo: Rastreando operações lentas do sistema de arquivos**

Digamos que você queira investigar operações lentas do sistema de arquivos. Você pode escrever um script `bpftrace` para medir a latência das chamadas de sistema `write()` e imprimir apenas aquelas que demoram mais do que um determinado limite.

Crie um arquivo chamado `slow_writes.bt` com o seguinte conteúdo:
```
kprobe:vfs_write
{
    @start[tid] = nsecs;
}

kretprobe:vfs_write /@start[tid]/
{
    $duration = (nsecs - @start[tid]) / 1000; // microssegundos
    if ($duration > 100) {
        printf("tid %d: escrita lenta (%d us)\n", tid, $duration);
    }
    delete(@start[tid]);
}
```

Você pode então executar este script com:
```bash
sudo bpftrace slow_writes.bt
```
Este script imprimirá uma mensagem sempre que uma operação de `write()` demorar mais de 100 microssegundos.

### O que vem a seguir?

Neste post, apenas arranhamos a superfície do que é possível com BCC e `bpftrace`. Nós o encorajamos a explorar as outras ferramentas disponíveis no pacote `bcc-tools` e a experimentar a escrita de seus próprios scripts `bpftrace`.

No post final desta série, daremos um passo adiante e mostraremos como escrever ferramentas eBPF personalizadas em Python usando o framework BCC. Fique ligado!
