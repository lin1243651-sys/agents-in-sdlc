---
title: "Ejercicio 5: Itera sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: supervisar agentes][previous-lesson]

## Revisa el trabajo

Durante este laboratorio has pedido a GitHub Copilot que documente el código, cree una función de juegos relacionados y añada funciones de accesibilidad con modos de alto contraste y claro. Revisarás los cambios y, si hace falta, pedirás ajustes antes de involucrar al resto del equipo.

### Escenario

Los principios de diseño de software y del desarrollo y las operaciones (DevOps) no cambian al añadir IA generativa. Revisa el código generado y sigue el proceso habitual. Examinarás las propuestas de documentación, juegos relacionados y accesibilidad antes de activar la revisión del equipo.

## Seguridad y GitHub Copilot cloud agent

Como cloud agent trabaja de forma asíncrona y sin supervisión, se han establecido restricciones para que el trabajo se realice de forma segura:

- Copilot tiene acceso de lectura al repositorio y acceso de escritura **solo** a la rama que utiliza para el código.
- Cloud agent se ejecuta en GitHub Actions, en un entorno efímero independiente.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- De forma predeterminada, el acceso a recursos externos está limitado, incluidos los servidores Model Context Protocol (MCP). Consulta cómo [configurar el firewall de cloud agent][agent-firewall].

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR), que añade documentación al código.

> [!NOTE]
> Es posible que la PR muestre un aviso de que el firewall bloqueó a GitHub Copilot. Es lo esperado, ya que el acceso a recursos externos, incluidos los servidores MCP, está limitado de forma predeterminada. Si quieres, puedes [configurar o deshabilitar el firewall de cloud agent][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la PR titulada algo parecido a **Add missing documentation** o a un título más descriptivo. Si Copilot sigue trabajando, la PR incluirá la marca **[WIP]** (*work in progress*, «trabajo en curso»); espera a que termine, lo que puede tardar unos minutos.
4. Selecciona **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado, que incluirá nuevos comentarios de documentación para TypeScript (TSDoc) y otra documentación; los cambios concretos pueden variar. Comprueba los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación. Si los hay, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones de la aplicación Astro se superarán; puede tardar varios minutos.
9. Añade un comentario en la PR y menciona a **@copilot**, como harías con otro miembro del equipo. Copilot verá el comentario e iniciará otra sesión para responder. Como los resultados no son deterministas, no hay un prompt único. Puedes pedir, por ejemplo:
   - añadir encabezados breves que expliquen el propósito de cada archivo de código.
   - añadir comentarios TSDoc a los archivos TypeScript y Astro.
   - crear un README que describa la estructura de la aplicación Astro.
10. Selecciona **View Session** para observar cómo Copilot inicia una nueva sesión y aplica los cambios.
11. Selecciona **Back to pull request** para volver a la PR.
12. Cuando Copilot termine, comprueba que hay una nueva confirmación y revisa los cambios en **Files changed**. Puedes seguir iterando. Cuando estés conforme, convierte la PR de borrador en lista para revisión y combínala.

## Revisa la función de juegos relacionados

Vuelve a la solicitud de incorporación de cambios (PR) que Copilot creó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR titulada algo parecido a **Show related games on the game details page** o a un título más descriptivo.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

7. Espera a que se ejecuten las comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones de la aplicación Astro; puede tardar varios minutos.
8. **Opcional:** cambia a la rama de Copilot en tu codespace para probar manualmente la función. Abre un terminal y sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Puedes iterar sobre el resultado y pedir cambios, por ejemplo, ajustar cuántos juegos se muestran o añadir encabezados de comentarios y comentarios TSDoc. Para hacerlo, añade otro comentario en **Conversation**; Copilot lo verá e iniciará otra sesión. Recuerda que esta incidencia se asignó antes de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

La PR del agente personalizado de accesibilidad debería incluir el modo de alto contraste asignado en el ejercicio 3 y el modo claro solicitado en el ejercicio 4.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR titulada algo parecido a **Add high contrast mode to website** o a un título más descriptivo. Si Copilot sigue trabajando, la PR incluirá la marca **[WIP]**; espera a que termine, lo que puede tardar varios minutos.
4. Selecciona **Files changed** y revisa la implementación. Presta atención a:
   - los componentes de interfaz de usuario (UI) para cambiar de modo.
   - el almacenamiento local que conserva las preferencias del usuario.
   - los cambios de las hojas de estilo en cascada (CSS) o de los estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas de aplicaciones de Internet enriquecidas accesibles (Accessible Rich Internet Applications, ARIA) y la navegación con teclado.
   - el código JavaScript o TypeScript que gestiona los cambios de modo.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

7. Espera a que se ejecuten las comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones de la aplicación Astro; puede tardar varios minutos.
8. **Opcional:** cambia a la rama de Copilot en tu codespace para probar manualmente las funciones. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicia la aplicación y comprueba que los modos de alto contraste y claro funcionan y se mantienen después de recargar la página.

Si encuentras problemas de accesibilidad, menciona **@copilot** en un comentario de la PR para pedir una actualización.

## Ejercicio opcional: sigue delegando

Practica asignando a Copilot más incidencias reales del backlog, por ejemplo:

- Crear un formulario de interés de patrocinadores en la página de detalles de un juego.
- Implementar paginación en la página de lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el entorno de cloud agent. En estos ejercicios, has:

- inspeccionado las instrucciones personalizadas del repositorio y observado su efecto en el resultado.
- asignado incidencias a Copilot cloud agent y observado cómo configura el entorno y trabaja de forma asíncrona.
- creado y usado un agente personalizado de accesibilidad para añadir los modos de alto contraste y claro.
- usado la página de agentes de Copilot para supervisar y orientar una sesión en curso.
- revisado e iterado sobre las PR de cloud agent y aprobado flujos de trabajo.

## Revisión y siguientes pasos

Has terminado este entorno. Para explorar escenarios relacionados con otras superficies:

- [Entorno de VS Code][vscode]: explora el modo agente de Copilot Chat y la integración MCP en el entorno de desarrollo integrado (IDE).
- [Entorno de la interfaz de línea de comandos (CLI)][cli]: entrega cambios revisados desde el terminal con Plan, Autopilot, habilidades, un agente de control de calidad (QA), Playwright MCP, Agent Merge y `/delegate`.

En tu propio repositorio, también puedes asignar una incidencia de refactorización o cobertura de pruebas, crear un agente personalizado para otro ámbito o configurar `copilot-setup-steps.yml` para otra pila tecnológica. [awesome-copilot][awesome-copilot] ofrece más archivos de instrucciones, agentes personalizados y habilidades.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de Copilot agents][copilot-agents]
- [Asigna incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración de cloud agent][cloud-agent-best-practices]
- [Configura el firewall de Copilot cloud agent][agent-firewall]

[previous-lesson]: ../4-managing-agents/
[vscode]: ../../vscode/
[cli]: ../../cli/
[awesome-copilot]: https://github.com/github/awesome-copilot
[github-copilot]: https://github.com/features/copilot
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
