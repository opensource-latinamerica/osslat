---
title: "Migraciones de Esquema y Datos de Base de Datos sin Interrupción con gh-ost: Un Imprescindible en Producción"
date: 2025-11-14T15:30:00-06:00
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

En el mundo actual de disponibilidad constante, dejar tu base de datos de producción fuera de línea para un cambio de esquema es a menudo inaceptable. Incluso unos pocos segundos de interrupción pueden significar pérdidas de ingresos, usuarios frustrados o servicios caídos. Esto es especialmente cierto para bases de datos MySQL grandes y con mucho tráfico.

Las operaciones `ALTER TABLE` tradicionales en MySQL pueden ser bloqueantes, lo que significa que bloquean la tabla e impiden escrituras (y a veces lecturas) durante la duración de la operación. Para tablas grandes, esto puede tomar minutos o incluso horas.

Aquí es donde entra **`gh-ost`** (GitHub Online Schema Transit). Desarrollado por GitHub, `gh-ost` es una herramienta de migración de esquema en línea para MySQL, basada en triggers y probada en batalla. Te permite realizar cambios de esquema complejos en tablas grandes con **cero interrupciones** para tu aplicación.

### ¿Cómo Logra gh-ost Cero Interrupciones?

`gh-ost` funciona creando una copia de tu tabla, aplicando los cambios de esquema a la copia y luego sincronizando sutilmente los datos antes de intercambiar atómicamente las tablas. Aquí tienes un desglose simplificado:

1.  **Crear una Tabla "Fantasma":** `gh-ost` crea una *tabla fantasma* vacía con el nuevo esquema, idéntica a tu tabla original pero con las modificaciones deseadas.
2.  **Aplicar Triggers:** Luego crea **triggers** en tu *tabla original* para las operaciones `INSERT`, `UPDATE` y `DELETE`. Estos triggers capturan los cambios realizados en la tabla original y los aplican a la tabla fantasma.
3.  **Copiar Datos Existentes:** En segundo plano, `gh-ost` copia todos los datos existentes de tu tabla original a la tabla fantasma en pequeños fragmentos manejables. Este proceso se acelera para minimizar el impacto en tu base de datos.
4.  **Sincronización Continua:** Mientras se copian los datos, los triggers continúan reenviando cualquier nuevo cambio de la tabla original a la tabla fantasma, asegurando que la tabla fantasma se mantenga actualizada.
5.  **Corte (Intercambio Atómico):** Una vez que la tabla fantasma está completamente sincronizada, `gh-ost` realiza un **intercambio atómico** utilizando una operación `RENAME TABLE`. Esta es una operación extremadamente rápida y no bloqueante que esencialmente renombra tu tabla original a un nombre descartado y renombra la tabla fantasma al nombre de tu tabla original. Tu aplicación comienza a usar la nueva tabla inmediatamente.
6.  **Limpieza:** La tabla antigua y original (ahora renombrada) puede eliminarse posteriormente.

### Beneficios Clave de gh-ost

* **Cero Interrupciones:** Este es su beneficio principal y más significativo. Tu aplicación permanece completamente disponible durante toda la migración.
* **Seguridad y Control:**
    * **Mecanismos de Aceleración (Throttle):** `gh-ost` monitorea activamente la carga de tu base de datos (retraso de replicación, uso de CPU, etc.) y ralentiza sus operaciones si la base de datos se vuelve demasiado ocupada.
    * **Hooks:** Puedes definir "hooks" personalizados (scripts) para que se ejecuten en varias etapas de la migración, lo que permite una automatización y validación avanzadas.
    * **Cancelación Grácil:** Puedes cancelar una migración de forma segura en casi cualquier punto, volviendo a la tabla original sin pérdida de datos.
    * **Checksums y Verificación:** Puede realizar comprobaciones de integridad de datos.
* **No Bloqueante:** Evita bloqueos de tabla de larga duración.
* **Auditable:** Proporciona registros detallados de sus operaciones.
* **Probado en Batalla:** Utilizado por GitHub durante años en algunas de las implementaciones de MySQL más grandes del mundo.

### Cuándo Usar gh-ost

`gh-ost` es ideal para:
* Grandes tablas de producción de MySQL donde un `ALTER TABLE` causaría una interrupción inaceptable.
* Cambios de esquema complejos (agregar columnas, cambiar tipos de columnas, agregar/eliminar índices, etc.).
* Entornos donde el despliegue continuo y la alta disponibilidad son críticos.

### Primeros Pasos

`gh-ost` es una herramienta de código abierto, típicamente instalada como un binario de Go. Se ejecuta desde una máquina cliente (o un servidor de migración dedicado) que tiene acceso de red a tu base de datos MySQL.

```bash
# Ejemplo de comando gh-ost para añadir una columna
gh-ost \
  --host=tu_host_bd \
  --port=3306 \
  --user=tu_usuario \
  --password=tu_contraseña \
  --database=tu_base_de_datos \
  --table=tu_tabla \
  --alter="ADD COLUMN nueva_columna VARCHAR(255) NOT NULL DEFAULT ''" \
  --execute
```