---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o dominios concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en `.github/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimiento del dominio. Un equipo puede compartir su experiencia en agentes reutilizables, como un agente de accesibilidad que aplique las directrices de accesibilidad para el contenido web (WCAG), uno de seguridad o uno de pruebas.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter del formato YAML (YAML Ain't Markup Language) con, al menos, `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] comparten algunas características: ambos se describen principalmente en Markdown e indican a una IA cómo realizar tareas. La diferencia más sencilla es que el **agente personalizado** es quien trabaja y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes como parte de su trabajo. En este laboratorio, el agente de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad; para ello, podría usar, por ejemplo, una habilidad para gestionar solicitudes de incorporación de cambios o ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear agentes personalizados. Prueba e itera para encontrar lo que funciona en cada entorno y escenario.

En este ejercicio, vas a:

- revisar un agente personalizado de accesibilidad existente.
- asignarle una tarea a Copilot cloud agent mediante el agente de accesibilidad.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Algunos usuarios han indicado que el tema oscuro actual es difícil de leer porque el contraste entre el texto y el fondo es insuficiente. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar. Usarás un agente personalizado para implementar esta función con rapidez.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

1. Vuelve al codespace y abre `.github/agents/accessibility.md`.
2. Revisa el frontmatter YAML y los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Recorre las secciones siguientes: responsabilidades principales al generar código para un sitio accesible, prácticas recomendadas de accesibilidad, ejemplos del lenguaje de marcado de hipertexto (HTML), las hojas de estilo en cascada (CSS) y JavaScript, y una lista de errores habituales.

## Crea y asigna una incidencia

El centro de control permite asignar tareas a cloud agent, supervisarlas y proporcionar indicaciones adicionales. Crea una incidencia para el modo de alto contraste y asígnala al agente personalizado.

1. Abre el repositorio y selecciona la pestaña **Issues**.
2. Selecciona **New issue** y, después, **Blank issue**.
3. Establece **Title** en `Add high contrast mode to website`.
4. Establece **Description** en:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

5. Selecciona **Create**.
6. En el lado derecho, selecciona **Assign to Copilot**.
7. Selecciona **Accessibility agent** en la lista de agentes personalizados y, después, selecciona **Assign**.

   ![Asignación de cloud agent con el agente personalizado Accessibility seleccionado](../../../_images/ex5-select-custom-agent.png)

Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

Has:

- revisado cómo se definen los agentes personalizados.
- asignado una tarea al agente de accesibilidad.

Los [agentes personalizados][custom-agents] permiten compartir la experiencia y los estándares del equipo para orientar el trabajo de Copilot.

Mientras Copilot implementa el modo de alto contraste, [supervisarás y orientarás la sesión del agente][next-lesson] desde el centro de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Prepárate para usar agentes personalizados en tu organización][org-custom-agents]
- [Prepárate para usar agentes personalizados en tu empresa][enterprise-custom-agents]

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
