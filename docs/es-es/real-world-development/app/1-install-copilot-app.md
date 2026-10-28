---
title: "Lección 1 - Instala la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio creado desde la plantilla, conoce el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo con agentes. Se basa en GitHub Copilot CLI y se integra con GitHub, por lo que los repositorios, ramas y procesos de CI funcionan directamente. Está diseñada para dirigir varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, y automatizar tareas repetitivas. Ya tienes Node.js y tu copia del proyecto; ahora instalarás la aplicación y conectarás el repositorio.

En esta lección, vas a:

- instalar la aplicación GitHub Copilot e iniciar sesión.
- añadir el proyecto a la aplicación desde su repositorio de GitHub.
- conocer el espacio de trabajo y el backlog creado para ti a partir de la plantilla.
- probar un chat rápido para conocer la aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para abordar un backlog creciente. La aplicación Copilot ofrece un espacio central para dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás la aplicación, la conectarás y empezarás a conversar sobre el proyecto.

> [!NOTE]
> Necesitas un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Con Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

La aplicación está disponible para Windows, macOS y Linux. Instálala, autentícate y añade el repositorio Tailspin Toys.

1. En un navegador, abre la [página de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación.
4. Selecciona **Sign in to GitHub** y sigue las indicaciones para autenticarte. Si usas GitHub Enterprise Cloud con residencia de datos, selecciona **Use GitHub Enterprise** y, cuando se solicite, introduce el nombre de host `*.ghe.com`. GitHub Enterprise Server no es compatible.
5. Si el proceso de incorporación te pide elegir un repositorio, selecciona el repositorio Tailspin Toys que creaste a partir de la plantilla. También puedes omitir este paso y añadirlo más adelante.
6. Elige un tema cuando se te solicite y termina la configuración inicial.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece, no tienes que volver a añadirlo.
8. Si todavía no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Usa el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En esta disposición, las carpetas de repositorio, como `tailspin-toys`, aparecen agrupadas en **Sessions**, con las sesiones individuales debajo de cada repositorio.

## Familiarízate con el espacio de trabajo

La aplicación organiza el trabajo en varias áreas de la barra lateral:

- **New**: inicia una sesión de chat con Copilot.
- **Pull requests**: explora y filtra solicitudes de incorporación de cambios, comprueba el estado de CI y revísalas sin salir de la aplicación.
- **Issues**: explora y filtra incidencias e inicia sesiones desde ellas.
- **Automations**: guarda tareas de agente para ejecutarlas según una programación o cuando las solicites. Sirven para gestionar listas de tareas, mantenimiento periódico y otros trabajos repetitivos. Se mencionan como un paso siguiente, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, plugins, habilidades y otros componentes. La usarás para configurar Playwright MCP.
- **Chats**: mantén conversaciones breves para preguntas e ideas que no necesitan una rama ni un espacio de trabajo propio. Esta sección aparece después del primer chat. Si no está visible, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de repositorio y las sesiones de agente. Selecciona un repositorio, como `tailspin-toys`, para abrir su vista, o una sesión para volver a la conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, de modo que puedes usar varias a la vez sin que sus cambios entren en conflicto. Empezarás tu primera sesión al añadir las calificaciones con estrellas.

> [!TIP]
> Si tienes dudas, pregúntale a Copilot. Puede ayudarte a saber cómo hacer algo o si es posible.

### Encuentra el backlog creado para ti

Explora el backlog que se creó al generar el repositorio desde la plantilla.

1. En la barra lateral, selecciona la carpeta del repositorio Tailspin Toys bajo **Sessions**, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por título, sin dar por sentado su número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer sus detalles. Cada incidencia también permite iniciar una sesión de agente. Empezarás por la incidencia de filtrado después de realizar un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** solo muestran elementos de los repositorios que has añadido a la aplicación Copilot. Añade otros repositorios para ver sus elementos de trabajo.

## Prueba un chat rápido

Los chats rápidos permiten hacer una pregunta o proponer ideas sin crear una rama ni un árbol de trabajo. Son adecuados para consultas breves que no necesitan una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat. Si **Chats** aún no aparece, selecciona **+** junto a **Sessions** para iniciar el primer chat; **Chats** aparecerá después.
2. Pregunta cómo funcionan las sesiones de la aplicación:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la conversación. Verás que cada sesión usa su propio árbol de trabajo de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar otra cuando quieras.

## Resumen y siguientes pasos

Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub.
- añadir un proyecto desde su repositorio de GitHub.
- familiarizarte con el espacio de trabajo y encontrar el backlog en **Issues**.
- usar un chat rápido para hacer una pregunta breve.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y le pedirás que muestre las calificaciones con estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Primeros pasos con la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
