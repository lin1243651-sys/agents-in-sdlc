---
title: "Lección 1 - Instala GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu codespace, conoce la herramienta y encuentra la incidencia de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un asistente de programación con agentes que se ejecuta en el terminal. Permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Instalarás la herramienta, te autenticarás y la prepararás para el resto del taller.

En esta lección, vas a:

- instalar GitHub Copilot CLI con npm.
- autenticarte con tu cuenta de GitHub.
- confiar en el repositorio del taller y probar una conversación.
- encontrar la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

## Escenario

Tu equipo está empezando a usar agentes de IA para abordar un backlog creciente. Copilot CLI ofrece esta capacidad desde el terminal, donde ya trabajan muchos desarrolladores.

## Instala Copilot CLI

Puedes instalar Copilot CLI con [npm][install-cli], WinGet o Homebrew. GitHub Codespaces incluye Node.js, así que utilizarás npm.

1. Vuelve al codespace y abre un terminal.
2. Comprueba que Node.js esté instalado y cumpla el requisito de versión:

   ```bash
   node --version
   ```

   Debe aparecer la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

## Autentícate con GitHub

La primera vez que inicies Copilot CLI, te pedirá que te autentiques.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se solicita, sigue las instrucciones del código de dispositivo para autenticarte y autorizar Copilot CLI.
3. Copilot CLI mostrará un mensaje similar a este:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirma que la ruta corresponde al repositorio Tailspin Toys y selecciona **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En Codespaces, puede que ya estés autenticado mediante tu sesión de GitHub. Si Copilot CLI no solicita autenticación, ya está listo.

## Familiarízate con la CLI

Los comandos introducidos en el prompt habitual del shell se ejecutan directamente en el codespace. Una vez iniciado Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, usa las teclas de dirección para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y, después, vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos disponibles en la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que está disponible el servidor MCP integrado de GitHub.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La usarás después de realizar un primer cambio rápido.

> [!TIP]
> Una conversación normal de Copilot CLI usa la rama que esté activa en el terminal; no crea automáticamente un árbol de trabajo. Crearás una rama de funcionalidad antes de cada cambio.

## Usa el acceso directo del taller

Normalmente, Copilot CLI solicita permiso para usar herramientas. En este taller, lo reiniciarás con `--yolo` para eliminar esas solicitudes de aprobación en el codespace.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un codespace o una máquina virtual, y nunca lo configures como alias predeterminado para el trabajo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning].

La opción `--enable-all-github-mcp-tools` habilita las herramientas MCP de lectura y escritura de GitHub que se usarán más adelante. Aunque el codespace limita el acceso a tu equipo local, los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Reinícialo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto y sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, usarás `copilot --resume` para volver a la misma conversación y rama.

## Resumen y siguientes pasos

En esta lección, has:

- instalado GitHub Copilot CLI con npm.
- iniciado sesión con tu cuenta de GitHub.
- confiado en el repositorio del taller y probado una conversación.
- encontrado la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

A continuación, [empezarás tu primer cambio acotado][next-lesson] y usarás Copilot CLI para mostrar calificaciones con estrellas en las tarjetas de juegos.

## Recursos

- [Instala GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
