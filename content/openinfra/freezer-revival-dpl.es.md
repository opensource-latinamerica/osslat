---
title: "Saliendo del Congelador: Cómo el Liderazgo Distribuido está Reviviendo un Proyecto de OpenStack"
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
  - gobernanza
  - dpl
authors:
  - oss.lat
images:
  - /images/blog/openinfra/freezer-dpl.webp
---

OpenStack es un vasto ecosistema de proyectos, cada uno con su propio ciclo de vida. Algunos proyectos maduran y se convierten en pilares de la plataforma, mientras que otros, con el tiempo, pueden ver una disminución en la actividad. Uno de esos proyectos, **Freezer**, el servicio de OpenStack para respaldo, restauración y recuperación ante desastres, se encontraba en ese camino, acercándose a un estado de obsolescencia.

Sin embargo, un modelo de gobernanza flexible dentro de OpenStack ha proporcionado una nueva vida a Freezer, demostrando cómo la comunidad se adapta para mantener vivos proyectos valiosos.

### El Desafío: El Peso de la Corona del PTL

Tradicionalmente, los proyectos de OpenStack son liderados por un **Líder de Equipo de Proyecto (PTL)**. El PTL es una sola persona responsable de una amplia gama de tareas: gestionar el desarrollo, supervisar los lanzamientos, representar al proyecto y mucho más. Este es un rol exigente, y para los proyectos con un grupo más reducido de colaboradores activos, encontrar a alguien dispuesto y capaz de asumir el rol de PTL puede ser un desafío importante. La carga del rol de PTL puede ser un cuello de botella, ralentizando o incluso deteniendo el progreso de un proyecto.

### Un Nuevo Modelo: Liderazgo de Proyecto Distribuido (DPL)

Para abordar esto, el Comité Técnico de OpenStack (TC) estableció el modelo de **Liderazgo de Proyecto Distribuido (DPL)**. DPL es una alternativa al único PTL, que permite que las responsabilidades de liderazgo se distribuyan entre varios colaboradores, conocidos como **enlaces (liaisons)**.

Estos enlaces cubren áreas específicas, tales como:
*   **Enlace de lanzamientos (Release liaison):** Gestiona el proceso de lanzamiento.
*   **Enlace de TaCT-SIG (TaCT-SIG liaison):** (anteriormente Enlace de Infra) - un punto de contacto para problemas de CI/CD e infraestructura.
*   **Enlace de seguridad (Security liaison):** Se ocupa de los asuntos relacionados con la seguridad.
*   **Enlace del TC (TC liaison):** Un miembro del Comité Técnico que actúa como mentor y punto de contacto.

Este modelo es menos oneroso para una sola persona y empodera a un grupo más amplio de colaboradores para que se apropien del éxito del proyecto.

### El Deshielo de Freezer: Salvado por DPL

Freezer había estado luchando por mantener el impulso, con su último gran lanzamiento datando de hace varios años. El proyecto corría el riesgo de quedar "congelado" indefinidamente. Al adoptar el modelo DPL, el equipo de Freezer pudo distribuir las responsabilidades de liderazgo, lo que facilitó la gestión del proyecto con la comunidad existente. Este movimiento ha sido fundamental para "descongelar" el proyecto, permitiendo que el desarrollo y el mantenimiento continúen.

### Una Elección Consciente por la Colaboración

Recientemente, el TC de OpenStack ha implementado una nueva política: el estado de DPL para todos los proyectos se restablece al comienzo de cada ciclo de desarrollo. Los proyectos que deseen continuar con el modelo DPL deben optar activamente por él de nuevo.

Esta política, aunque parezca un pequeño paso burocrático, es un movimiento significativo. Asegura que los proyectos elijan consciente y deliberadamente su estructura de liderazgo en cada ciclo. Fomenta la participación activa de los enlaces y del equipo del proyecto, evitando que el modelo DPL se convierta en un estado pasivo.

Para proyectos como Freezer, esto significa que la comunidad reafirmará regularmente su compromiso con el proyecto y su modelo de liderazgo elegido. Es un testimonio de la flexibilidad de la gobernanza de OpenStack y de la dedicación de la comunidad para preservar proyectos valiosos. El modelo DPL no solo ha salvado a Freezer del abismo; ha proporcionado un camino sostenible hacia adelante, construido sobre la colaboración y la responsabilidad compartida.
