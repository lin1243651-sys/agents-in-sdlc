---
slug: es-es/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** en VS Code incorpora GitHub Copilot al editor que ya usas. En Visual Studio Code y GitHub Codespaces, trabajarás con Copilot Chat en modo agente, conectarás herramientas externas mediante MCP y usarás agentes personalizados sin salir del IDE, donde Copilot puede acceder a tus archivos, al terminal y a los problemas.

Empezarás añadiendo instrucciones personalizadas y observando cómo Copilot las sigue. Después, usarás el modo agente para crear una función de filtrado que afecta a la interfaz, la capa de datos y las pruebas. Conectarás Playwright MCP para probar la función en un navegador antes de abrir una solicitud de incorporación de cambios. Por último, revisarás y usarás un agente personalizado de accesibilidad y supervisarás, orientarás e iterarás sobre los cambios de Copilot desde el editor.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea el repositorio y el codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade y verifica instrucciones personalizadas en VS Code |
| [2. Modo agente][ex2] | Generación de código | Crea una función de filtrado con el modo agente |
| [3. MCP con Playwright][ex3] | Herramientas externas | Prueba la función en un navegador con el servidor MCP de Playwright |
| [4. Agentes personalizados][ex4] | Agentes especializados | Revisa y usa agentes personalizados |
| [5. Supervisar agentes][ex5] | Supervisión | Supervisa y orienta sesiones de agente |
| [6. Iterar][ex6] | Revisión | Revisa localmente el trabajo de Copilot y elige los siguientes pasos |
| [Opcional: integra Foundry][foundry-toolkit] | Agentes de IA | Prepara un modelo, publica un agente y conéctalo al sitio con VS Code y Foundry Toolkit |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Acceso a GitHub Codespaces

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis mediante [GitHub Education][student-plan]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que se usan en este taller.

## Primeros pasos

**[Empieza por el ejercicio 0: requisitos previos →][ex0]**

[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
