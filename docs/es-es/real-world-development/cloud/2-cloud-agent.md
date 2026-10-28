---
title: "Ejercicio 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: instrucciones personalizadas][previous-lesson]

Es probable que casi todas las organizaciones tengan deuda técnica: problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog porque no ha habido tiempo para implementarlas. GitHub Copilot cloud agent está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así, puede encargarse de tareas tediosas, acelerar el desarrollo y permitir que los desarrolladores se centren en cuestiones de mayor alcance.

En este ejercicio, vas a:

- personalizar el entorno de generación de código.
- comprobar cómo se protegen las operaciones.
- crear incidencias claramente delimitadas.
- asignar trabajo a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: el sitio carece de ella por completo. Como primer paso, el equipo quiere añadir comentarios de documentación para TypeScript (TSDoc) a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere que la página de detalles de cada juego muestre otros títulos de su misma categoría, para que la gente pueda seguir explorando. No necesita una implementación pulida todavía; solo una versión que permita hacer pruebas de aceptación de la experiencia. Este trabajo bloquea ese objetivo, aunque hay otras incidencias de mayor prioridad. Son tareas que pueden quedar relegadas y que conviene asignar a cloud agent: el agente trabaja de forma asíncrona mientras los desarrolladores se ocupan de otras tareas y, después, pueden revisar su propuesta.

## Presentación de GitHub Copilot cloud agent

[GitHub Copilot cloud agent][cloud-agent] puede realizar tareas en segundo plano de forma parecida a un desarrollador. Puedes usarlo de varias maneras, por ejemplo, [asignándole una incidencia de GitHub][assign-issue]. Cuando se la asignas, Copilot crea una PR en borrador para registrar el progreso, configura un entorno y empieza a trabajar. Puedes inspeccionar su sesión mientras sigue activa o después de completarse. Cuando la solución esté lista para revisarla, Copilot te menciona en la PR.

## La importancia de unas instrucciones bien delimitadas

GitHub Copilot no elimina la necesidad de pensar. Incluso las tareas aparentemente sencillas pueden tener complejidad cuando se examinan de cerca; sigue las [prácticas recomendadas para delimitar tareas de cloud agent][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el proceso. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

Los ejercicios anteriores ya han presentado las instrucciones personalizadas y cómo orientan a Copilot. Si ya completaste el ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, aprovecha ahora para revisarlo. Antes de asignar trabajo a cloud agent, inspecciona los archivos de instrucciones del repositorio sin modificarlos. Así podrás reconocer su efecto más adelante.

1. En la interfaz web de GitHub o en un codespace, abre `.github/copilot-instructions.md` y revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios de documentación TSDoc.
2. Abre `.github/instructions/unit-tests.instructions.md` y observa que el frontmatter `applyTo` limita estas instrucciones a los archivos `**/*.test.ts`.
3. En el siguiente apartado, cuando asignes a cloud agent la incidencia `Code lacks documentation`, observa si la PR incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios que procedan de estos archivos de instrucciones. Volverás a revisarlo en un ejercicio posterior.

## Configura el entorno de desarrollo de cloud agent

Para crear código y comprobarlo se necesita un entorno específico y, normalmente, ejecutar scripts de configuración. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software. Cloud agent usa [GitHub Actions][github-actions] para configurar su entorno. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Este flujo le proporciona las herramientas de desarrollo y dependencias necesarias. En este laboratorio ya está configurado para instalar Node.js y las dependencias del proyecto, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y sembrada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si necesitas clonar el repositorio como parte de la configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente al terminar.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (la aplicación completa ahora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene dos aspectos importantes:

- Contiene un único trabajo, `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar el flujo manualmente desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

La documentación desactualizada o inexistente es deuda técnica que ralentiza el trabajo y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot puede ayudar a crear documentación, por lo que esta es una buena incidencia para asignar a cloud agent. El agente trabajará en segundo plano; en un ejercicio posterior volverás para revisar su trabajo.

1. Abre el repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de creación.
4. Selecciona **Blank issue**.
5. Establece **Title** en `Code lacks documentation`.
6. Establece **Description** en:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera uno o dos minutos y actualiza la página.
12. Espera a que Copilot cree una lista de tareas. Puede tardar varios minutos mientras configura el entorno, analiza el proyecto y determina cómo abordar la incidencia.
13. Revisa la lista y las tareas previstas.
14. Desplázate por la cronología de la PR y busca la actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Desplázate por la sesión en directo para observar cómo Copilot explora el código y comprende el estado del proyecto, se detiene a planificar y tomar decisiones, y crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales de cloud agent es trabajar de forma asíncrona para que puedas ocuparte de otras tareas. Aprovecharás esa capacidad para asignarle otra tarea y, mientras tanto, desarrollar una funcionalidad.

## Añade una sección de juegos relacionados

Una de las ventajas de GitHub Copilot cloud agent es que permite dividir el trabajo: puedes ocuparte de una tarea mientras el agente se encarga de otra. Aunque añadir una sección de juegos relacionados quizá no lleve mucho tiempo, puedes dedicar ese tiempo a otro trabajo. Asígnale esta tarea a cloud agent.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de creación.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Establece **Title** en `Show related games on the game details page`.
6. Establece **Description** en:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, aparecerá el emoji 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Cloud agent trabaja de forma parecida a un ingeniero de software: no hace falta supervisarlo continuamente, pero debes revisar el resultado cuando termine. Ahora puedes centrarte en crear y usar agentes personalizados.

## Resumen y siguientes pasos

Has explorado [GitHub Copilot cloud agent][cloud-agent]. Puedes asignarle incidencias para abordar deuda técnica, crear funcionalidades o migrar código entre frameworks de forma asíncrona. En este ejercicio, has:

- explorado cómo personalizar el entorno de generación de código.
- revisado cómo se protegen las operaciones.
- aprendido por qué es importante delimitar las incidencias con claridad.
- asignado incidencias a Copilot.

Mientras cloud agent trabaja, crearás y usarás agentes personalizados. [Cloud agent también puede usar servidores Model Context Protocol (MCP)][cloud-agent-mcp] e instrucciones personalizadas.

## Recursos

- [Acerca de Copilot cloud agent][cloud-agent]
- [Asigna incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración de cloud agent][cloud-agent-best-practices]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
