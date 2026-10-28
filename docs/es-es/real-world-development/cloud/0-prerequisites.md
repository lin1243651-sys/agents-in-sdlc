---
title: "Ejercicio 0: requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de GitHub Copilot cloud agent, prepara el entorno. Crearás una copia propia del repositorio Tailspin Toys e iniciarás un [codespace][codespaces] para editar archivos de instrucciones y revisar el trabajo del agente.

## Configura el repositorio del laboratorio

Para obtener una copia del repositorio con el código que crearás, crea una instancia desde la [plantilla][template-repository]. La nueva instancia incluirá todos los archivos necesarios para el laboratorio y la usarás durante los ejercicios.

1. En una nueva ventana del navegador, abre la [plantilla Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** en la página del repositorio y, después, selecciona **Create a new repository**.

   ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si realizas el taller en un evento de GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, puedes crear el repositorio en una organización donde tengas acceso a GitHub Copilot.

   ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio (**organization-or-user-name/repository-name**), ya que la necesitarás más adelante.

## Crea un codespace

A continuación, usarás un codespace para completar los ejercicios.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Ofrece un entorno de desarrollo integrado (IDE) completo con compatibilidad para varios lenguajes de programación, extensiones y herramientas.

1. Abre el repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

   ![Selecciona el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un codespace.

   ![Crea un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará varios minutos, aunque será mucho más rápida que instalar manualmente todos los servicios. Mientras esperas, puedes explorar otras funciones de GitHub Copilot, que usarás en los ejercicios siguientes.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está preparado para ejecutarse en un codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos entornos incluyen los requisitos necesarios. Para trabajar en local, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que usa el codespace.

## Resumen

Has creado una copia del repositorio del laboratorio e iniciado la creación del codespace que usarás junto con Copilot cloud agent.

## Siguiente paso

A continuación, añadirás instrucciones personalizadas para el agente. Continúa con el [Ejercicio 1: instrucciones personalizadas][next-lesson].

## Recursos

- [Información general de GitHub Codespaces][codespaces]
- [Crea un repositorio a partir de una plantilla][template-repository]
- [Guía de inicio de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces]: https://github.com/features/codespaces
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
[next-lesson]: ../1-custom-instructions/
