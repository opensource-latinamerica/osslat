---
title: "Depuração com eBPF 101: Um Guia para Iniciantes"
author: oss.lat
date: 2026-01-27
description: "Uma introdução amigável para iniciantes ao eBPF para depuração e rastreamento no Linux."
images:
    - images/blog/ebpf-debugging-basics.png
tags:
    - eBPF
    - Depuração
    - Linux
    - Rastreamento
    - Iniciante
series: ["Depuração com eBPF"]
---

Bem-vindo à primeira postagem de nossa série sobre eBPF para depuração! Nesta postagem, apresentaremos os conceitos básicos do eBPF e mostraremos como começar com ferramentas simples, mas poderosas, para rastrear и depurar seus sistemas Linux.

### O que é eBPF?

O eBPF (extended Berkeley Packet Filter) é uma tecnologia revolucionária que permite executar programas em sandbox no kernel do Linux sem alterar o código-fonte do kernel ou carregar módulos do kernel. Pense nele como uma máquina virtual leve no kernel que fornece uma maneira segura e eficiente de adicionar novos recursos ao kernel em tempo de execução.

Originalmente focado na filtragem de pacotes de rede, o eBPF evoluiu para se tornar uma ferramenta de uso geral para uma ampla gama de casos de uso, incluindo:
- **Redes:** Processamento e filtragem de pacotes de alto desempenho.
- **Observabilidade:** Coleta de métricas e eventos de desempenho detalhados.
- **Segurança:** Monitoramento de chamadas de sistema e atividade de rede para aplicar políticas de segurança.
- **Rastreamento e Depuração:** Obtenção de insights profundos sobre o comportamento do kernel e do aplicativo.

### Configurando Seu Ambiente

Para começar com o eBPF, você precisará de uma distribuição Linux moderna. Para este guia, focaremos nas ferramentas disponíveis no Ubuntu 24.04 e posterior, onde muitas ferramentas eBPF são incluídas por padrão.

Os dois principais conjuntos de ferramentas que usaremos são:
- **BCC (BPF Compiler Collection):** Uma coleção de ferramentas de linha de comando poderosas e um framework Python para criar programas eBPF personalizados.
- **`bpftrace`:** Uma linguagem de rastreamento de alto nível que facilita a escrita de scripts de rastreamento eBPF personalizados com uma sintaxe simples.

Você pode instalar essas ferramentas no Ubuntu com:
```bash
sudo apt-get update
sudo apt-get install -y bcc bpftrace
```

### Seus Primeiros Rastreamentos com `bpftrace`

`bpftrace` é a ferramenta perfeita para iniciantes começarem a explorar o eBPF. Ele fornece uma sintaxe simples, semelhante ao `awk`, para escrever one-liners e pequenos scripts para rastrear eventos do kernel e do espaço do usuário.

Vamos tentar alguns exemplos básicos:

**1. Rastreando novos processos:**
Este one-liner imprimirá o nome de cada novo processo que for executado no sistema.
```bash
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_execve { printf("%s\n", comm); }'
```

**2. Contando chamadas de sistema:**
Este comando contará todas as chamadas de sistema e imprimirá um resumo a cada segundo.
```bash
sudo bpftrace -e 'tracepoint:raw_syscalls:sys_enter { @[probe] = count(); } interval:s:1 { print(@); }'
```

**3. Rastreando chamadas de sistema `open()`:**
Isso rastreará todas as chamadas de sistema `open()`, mostrando o nome do processo e o arquivo que está sendo aberto.
```bash
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_openat { printf("%s %s\n", comm, str(args->filename)); }'
```

Estes são apenas alguns exemplos simples, mas demonstram o poder e a simplicidade do `bpftrace`. Na próxima postagem desta série, exploraremos técnicas mais avançadas e apresentaremos as ferramentas da coleção BCC. Fique ligado!
