---
title: "Ejercicio 3 - Prueba tu funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: modo agente][previous-lesson] · [Siguiente lección: agentes personalizados →][next-lesson]

Has creado la función de filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios (PR), confirmarás que funciona en el navegador. Configurarás el **servidor MCP de Playwright** para que Copilot controle un navegador real, pruebe la función y te ayude a publicar la rama y abrir la PR.

En este ejercicio, vas a:

- aprender qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- añadir el servidor MCP de Playwright al espacio de trabajo.
- pedir a Copilot que pruebe manualmente el filtrado en un navegador.
- publicar la rama y abrir una PR.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente resulta mucho más potente cuando puede acceder a recursos externos al editor. [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) es un estándar que permite a Copilot comunicarse con herramientas y servicios externos. Los agentes pueden obtener información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el modelo de lenguaje grande (LLM) y las herramientas, incluidas las que aportan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que sirve de puente entre el agente y los servicios externos. El servidor gestiona la comunicación con herramientas externas, como interfaces de programación de aplicaciones (API) existentes o herramientas locales en paquetes del gestor de paquetes de Node.js (npm). Cada servidor MCP ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares son:

- **[Servidor MCP de GitHub][github-mcp-server]**: proporciona acceso a varias interfaces de programación de aplicaciones (API) para gestionar repositorios de GitHub. Permite, por ejemplo, crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Servidor MCP de Playwright][playwright-mcp-server]**: proporciona automatización del navegador mediante Playwright. Permite navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP con herramientas y recursos distintos. GitHub aloja un [registro de MCP][mcp-registry] para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Antes de usar uno, revisa su código fuente, verifica quién lo publica y valora las implicaciones de seguridad. Usa servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo.

1. Abre `.vscode/mcp.json` en el codespace.
2. Confirma que ya está configurado el servidor `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La entrada `github` viene con la plantilla del proyecto; por eso Copilot pudo leer el backlog en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalarlo localmente. VS Code se conecta mediante el protocolo de transferencia de hipertexto (HTTP) y te autentica con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor MCP de Playwright

Añade un segundo servidor para que Copilot pueda controlar un navegador.

1. En `.vscode/mcp.json`, añade `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

Al contrario que el servidor remoto `github`, `playwright` es un servidor local: VS Code lo inicia en el equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin una ventana visible, necesario en un codespace, que no tiene escritorio gráfico.

> [!NOTE]
> Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador ya está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia y confía en el servidor

VS Code inicia los servidores MCP **cuando hacen falta**. La primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, selecciona **Start** sobre la entrada `playwright` para iniciar el servidor y comprobar la configuración.
2. Si VS Code solicita confirmación, revisa la configuración y selecciona la opción para confiar en el servidor.

Iniciarlo manualmente es opcional; si no lo haces, VS Code lo iniciará y solicitará confianza cuando Copilot necesite sus herramientas.

## Prueba la función de filtrado

El servidor MCP de Playwright permite a Copilot controlar un navegador real: puede abrir una página, navegar, aplicar filtros y leer los resultados para resumir lo observado. Así puedes comprobar el comportamiento sin salir de la conversación.

El servidor MCP de Playwright se basa en el [árbol de accesibilidad][playwright-mcp-server] de la página, no en capturas de pantalla. Por eso el agente interpreta elementos estructurados y etiquetados —como botones, enlaces y elementos de lista— de forma parecida a las tecnologías de asistencia. Una comprobación funcional rápida sirve también como verificación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pedirás a Copilot que compruebe el filtrado en el navegador.

1. Envía este prompt:

   ```text
   Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

   1. Go to the games page and note how many games are listed.
   2. Apply a category filter and confirm the list updates to only show games in that category.
   3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
   4. Combine a category and a publisher filter and confirm the results respect both.

   Report what you observe at each step, and call out anything that does not behave as expected.
   ```

2. Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará sus observaciones. Lee el informe y compáralo con los criterios de aceptación de la incidencia. Si algo no funciona, haz preguntas de seguimiento o pide a Copilot que corrija el código antes de abrir una PR.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si detuviste el servidor de desarrollo, vuelve a iniciarlo. La primera vez, Copilot podría descargar un navegador; si indica que falta, pídele que ejecute `npx playwright install chromium`.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/

## Publica la rama y crea una solicitud de incorporación de cambios

Una vez que hayas confirmado que la función funciona, abre una solicitud de incorporación de cambios (PR) para que el equipo pueda revisarla. Primero publica la rama `filtering-vscode`.

1. Abre **Source Control** en el codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una PR, como usar github.com o la interfaz de línea de comandos de GitHub (CLI). Como ya estás usando GitHub Copilot, pídele que la cree. Puede localizar la incidencia correspondiente y vincularla a la PR.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. En el menú de agentes, confirma que está seleccionado **Agent** para que Copilot pueda usar las herramientas de GitHub.
3. Envía este prompt:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que use una herramienta de GitHub, puede pedirte que te autentiques; sigue las indicaciones.
5. Comprueba cómo Copilot busca entre las incidencias, encuentra la adecuada y crea la PR.
6. Selecciona el enlace que proporciona Copilot para revisar la PR, pero **no la combines todavía**.

## Resumen y siguientes pasos

En este ejercicio, has:

- aprendido qué es MCP y cómo los servidores MCP amplían las herramientas de Copilot.
- añadido el servidor MCP de Playwright al espacio de trabajo.
- pedido a Copilot que probara manualmente el filtrado en un navegador.
- publicado la rama y abierto una PR.

A continuación, crearás un [agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué se habla tanto de ello][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
