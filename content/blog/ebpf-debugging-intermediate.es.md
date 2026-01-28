---
title: "Depuración con eBPF en la Práctica: Técnicas Intermedias"
authors:
  - oss.lat
date: 2026-02-09
description: "Lleva tus habilidades de depuración con eBPF al siguiente nivel con las herramientas de BCC y scripts de bpftrace más avanzados."
images:
    - images/blog/ebpf-debugging-intermediate.jpeg
tags:
    - eBPF
    - Depuración
    - Linux
    - Rastreo
    - BCC
    - Intermedio
series: ["Depuración con eBPF"]
---

¡Bienvenido de nuevo a nuestra serie de depuración con eBPF! En la primera publicación, presentamos los conceptos básicos de eBPF y `bpftrace`. Ahora, es el momento de profundizar y explorar las potentes herramientas que ofrece la BPF Compiler Collection (BCC), así como técnicas más avanzadas de `bpftrace`.

### Introducción a la BPF Compiler Collection (BCC)

BCC es un rico conjunto de herramientas que proporciona una serie de utilidades de línea de comandos listas para usar para el análisis de rendimiento y la depuración. Estas herramientas están construidas sobre eBPF y proporcionan una interfaz fácil de usar para escenarios de rastreo complejos.

La mayoría de las herramientas de BCC siguen una convención de nomenclatura similar, terminando en `-bpfcc`. Exploremos algunas de las más útiles.

**1. `execsnoop-bpfcc`:**
Esta herramienta es perfecta para entender qué procesos se están ejecutando en tu sistema. Es similar al one-liner de `bpftrace` que vimos en la publicación anterior, pero proporciona una salida más detallada.

Para rastrear todos los nuevos procesos ejecutados por el usuario `root`, puedes ejecutar:
```bash
sudo execsnoop-bpfcc -Uu root -T
```
El indicador `-T` agrega una marca de tiempo, que es muy útil para correlacionar eventos.

**2. `opensnoop-bpfcc`:**
Si necesitas saber qué archivos está abriendo un proceso, `opensnoop-bpfcc` es la herramienta para el trabajo. Esto es increíblemente útil para depurar problemas de permisos o para entender los patrones de acceso a archivos de una aplicación.

Para rastrear todos los archivos abiertos por un proceso llamado `qemu-system-x86`, puedes usar:
```bash
sudo /usr/sbin/opensnoop-bpfcc --full-path --name qemu-system-x86
```

**3. `tcplife-bpfcc`:**
Para la depuración relacionada con la red, `tcplife-bpfcc` proporciona un resumen de las sesiones TCP, incluido el ID del proceso, las direcciones de origen y destino, y la duración de la sesión.

Para rastrear todas las sesiones TCP, simplemente ejecuta:
```bash
sudo tcplife-bpfcc
```

### Scripts Avanzados de `bpftrace`

Aunque las herramientas de BCC son excelentes para escenarios comunes, a veces necesitas escribir tu propia lógica personalizada. `bpftrace` te permite escribir scripts más complejos para hacer precisamente eso.

**Ejemplo: Rastreando operaciones lentas del sistema de archivos**

Digamos que quieres investigar operaciones lentas del sistema de archivos. Puedes escribir un script de `bpftrace` para medir la latencia de las llamadas al sistema `write()` e imprimir solo las que tarden más de un cierto umbral.

Crea un archivo llamado `slow_writes.bt` con el siguiente contenido:
```
kprobe:vfs_write
{
    @start[tid] = nsecs;
}

kretprobe:vfs_write /@start[tid]/
{
    $duration = (nsecs - @start[tid]) / 1000; // microsegundos
    if ($duration > 100) {
        printf("tid %d: escritura lenta (%d us)\n", tid, $duration);
    }
    delete(@start[tid]);
}
```

Luego puedes ejecutar este script con:
```bash
sudo bpftrace slow_writes.bt
```
Este script imprimirá un mensaje cada vez que una operación `write()` tarde más de 100 microsegundos.

### ¿Qué Sigue?

En esta publicación, solo hemos arañado la superficie de lo que es posible con BCC y `bpftrace`. Te animamos a explorar las otras herramientas disponibles en el paquete `bcc-tools` y a experimentar escribiendo tus propios scripts de `bpftrace`.

En la última publicación de esta serie, daremos un paso más y te mostraremos cómo escribir herramientas eBPF personalizadas en Python utilizando el framework de BCC. ¡Mantente al tanto!
