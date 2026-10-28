---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has asignado a GitHub Copilot cloud agent tres tareas para mejorar la experiencia y añadir funciones. Aunque cloud agent trabaja de forma autónoma y asíncrona, sigue siendo importante supervisar las tareas.

La [página de agentes][agents-page] de GitHub.com funciona como centro de control: permite ver todas las tareas del agente que tienen solicitudes de incorporación de cambios (PR) abiertas, consultar las operaciones realizadas y orientar las sesiones que siguen en curso.

En esta lección, vas a:

- explorar la página de agentes para supervisar las tareas de cloud agent.
- orientar una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar el modo de alto contraste, el equipo decidió que también sería útil añadir un modo claro. Como el agente ya está trabajando en los estilos y los controles, le pedirás que incluya el modo claro.

## Revisa las tareas de cloud agent

Comprueba el estado de las tareas que has asignado.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas que aparecen en los paneles principal y lateral. Deberías encontrar las tareas que asignaste a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar interfaces de programación de aplicaciones (API) para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones de Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores Model Context Protocol (MCP).
   - realizar los distintos pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo y el enfoque del agente.

4. Revisa también el panel de PR de la derecha, donde puedes consultar la solicitud y los archivos modificados para seguir el trabajo.

## Orienta cloud agent

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión correspondiente al modo de alto contraste. El título exacto depende del nombre que Copilot haya elegido y del estado de la tarea.

   ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Verás mensajes que muestran el inicio de la tarea.
3. En el cuadro **Steer active session while Copilot is working**, escribe:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarea de cloud agent con el cuadro para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Confirma que Copilot lo reconoce y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo cuando termine el paso que estaba realizando en el momento de enviar el mensaje. El proceso puede tardar varios minutos; puedes aprovechar ese tiempo para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas de cloud agent. Permite seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar el alcance o aportar indicaciones. En esta lección, has:

- usado el centro de control y la página de agentes para supervisar tareas.
- orientado una sesión en curso para solicitar otra funcionalidad.

Cuando Copilot termine las funciones de accesibilidad, [iterarás sobre las solicitudes de incorporación de cambios][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
