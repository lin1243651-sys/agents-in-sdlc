---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: requisitos previos][previous-lesson] · [Siguiente lección: modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de fondo, asegúrate de que tenga acceso a ese contexto. Hay varias formas de compartirlo con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan directrices para la generación de código.

En este ejercicio, vas a:

- explorar cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones del repositorio y las instrucciones específicas de rutas.
- generar la primera parte de los datos del filtrado, un helper de editoriales, con las instrucciones actuales.
- añadir un estándar para todo el repositorio a `.github/copilot-instructions.md`.
- repetir el prompt y observar cómo el código aplica el nuevo estándar.
- confirmar los cambios en `main` para que Copilot use las instrucciones actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares. Copilot no es determinista. El objetivo es observar la tendencia del cambio después de actualizar las instrucciones, no obtener una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices para las prácticas de desarrollo. Entre ellas:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación para TypeScript (TSDoc).
- Cada archivo debe empezar con un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones, proporcionarás a Copilot el contexto necesario para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que comprenda mejor el estilo de programación y los requisitos del proyecto. Así puedes orientar sus sugerencias para que proponga código y fragmentos más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas y los tipos de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto útil para la mayoría de las solicitudes, como la pila tecnológica, una descripción general, prácticas recomendadas y directrices globales.
- Los archivos `.github/instructions/*.instructions.md` pueden proporcionar instrucciones para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, pero no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, la interfaz de línea de comandos (CLI) de Copilot y Copilot cloud agent usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también admite archivos de instrucciones de otros estándares, como AGENTS.md y CLAUDE.md; consulta la [referencia de compatibilidad][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

- Esta conversación no cubre en profundidad la creación de archivos de instrucciones, pero los ejemplos del proyecto muestran un enfoque representativo. Mantén `copilot-instructions.md` centrado en directrices del proyecto, como una descripción de lo que se desarrolla, la estructura del proyecto y los estándares globales.
- Usa `*.instructions.md` para tipos de archivo concretos, como pruebas unitarias, componentes Astro y la capa de datos, o para tareas determinadas.
- Escribe en lenguaje natural y mantén claras las directrices. Incluye ejemplos de código correcto e incorrecto.

No hay una única forma de crear archivos de instrucciones ni de usar IA. La experimentación permitirá descubrir qué funciona mejor para tu proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los archivos de este proyecto, verás instrucciones para muchas tareas, como las [actualizaciones de interfaz][ui-instructions] y los archivos [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en la interfaz de línea de comandos (CLI) de Copilot. El ejercicio de cada entorno indicará cómo usarla cuando corresponda.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora las instrucciones personalizadas del proyecto

Lee los archivos de instrucciones que incluye el repositorio: un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas diferentes. Puedes abrirlos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Lee la descripción y las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, revisa también **GitHub Actions Workflows**.
3. Explora `.github/instructions`; contiene instrucciones para Astro, Drizzle, pruebas y otras tareas.
4. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo`, que define el glob relativo a la raíz del repositorio. Por ejemplo, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
5. Lee las instrucciones para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y ejemplos del proyecto. Las rutas son relativas a ese archivo.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de comentarios de archivo.

## Crea una rama

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

Para observar el efecto de las instrucciones personalizadas, primero enviarás un prompt con el archivo de instrucciones actual. Después lo actualizarás y volverás a enviar el mismo prompt para comparar los resultados.

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará qué seleccionar.

1. Cierra las pestañas de editor anteriores para que Copilot use solo el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar dónde debe ubicarse el helper.
3. En el menú de agentes de Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará los cambios, posiblemente en el helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe primero un cliente `db` y devuelve una matriz tipada de editoriales. Estas convenciones proceden de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto no incluye comentarios TSDoc ni un encabezado de comentarios al principio del archivo.

> [!CAUTION]
> Copilot puede añadir comentarios aunque no se los pidas. No pasa nada; el objetivo es observar una mejora de la coherencia después de actualizar las instrucciones.

## Añade un estándar para todo el repositorio

`.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade reglas de documentación a los estándares de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que aún no contiene una regla de documentación en el código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste antes, puedes usar `.github/copilot-instructions.md` para los estándares generales del proyecto y `*.instructions.md` para instrucciones específicas de lenguajes, tipos de archivo o tareas. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios de documentación que acabas de añadir.

## Repite el prompt y observa el cambio

Vuelve a enviar el prompt original para comprobar cómo influyen las nuevas instrucciones.

1. En Copilot Chat, selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo.
3. Envía el mismo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Acabas de orientar a Copilot para que aplique un estándar nuevo a código real, que servirá de base para el siguiente ejercicio.

## Confirma las instrucciones y envía la rama

Confirma las instrucciones y todos los archivos modificados por Copilot para el helper.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba los cambios:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y el helper, además de todos los archivos modificados por Copilot para crear la base del filtrado:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Con la rama enviada, crea una solicitud de incorporación de cambios (PR) y vincúlala a la incidencia del backlog sobre los estándares de documentación. Copilot puede encontrar la incidencia, crear la PR y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub. La plantilla del proyecto ya configura las herramientas de GitHub para el espacio de trabajo, así que no tienes que configurar nada. La primera vez que Copilot use una herramienta, VS Code podría pedirte que inicies sesión en GitHub; sigue las indicaciones. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante el Protocolo de contexto del modelo (Model Context Protocol, MCP).

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que está seleccionado **Agent** en el selector de agentes.
3. Pide a Copilot que encuentre la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual. Indícale que la PR debe cerrar esa incidencia y describir también la base del helper de editoriales que se usará en el filtrado:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot solicite permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** según corresponda.
6. Cuando se cree la PR, pide a Copilot que la combine con `main`, vuelva a `main` en local y descargue los cambios más recientes:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Revisa las solicitudes de permiso para las acciones de GitHub y los comandos del shell antes de seleccionar **Allow**.

Con Copilot, has creado y combinado una solicitud de incorporación de cambios.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- explorar cómo las instrucciones personalizadas del repositorio y las instrucciones específicas de rutas proporcionan contexto al agente.
- enviar un prompt de generación de código con las instrucciones existentes y observar el resultado.
- añadir un estándar para todo el repositorio a `.github/copilot-instructions.md`.
- repetir el prompt y observar cómo el código propuesto aplica el estándar nuevo.
- confirmar las instrucciones y el helper en `main` para el ejercicio siguiente.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson] mientras Copilot añade una función al código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Instrucciones personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
