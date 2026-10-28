---
title: "Lección 7 - Crea y usa un agente de QA"
description: "Crea un agente de QA personalizado que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar el filtrado en el navegador. Ahora reunirás estas capacidades en un agente personalizado con un proceso de control de calidad (QA) bien definido.

En esta lección, vas a:

- explorar cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crear e inspeccionar un perfil de control de calidad reutilizable.
- seleccionar el agente de QA y revisar sus conclusiones respecto a la incidencia.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso y crear un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe su propósito, instrucciones y herramientas disponibles. En este taller, definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Cada personalización cumple una función distinta: las instrucciones del repositorio describen los estándares del equipo; la habilidad `quality-checks` agrupa comprobaciones repetibles; Playwright MCP proporciona herramientas de navegador; y el perfil de QA indica a Copilot cómo usar esas capacidades para evaluar los requisitos y comunicar los resultados.

## Crea el perfil de QA

Pide a Copilot que cree un perfil reutilizable antes de abrir la PR de la funcionalidad.

1. Vuelve al codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Revisa el perfil para confirmar que incluye el proceso y los límites previstos antes de usarlo.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero proporciona un nombre visible al agente.
3. Confirma que las instrucciones de QA empiezan por los requisitos, siguen las instrucciones del repositorio, ejecutan `quality-checks` y usan Playwright MCP.
4. Confirma que el agente presenta pruebas que respaldan el informe, pide permiso antes de cambiar código de implementación y no confirma cambios ni abre PR.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecuta QA en la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Reanuda la conversación de filtrado para conservar el contexto de la incidencia y el plan.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, puede que no aparezca en la lista de agentes disponibles. El comando anterior lo activará.

2. Pide al agente que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Espera a que termine el análisis.
4. Lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable y revisado su trabajo. En esta lección, has:

- explorado cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- creado e inspeccionado un perfil reutilizable de QA.
- seleccionado el agente de QA y revisado sus conclusiones respecto a la incidencia.

Ya tienes la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación listos para revisar. A continuación, los reunirás en una [PR de la funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crea agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
