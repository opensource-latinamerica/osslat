---
title: "Dominando a Depuração com eBPF: Scripts e Casos de Uso Avançados"
author: oss.lat
date: 2026-01-27
description: "Desbloqueie todo o potencial do eBPF escrevendo ferramentas Python personalizadas com o framework BCC e explorando casos de uso avançados."
images:
    - images/blog/ebpf-debugging-advanced.jpeg
tags:
    - eBPF
    - Depuração
    - Linux
    - Rastreamento
    - BCC
    - Python
    - Avançado
series: ["Depuração com eBPF"]
---

Bem-vindo ao post final da nossa série sobre eBPF para depuração. Cobrimos os conceitos básicos do `bpftrace` e as ferramentas de linha de comando da BPF Compiler Collection (BCC). Agora, daremos um salto à frente e aprenderemos a escrever nossas próprias ferramentas eBPF personalizadas usando Python e o framework BCC.

### Escrevendo Ferramentas Personalizadas com BCC e Python

Embora as ferramentas BCC pré-construídas sejam poderosas, o verdadeiro potencial do eBPF é desbloqueado quando você começa a escrever suas próprias ferramentas para resolver problemas específicos. O framework BCC fornece bindings Python que permitem que você escreva programas eBPF em C e interaja com eles a partir de um script Python.

Vamos escrever uma ferramenta simples que rastreia chamadas de sistema `open()`, semelhante ao `opensnoop`, mas com nossa própria saída personalizada.

Crie um arquivo chamado `myopensnoop.py` com o seguinte conteúdo:
```python
#!/usr/bin/python3
from bcc import BPF

# Programa eBPF escrito em C
bpf_program = """
#include <uapi/linux/ptrace.h>
#include <linux/fs.h>

BPF_HASH(start, u32);

int trace_open_entry(struct pt_regs *ctx, int dfd, const char __user *filename)
{
    u32 pid = bpf_get_current_pid_tgid();
    bpf_probe_read_user_str(&start, sizeof(start), (void *)filename);
    return 0;
}
"""

# Carrega o programa eBPF
b = BPF(text=bpf_program)

# Anexa o programa eBPF à chamada de sistema openat()
b.attach_kprobe(event="do_sys_openat2", fn_name="trace_open_entry")

# Imprime o cabeçalho
print("%-18s %-16s %-6s %s" % ("TIME(s)", "COMM", "PID", "FILE"))

# Processa os eventos
while True:
    try:
        (task, pid, cpu, flags, ts, msg) = b.trace_fields()
        print("%-18.9f %-16s %-6d %s" % (ts, task.decode('utf-8', 'replace'), pid, msg.decode('utf-8', 'replace')))
    except KeyboardInterrupt:
        exit()
```

Para executar este script, torne-o executável e execute-o com `sudo`:
```bash
chmod +x myopensnoop.py
sudo ./myopensnoop.py
```

Este é um exemplo simplificado, mas demonstra a estrutura básica de uma ferramenta BCC Python. Você escreve seu código C eBPF dentro de uma string Python, carrega-o com `BPF(text=...)`, anexa-o a uma sonda do kernel e, em seguida, processa os eventos em um loop.

### Casos de Uso Avançados: Segurança e Além

O eBPF não é apenas para depuração e análise de desempenho. Sua capacidade de inspecionar e controlar o comportamento do sistema no nível do kernel o torna uma ferramenta poderosa para segurança.

Por exemplo, o eBPF pode ser usado para:
- **Detectar e bloquear atividades maliciosas:** Ao monitorar chamadas de sistema, você pode detectar comportamentos suspeitos, como acesso inesperado a arquivos, conexões de rede ou execuções de processos. O artigo de `windshock.github.io` mostra como detectar backdoors baseados em eBPF.
- **Aplicar políticas de segurança:** Ferramentas como o Cilium usam o eBPF para aplicar políticas de segurança de rede no nível do kernel, fornecendo controle refinado sobre o tráfego de rede entre contêineres e microsserviços.
- **Obter visibilidade profunda para forense:** O eBPF pode fornecer uma riqueza de informações para forense digital e resposta a incidentes, permitindo que você reconstrua a linha do tempo de um ataque com um alto nível de detalhe.

### Conclusão

Esta série foi uma jornada desde o básico do `bpftrace` até a escrita de ferramentas eBPF personalizadas com Python. Esperamos que você tenha visto o quão poderoso e versátil o eBPF pode ser para depuração, análise de desempenho e segurança no Linux.

O ecossistema eBPF está em constante crescimento e evolução, por isso o encorajamos a continuar explorando e aprendendo. O site oficial do eBPF em [ebpf.io](https://ebpf.io/) é um ótimo recurso para se manter atualizado com os últimos desenvolvimentos.
