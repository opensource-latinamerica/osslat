---
title: "Out of the Freezer: How Distributed Leadership is Reviving an OpenStack Project"
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
  - governance
  - dpl
authors:
  - oss.lat
images:
  - /images/blog/openinfra/freezer-dpl.webp
---

OpenStack is a vast ecosystem of projects, each with its own lifecycle. Some projects mature and become cornerstones of the platform, while others, over time, can see a decline in activity. One such project, **Freezer**, the OpenStack service for backup, restore, and disaster recovery, was on such a path, nearing a state of deprecation.

However, a flexible governance model within OpenStack has provided a new lease on life for Freezer, demonstrating how the community adapts to keep valuable projects alive.

### The Challenge: The Weight of the PTL Crown

Traditionally, OpenStack projects are led by a **Project Team Lead (PTL)**. The PTL is a single individual responsible for a wide range of tasks: managing development, overseeing releases, representing the project, and much more. This is a demanding role, and for projects with a smaller pool of active contributors, finding someone willing and able to take on the PTL role can be a significant challenge. The burden of the PTL role can be a bottleneck, slowing down or even halting a project's progress.

### A New Model: Distributed Project Leadership (DPL)

To address this, the OpenStack Technical Committee (TC) established the **Distributed Project Leadership (DPL)** model. DPL is an alternative to the single PTL, allowing the responsibilities of leadership to be distributed among several contributors, known as **liaisons**.

These liaisons cover specific areas, such as:
*   **Release liaison:** Manages the release process.
*   **TaCT-SIG liaison:** (formerly Infra liaison) - a point of contact for CI/CD and infrastructure issues.
*   **Security liaison:** Handles security-related matters.
*   **TC liaison:** A member of the Technical Committee who acts as a mentor and point of contact.

This model is less burdensome for any single individual and empowers a wider group of contributors to take ownership of the project's success.

### Freezer's Thaw: Saved by DPL

Freezer had been struggling to maintain momentum, with its last major release dating back several years. The project was at risk of being "frozen" indefinitely. By adopting the DPL model, the Freezer team was able to distribute the leadership responsibilities, making it easier to manage the project with the existing community. This move has been instrumental in "thawing" the project, allowing development and maintenance to continue.

### A Conscious Choice for Collaboration

Recently, the OpenStack TC has implemented a new policy: the DPL status for all projects is reset at the beginning of each development cycle. Projects wishing to continue with the DPL model must actively opt-in again.

This policy, while seeming like a small bureaucratic step, is a significant move. It ensures that projects consciously and deliberately choose their leadership structure each cycle. It encourages active participation from the liaisons and the project team, preventing the DPL model from becoming a passive state.

For projects like Freezer, this means the community will regularly reaffirm its commitment to the project and its chosen leadership model. It’s a testament to the flexibility of OpenStack’s governance and the community's dedication to preserving valuable projects. The DPL model has not just saved Freezer from the brink; it has provided a sustainable path forward, built on collaboration and shared responsibility.
