---
title: "Ejercicio 2 - Añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso el cambio más sencillo de una aplicación suele requerir actualizar varios archivos y realizar operaciones como ejecutar pruebas. El flujo de trabajo de un desarrollador puede consistir en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, descubrir qué archivo se pasó por alto y volver a actualizarlo. La lista continúa. El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE: explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Explora cómo usarlo para añadir funciones al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y cloud agent ofrecen experiencias distintas. El modo agente trabaja en el IDE para que puedas obtener comentarios e interactuar con rapidez. Cloud agent está diseñado como un compañero de programación que trabaja de forma asíncrona, como un miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio, aprenderás a usar el modo agente para explorar el proyecto, identificar archivos, implementar funciones en la interfaz y la capa de datos, y revisar los cambios y las pruebas antes de combinarlos.

En concreto, vas a:

- usar el modo agente para explorar el proyecto, encontrar archivos relevantes y coordinar cambios.
- implementar la función de filtrado en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados antes de combinarlos.

## Escenario

El catálogo de juegos crece y quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya creaste un helper de editoriales; ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web Tailspin Toys

Antes de hacer cambios, explora el sitio web Tailspin Toys para conocer su funcionalidad actual. Es una plataforma de financiación colectiva de juegos de mesa con una temática para desarrolladores. Permite mostrar una lista de juegos y ver sus detalles. El sitio es una única aplicación Astro que genera páginas en lenguaje de marcado de hipertexto (HTML) estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante el mapeador objeto-relacional (ORM) Drizzle; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz del lado del cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade mediante un pequeño script Astro acotado que usa las API estándar del modelo de objetos del documento (DOM).

### Inicia el sitio web

Para facilitar la ejecución del sitio, el proyecto incluye la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Inícialo en el codespace:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Vuelve a `main`, actualízala y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, mostrará un aviso con la URL.

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro que te pregunta si quieres abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio seleccionando `http://localhost:4321` en el terminal mientras mantienes pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux).

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost en el terminal, se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor web que se ejecuta en el codespace.

### Explora el sitio web

Las funciones principales incluyen:

- **Página de inicio**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

La implementación inicial del sitio funciona, pero quieres añadir funciones. Empieza por revisar el backlog: la plantilla del proyecto creó incidencias automáticamente. Pide a GitHub Copilot que te muestre esas tareas.

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve al codespace, abre Copilot Chat y selecciona **New Chat**. El modo y el modelo varían por ejercicio; cada paso indicará qué seleccionar.

1. Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones los indican cuando corresponda.
2. En el menú de agentes de la vista Chat, selecciona **Agent**. El agente puede planificar y aplicar cambios en varios archivos, ejecutar comandos del terminal y usar herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

3. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo concreto. Se usa Claude Sonnet 4.5 porque se utilizó al crear el taller y se espera que produzca sugerencias relativamente coherentes y facilite la experiencia. Los modelos de lenguaje grandes (LLM) son probabilísticos, por lo que las sugerencias pueden diferir de los ejemplos. Es normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje grandes (LLM), Copilot podría usar un comando distinto del Protocolo de contexto del modelo (MCP) y completar la tarea igualmente.

4. Pide a Copilot que muestre el backlog enviando este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

5. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
6. Revisa la lista generada.

Copilot también habrá priorizado las incidencias según cuáles cree que serán más útiles para el usuario.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto para esta tarea. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funciones, que incluyen:
   - una descripción de la arquitectura.
   - principios de diseño de componentes y pautas de pruebas y accesibilidad.
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar instrucciones, dividir conjuntos extensos en partes más manejables y señalar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios independientes en la aplicación:

- Añadir o mejorar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar las pruebas del comportamiento del filtrado.
- Actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código. El modo agente puede realizar estas tareas.

1. Continúa la conversación actual con Copilot o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)
3. Confirma que **Agent** sigue seleccionado en el menú de agentes.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la función según la incidencia relacionada del backlog. Envía este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la función. Deberías verlo identificar los helpers de datos, la interfaz y las pruebas, modificar archivos y ejecutar las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar varios minutos, según la complejidad de la tarea y del código. Mientras trabaja, pueden aparecer los controles **Keep** y **Undo** en el editor. Al terminar, habrá un control para cada cambio; no es necesario seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas o que algunas pruebas fallen durante el proceso. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completa de Copilot Agent Mode en Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva función.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio, has explorado cómo usar el modo agente de GitHub Copilot para añadir funciones al sitio Tailspin Toys. Has aprendido a:

- explorar el proyecto, encontrar archivos relevantes y coordinar cambios.
- implementar el filtrado en la interfaz y la capa de datos.
- revisar los cambios y las pruebas antes de conservarlos.

A continuación, [probarás la función con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementa la paginación

Como crece la lista de juegos, será necesario habilitar la paginación. Usa las habilidades de este ejercicio para pedir a Copilot que añada paginación. Ten en cuenta lo siguiente:

- Sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones del proyecto.
- Decide cómo implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- En el prompt, proporciona a Copilot las directrices necesarias para implementar el comportamiento que quieres.
- Puede que tengas que iterar con Copilot, pedir cambios y aportar contexto. Este es el flujo de trabajo habitual con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: usos y diferencias][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
