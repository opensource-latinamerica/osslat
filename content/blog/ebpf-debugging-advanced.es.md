---
title: "Dominando la Depuración con eBPF: Scripts y Casos de Uso Avanzados"
author: oss.lat
date: 2026-01-27
description: "Desbloquea todo el potencial de eBPF escribiendo herramientas personalizadas en Python con el framework BCC y explorando casos de uso avanzados."
images:
    - images/blog/ebpf-debugging-advanced.jpeg
tags:
    - eBPF
    - Depuración
    - Linux
    - Rastreo
    - BCC
    - Python
    - Avanzado
series: ["Depuración con eBPF"]
---

Bienvenido a la última publicación de nuestra serie sobre eBPF para la depuración. Hemos cubierto los conceptos básicos de `bpftrace` y las herramientas de línea de comandos de la BPF Compiler Collection (BCC). Ahora, daremos un paso adelante y aprenderemos a escribir nuestras propias herramientas eBPF personalizadas usando Python y el framework de BCC.

### Escribiendo Herramientas Personalizadas con BCC y Python

Si bien las herramientas preconstruidas de BCC son potentes, el verdadero potencial de eBPF se desbloquea cuando comienzas a escribir tus propias herramientas para resolver problemas específicos. El framework de BCC proporciona enlaces de Python que te permiten escribir programas eBPF en C e interactuar con ellos desde un script de Python.

Escribamos una herramienta simple que rastree las llamadas al sistema `open()`, similar a `opensnoop`, pero con nuestra propia salida personalizada.

Crea un archivo llamado `myopensnoop.py` con el siguiente contenido:
```python
#!/usr/bin/python3
from bcc import BPF

# Programa eBPF escrito en C
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

# Carga el programa eBPF
b = BPF(text=bpf_program)

# Adjunta el programa eBPF a la llamada al sistema openat()
b.attach_kprobe(event="do_sys_openat2", fn_name="trace_open_entry")

# Imprime el encabezado
print("%-18s %-16s %-6s %s" % ("TIME(s)", "COMM", "PID", "FILE"))

# Procesa los eventos
while True:
    try:
        (task, pid, cpu, flags, ts, msg) = b.trace_fields()
        print("%-18.9f %-16s %-6d %s" % (ts, task.decode('utf-8', 'replace'), pid, msg.decode('utf-8', 'replace')))
    except KeyboardInterrupt:
        exit()
```

Para ejecutar este script, hazlo ejecutable y ejecútalo con `sudo`:
```bash
chmod +x myopensnoop.py
sudo ./myopensnoop.py
```

Este es un ejemplo simplificado, pero demuestra la estructura básica de una herramienta de Python de BCC. Escribes tu código C de eBPF dentro de una cadena de Python, lo cargas con `BPF(text=...)`, lo adjuntas a una sonda del kernel y luego procesas los eventos en un bucle.

### Casos de Uso Avanzados: Seguridad y Más Allá

eBPF no es solo para la depuración y el análisis de rendimiento. Su capacidad para inspeccionar y controlar el comportamiento del sistema a nivel del kernel lo convierte en una herramienta poderosa para la seguridad.

Por ejemplo, eBPF se puede utilizar para:
- **Detectar y bloquear actividad maliciosa:** Al monitorear las llamadas al sistema, puedes detectar comportamientos sospechosos como acceso a archivos inesperado, conexiones de red o ejecuciones de procesos. El artículo de `windshock.github.io` muestra cómo detectar puertas traseras basadas в eBPF.
- **Hacer cumplir las políticas de seguridad:** Herramientas como Cilium usan eBPF para hacer cumplir las políticas de seguridad de la red a nivel del kernel, proporcionando un control detallado sobre el tráfico de red entre contenedores y microservicios.
- **Obtener una visibilidad profunda para la forense:** eBPF puede proporcionar una gran cantidad de información para la forense digital y la respuesta a incidentes, permitiéndote reconstruir la línea de tiempo de un ataque con un alto nivel de detalle.

### Conclusión

Esta serie ha sido un viaje desde los conceptos básicos de `bpftrace` hasta la escritura de herramientas eBPF personalizadas con Python. Esperamos que hayas visto cuán potente y versátil puede ser eBPF para la depuración, el análisis de rendimiento y la seguridad en Linux.

El ecosistema de eBPF está en constante crecimiento y evolución, por lo que te animamos a seguir explorando y aprendiendo. El sitio web oficial de eBPF en [ebpf.io](https://ebpf.io/) es un gran recurso para mantenerte actualizado con los últimos desarrollos.
