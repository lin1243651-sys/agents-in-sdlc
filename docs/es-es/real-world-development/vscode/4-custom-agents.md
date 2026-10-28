---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del dominio que le ayuden a realizar ciertos tipos de trabajo de forma más eficaz. Los equipos pueden convertir su experiencia en agentes reutilizables: un agente de accesibilidad que aplique las [directrices de accesibilidad para el contenido web (WCAG)][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter del formato YAML (YAML Ain't Markup Language) con, al menos, los campos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se solapan en algunos aspectos: ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar tareas. La distinción más clara es que un **agente personalizado** es quien realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes. En este laboratorio, el agente personalizado de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad. Como parte de ese trabajo, podría utilizar habilidades, por ejemplo, para gestionar el flujo de una solicitud de incorporación de cambios o ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier uso de IA, prueba e itera para encontrar lo que funciona en cada entorno y escenario.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Algunos usuarios han indicado que el tema oscuro actual les resulta difícil de leer porque el contraste entre el texto y el fondo es insuficiente. Para responder a esta necesidad, el equipo de diseño ha solicitado un modo de alto contraste que los usuarios puedan activar y desactivar.

La accesibilidad es importante, así que usarás un agente personalizado para implementar esta función con rapidez.

En este ejercicio, vas a revisar un agente personalizado de accesibilidad existente y usarlo en Copilot Chat para implementar un modo de alto contraste.

En concreto, vas a:

- revisar un agente personalizado de accesibilidad existente.
- usar el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Revisa el agente personalizado de accesibilidad

Un agente personalizado de accesibilidad ya está disponible en el proyecto. Revisa su contenido para entender cómo orientará a Copilot. Vuelve al codespace y crea una rama nueva desde `main` para mantener este trabajo separado de la solicitud de incorporación de cambios (PR) de filtrado del ejercicio 3.

1. En un terminal, crea una rama nueva desde `main`:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abre `.github/agents/accessibility.md`.
3. Revisa el frontmatter YAML y los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

4. Recorre las secciones siguientes y revisa:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas para la accesibilidad.
   - los ejemplos del lenguaje de marcado de hipertexto (HTML), las hojas de estilo en cascada (CSS) y JavaScript.
   - los errores y problemas habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra en el selector de agentes, situado en la parte inferior de la vista Copilot Chat, todos los agentes personalizados definidos en `.github/agents`. Puedes seleccionar uno para limitar una sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve al codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.

1. Antes de empezar, vuelve al codespace y abre el panel Copilot Chat.
2. Selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; las instrucciones de cada paso indican qué seleccionar cuando corresponda.
3. En el menú de agentes de la vista Chat, selecciona **Agent** si aún no está seleccionado.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Abre el selector de agentes situado en la parte inferior de la vista Chat; de forma predeterminada, muestra el agente activo, que suele ser **default**.
5. Selecciona **Accessibility agent** en la lista de agentes.
6. Envía este prompt al agente de accesibilidad:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

7. Observa cómo Copilot explora el código, propone cambios y los aplica a los archivos del proyecto. Cada edición aparece en el chat con la ruta del archivo y una comparación de los cambios.
8. A medida que se aplican las ediciones, observa cómo se actualiza el indicador **Files changed**, que muestra el conjunto de archivos modificados por el agente.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot está realizando cambios reales en el repositorio y puede editar componentes Astro, archivos CSS y pruebas relacionadas.

En el ejercicio siguiente revisarás y orientarás este trabajo en curso.

## Resumen y siguientes pasos

Esta lección ha explorado los [agentes personalizados][custom-agents], asistentes de IA especializados en tareas y ámbitos concretos. Los agentes permiten convertir la experiencia y los estándares del equipo en recursos reutilizables que orientan a Copilot para que realice determinados tipos de trabajo con mayor eficacia. Has:

- revisado cómo se definen los agentes personalizados.
- usado un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios y añadirás un modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crea agentes personalizados en tu IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
