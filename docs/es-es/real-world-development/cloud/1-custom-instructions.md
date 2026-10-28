---
title: "Ejercicio 1 - Instrucciones personalizadas (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: requisitos previos][previous-lesson] · [Siguiente lección: GitHub Copilot cloud agent →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita información de fondo, asegúrate de que tenga acceso a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda tanto qué debe hacer como cómo debe hacerlo.

En este ejercicio, vas a:

- explorar cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos de rutas.
- añadir un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y de la interfaz de línea de comandos (CLI), aquí no compararás prompts antes y después. Copilot cloud agent trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones cuando revises las solicitudes de incorporación de cambios que cree el agente.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices para las prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación para TypeScript (TSDoc).
- Cada archivo debe empezar con un bloque de comentarios que describa su propósito.

Los archivos de instrucciones proporcionarán a Copilot la información necesaria para seguir estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que comprenda el estilo de programación y los requisitos del proyecto. Así puedes orientar sus sugerencias para que proponga código y fragmentos más pertinentes. Puedes definir convenciones de programación, bibliotecas y los tipos de comentarios que prefieres. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivos y tareas.

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe incluir información del proyecto relevante para la mayoría de las solicitudes de chat o de la interfaz de línea de comandos (CLI), como la pila tecnológica, una descripción del proyecto, prácticas recomendadas y directrices generales.
- Los archivos `.github/instructions/*.instructions.md` pueden proporcionar instrucciones para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En un entorno de desarrollo integrado (IDE), los archivos de instrucciones se usan para generar código en Copilot Chat, pero no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Por último, Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md; consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

- Mantén `copilot-instructions.md` centrado en directrices del proyecto: qué se está creando, la estructura del proyecto y los estándares globales de programación.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo, como pruebas unitarias, componentes Astro y la capa de datos, o para tareas concretas.
- Escribe en lenguaje natural, con directrices claras y ejemplos de código correcto e incorrecto.

No hay una única forma de crear archivos de instrucciones ni de usar IA. La experimentación permite descubrir qué funciona mejor para cada proyecto y entorno.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los archivos de este proyecto, verás que hay instrucciones para distintos tipos de tareas, incluidas las [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudar a crear archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El taller del entorno correspondiente lo explicará cuando sea pertinente.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones que incluye el repositorio —un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas— en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Lee la descripción del proyecto y las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, revisa también las directrices anidadas **GitHub Actions Workflows**.
3. Explora la carpeta `.github/instructions`, donde hay instrucciones para Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md` y revisa el campo `applyTo` del principio. Este glob, relativo a la raíz del repositorio, se aplica a los archivos TypeScript de pruebas que coincidan, como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de comentarios de archivo.

## Añade un estándar para todo el repositorio

La única edición de este ejercicio será añadir un estándar para todo el proyecto: documentación en el código mediante comentarios TSDoc y un encabezado de comentarios para cada archivo. Cloud agent seguirá estas instrucciones cuando trabaje en incidencias que le asignes más adelante. Antes de editar el archivo, crea una rama para trabajar en el cambio.

1. En el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Vuelve a abrir `.github/copilot-instructions.md`.
3. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que todavía no exige documentación dentro del código.
4. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en el apartado anterior, los archivos de instrucciones pueden estar en `.github/copilot-instructions.md` para directrices generales del proyecto, o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios de documentación que acabas de añadir.

## Confirma, envía y combina la actualización de instrucciones

Cloud agent lee las instrucciones de la rama a la que apunta la incidencia. En el próximo ejercicio, el agente partirá de `main`, así que tus cambios deben llegar a `main`.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Cloud agent leerá las instrucciones de la rama a la que apunta la incidencia; por eso deben estar en `main` antes de asignarle trabajo en el ejercicio siguiente.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar el cambio directamente en `main`. La rama permite practicar un flujo de trabajo habitual.

## Resumen y siguientes pasos

Has:

- explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto.
- añadido un estándar para todo el repositorio a `.github/copilot-instructions.md`.

Verás cómo se aplica en las solicitudes de incorporación de cambios que genere cloud agent en los ejercicios siguientes.

A continuación, [asignarás tu primera incidencia a GitHub Copilot cloud agent][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
