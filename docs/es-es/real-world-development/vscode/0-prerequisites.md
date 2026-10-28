---
title: "Ejercicio 0: requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara el entorno. Crearás una copia propia del repositorio Tailspin Toys, iniciarás un [codespace][codespaces] y confirmarás que GitHub Copilot Chat está disponible en el editor.

## Configura el repositorio del laboratorio

Para crear una copia del repositorio con el código que desarrollarás, crea una instancia desde la [plantilla][template-repository]. La nueva instancia incluirá todos los archivos necesarios para el laboratorio y la usarás durante los ejercicios.

1. En una nueva ventana del navegador, abre la [plantilla Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** en la página del repositorio y, después, selecciona **Create a new repository**.

   ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si realizas el taller en un evento de GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

   ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio (**organization-or-user-name/repository-name**), ya que la necesitarás más adelante.

> [!NOTE]
> **El backlog está preparado**
>
> Al crear el repositorio desde la plantilla, se genera automáticamente un backlog de incidencias de GitHub. Trabajarás con esas incidencias durante el taller; no tienes que crear ninguna.

## Crea un codespace

Usarás un codespace para completar los ejercicios. [GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Ofrece un entorno de desarrollo integrado (IDE) completo con compatibilidad para varios lenguajes de programación, extensiones y herramientas.

1. Abre el repositorio nuevo.
2. Selecciona el botón verde **Code**.

   ![Selecciona el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un codespace.

   ![Crea un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará varios minutos, aunque será mucho más rápida que instalar todos los servicios manualmente. Mientras esperas, puedes explorar otras funciones de GitHub Copilot, que usarás en los ejercicios siguientes.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> El taller se puede ejecutar en un codespace o en un [contenedor de desarrollo][dev-containers] local. Para trabajar en local, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que usa el codespace.

## Usa GitHub Copilot Chat y el modo agente

Para acceder al modo agente de GitHub Copilot Chat, necesitas tener instalada la extensión GitHub Copilot Chat en el IDE. Si usas GitHub Codespaces, ya debería estar instalada.

> [!TIP]
> Si no tienes instalada la extensión GitHub Copilot Chat, [instálala desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista Extensions de Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Puede que tengas que autenticarte con tu cuenta de GitHub para habilitar la extensión.

1. Vuelve al codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del codespace.
3. Escribe un mensaje como `Hello world` en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>. Esto debería activar Copilot Chat.
4. Si aún no te has autenticado, se te pedirá que inicies sesión con tu cuenta de GitHub. Sigue las instrucciones para autenticarte.

   ![Solicitud de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, confirma que aparece la ventana de Copilot Chat.

## Resumen

Has creado una copia del repositorio del laboratorio e iniciado la creación del codespace que usarás para escribir código cuando empiecen los ejercicios.

## Siguiente paso

A continuación, empezarás a trabajar con Copilot. En el [Ejercicio 1: instrucciones personalizadas][next-lesson], le enseñarás las convenciones del proyecto.

## Recursos

- [Información general de GitHub Codespaces][codespaces]
- [Crea un repositorio a partir de una plantilla][template-repository]
- [Guía de inicio de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
