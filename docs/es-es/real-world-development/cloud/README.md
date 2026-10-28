---
slug: es-es/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que GitHub Copilot trabaje de forma asíncrona en la nube. Asignas trabajo en GitHub y el agente en la nube lo aborda en segundo plano: explora el repositorio, realiza cambios y abre una solicitud de incorporación de cambios (PR) mientras tú te ocupas de otras tareas.

En estos ejercicios añadirás instrucciones personalizadas para el agente y después le asignarás una incidencia de GitHub para que la implemente. Revisarás y usarás agentes personalizados, supervisarás y orientarás las sesiones desde el panel de agentes y, por último, revisarás las solicitudes de incorporación de cambios e iterarás sobre los resultados.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea el repositorio y el codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade instrucciones personalizadas que seguirá el agente |
| [2. Cloud agent][ex2] | Agente asíncrono | Asigna incidencias a GitHub Copilot cloud agent |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revisa y usa agentes personalizados |
| [4. Supervisar agentes][ex4] | Supervisión | Supervisa y orienta las sesiones del agente |
| [5. Iterar][ex5] | Revisión | Revisa las PR, itera sobre el trabajo de Copilot y elige los siguientes pasos |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] GitHub Copilot cloud agent habilitado en tu cuenta u organización

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis mediante [GitHub Education][student-plan]. El plan **Copilot Student** incluye las funciones de agente, Model Context Protocol (MCP), revisión de código y la interfaz de línea de comandos (CLI) de Copilot que se usan en este taller, por lo que permite completar todos los entornos.

> [!NOTE]
> Cloud agent está disponible en **Copilot Student, Pro, Pro+, Business y Enterprise**, es decir, en todos los planes excepto Copilot Free. En Copilot Business o Enterprise, un administrador debe habilitarlo para la organización.

> [!NOTE]
> Este entorno no trata MCP. Para aprender a usar servidores MCP con Copilot, consulta el [entorno CLI][cli] o el [entorno VS Code][vscode].

## Primeros pasos

**[Empieza por el ejercicio 0: requisitos previos →][ex0]**

[student-plan]: https://github.com/education/students
[cli]: ../cli/
[vscode]: ../vscode/
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
