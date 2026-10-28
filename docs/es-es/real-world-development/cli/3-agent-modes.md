---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado desde una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Los cambios mayores requieren un proceso más sólido que una funcionalidad pequeña. GitHub Copilot CLI puede adaptarse al flujo de trabajo de tu organización para ayudarte a crear lo adecuado de la forma correcta. En esta lección partirás de una incidencia para planificar e implementar una funcionalidad, comprobar el código, validar el comportamiento y prepararlo para combinarlo.

> [!NOTE]
> A medida que avances en este flujo, usarás la misma conversación y rama. Normalmente se usarían ramas o solicitudes de incorporación de cambios diferentes para distintos tipos de archivos, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección, vas a:

- iniciar una conversación de Copilot desde una incidencia de GitHub.
- definir los requisitos en modo Plan.
- implementar la funcionalidad en modo Autopilot.
- revisar el código.
- validar manualmente la funcionalidad en un navegador reenviado.

Durante el trabajo, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la solicitud de incorporación de cambios de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero es necesario acordar algunos detalles, como la combinación de categorías, antes de escribir código. Usarás el modo Plan para resolver esas decisiones y después autorizarás una implementación acotada con Autopilot.

## Contexto

La introducción de agentes de programación con IA no cambia los fundamentos del desarrollo. La mayoría de los equipos sigue un flujo parecido a este:

1. Partir de una incidencia que describe el trabajo.
2. Planificar lo que hay que crear.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código después de que se revise y supere la integración continua.

> [!NOTE]
> El flujo concreto varía según el equipo y la organización, pero la mayoría sigue una variante de estos pasos.

Este proceso ayuda a garantizar que el código generado por IA cumple los requisitos y pasa por las mismas revisiones que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente; este propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma, escribe código, ejecuta pruebas y repite el proceso hasta completar la tarea.

Empieza en Plan, revisa el plan y después usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de comenzar el filtrado, vuelve al codespace y prepara el repositorio y el terminal.

1. Vuelve al codespace. Si está detenido, reinícialo.
2. Confirma que la PR de calificaciones con estrellas se ha combinado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para resaltar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

El prompt empezará con `#7` (o un número similar). El prefijo `#` permite añadir al contexto una incidencia o PR de GitHub.

## Planifica el filtrado

La planificación permite acordar cómo implementar una funcionalidad antes de delegar el trabajo a Copilot.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y propondrá un enfoque.

3. Responde a las preguntas sobre el funcionamiento del filtrado según tus preferencias.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que revise cualquier parte que no refleje tus decisiones.

## Autoriza Autopilot

Cuando hayas escrito y revisado el plan, podrás implementarlo con Autopilot.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente que muestre tu versión.
2. Confirma que el indicador del modo muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa el plan primero. Si Copilot informa de dependencias ausentes o conflictos de puertos, resuelve el problema de configuración antes de dar las comprobaciones por terminadas.

## Revisa y verifica la implementación

Revisa el código generado antes de combinarlo, como harías con cualquier otro cambio.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los errores.

## Explora la nueva funcionalidad

Inicia la aplicación y ábrela a través del puerto reenviado de Codespaces para comprobar que funciona.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones acordadas en el plan.
4. Confirma que las opciones para restablecer los filtros y los resultados vacíos coinciden con la incidencia y tus decisiones.
5. Vuelve al codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has usado varios modos de conversación para crear y revisar una funcionalidad. En esta lección, has:

- iniciado una conversación desde una incidencia de GitHub.
- definido requisitos en modo Plan.
- implementado la funcionalidad con Autopilot.
- revisado el código.
- validado la funcionalidad manualmente en un navegador reenviado.

A continuación, profundizarás en cómo se genera el código y cómo seguir las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
