---
title: "Depuración con eBPF 101: Una Guía para Principiantes"
authors:
  - oss.lat
date: 2026-02-02
description: "Una introducción amigable para principiantes a eBPF para la depuración y el rastreo en Linux."
images:
    - images/blog/ebpf-debugging-basics.png
tags:
    - eBPF
    - Depuración
    - Linux
    - Rastreo
    - Principiante
series: ["Depuración con eBPF"]
---

¡Bienvenido a la primera publicación de nuestra serie sobre eBPF para la depuración! En esta publicación, te presentaremos los conceptos básicos de eBPF y te mostraremos cómo comenzar con herramientas simples pero potentes para rastrear y depurar tus sistemas Linux.

### ¿Qué es eBPF?

eBPF (extended Berkeley Packet Filter) es una tecnología revolucionaria que te permite ejecutar programas en un sandbox dentro del kernel de Linux sin cambiar el código fuente del kernel ni cargar módulos del kernel. Piénsalo como una máquina virtual ligera dentro del kernel que proporciona una forma segura y eficiente de agregar nuevas capacidades al kernel en tiempo de ejecución.

Originalmente enfocado en el filtrado de paquetes de red, eBPF ha evolucionado para convertirse en una herramienta de propósito general para una amplia gama de casos de uso, que incluyen:
- **Redes:** Procesamiento y filtrado de paquetes de alto rendimiento.
- **Observabilidad:** Recopilación de métricas y eventos de rendimiento detallados.
- **Seguridad:** Monitoreo de llamadas al sistema y actividad de red para hacer cumplir las políticas de seguridad.
- **Rastreo y Depuración:** Obtención de información profunda sobre el comportamiento del kernel y las aplicaciones.

### Configurando Tu Entorno

Para comenzar con eBPF, necesitarás una distribución de Linux moderna. Para esta guía, nos centraremos en las herramientas disponibles en Ubuntu 24.04 y versiones posteriores, donde muchas herramientas de eBPF se incluyen de forma predeterminada.

Los dos conjuntos de herramientas principales que usaremos son:
- **BCC (BPF Compiler Collection):** Una colección de potentes herramientas de línea de comandos y un framework de Python para crear programas eBPF personalizados.
- **`bpftrace`:** Un lenguaje de rastreo de alto nivel que facilita la escritura de scripts de rastreo eBPF personalizados con una sintaxis simple.

Puedes instalar estas herramientas en Ubuntu con:
```bash
sudo apt-get update
sudo apt-get install -y bcc bpftrace
```

### Tus Primeros Rastros con `bpftrace`

`bpftrace` es la herramienta perfecta para que los principiantes comiencen a explorar eBPF. Proporciona una sintaxis simple, similar a `awk`, para escribir one-liners y pequeños scripts para rastrear eventos del kernel y del espacio de usuario.

Probemos algunos ejemplos básicos:

**1. Rastreando nuevos procesos:**
Este one-liner imprimirá el nombre de cada nuevo proceso que se ejecute en el sistema.
```bash
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_execve { printf("%s\n", comm); }'
```

**2. Contando llamadas al sistema:**
Este comando contará todas las llamadas al sistema e imprimirá un resumen cada segundo.
```bash
sudo bpftrace -e 'tracepoint:raw_syscalls:sys_enter { @[probe] = count(); } interval:s:1 { print(@); }'
```

**3. Rastreando llamadas al sistema `open()`:**
Esto rastreará todas las llamadas al sistema `open()`, mostrando el nombre del proceso y el archivo que se está abriendo.
```bash
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_openat { printf("%s %s\n", comm, str(args->filename)); }'
```

Estos son solo algunos ejemplos simples, pero demuestran el poder y la simplicidad de `bpftrace`. En la próxima publicación de esta serie, exploraremos técnicas más avanzadas y te presentaremos las herramientas de la colección BCC. ¡Mantente al tanto!
