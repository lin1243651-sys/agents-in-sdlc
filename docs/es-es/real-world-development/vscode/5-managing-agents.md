---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: agentes personalizados][previous-lesson]

En modo agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo ocurre en tu equipo y en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición a medida que se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con indicaciones de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio, vas a:

- supervisar en tiempo real el trabajo del agente de accesibilidad.
- revisar y aceptar las ediciones propuestas.
- orientar la sesión para añadir un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita un modo claro para quienes encuentran demasiado oscuro el tema predeterminado. En vez de iniciar una conversación nueva, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea, con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio anterior.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos del terminal.
3. Cuando el agente proponga una edición, revisa la diferencia que aparece en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. Abre el indicador **Files changed** en la parte superior del chat para revisar todos los archivos modificados.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso —por ejemplo, para ejecutar un comando del terminal—, el agente se detendrá y mostrará **Allow**. Aprueba solo los comandos que te parezcan seguros.

5. Espera a que termine la implementación del modo de alto contraste y lee el resumen final.

## Orienta al agente para añadir un modo claro

Una vez implementado el modo de alto contraste, amplía la misma conversación para añadir el modo claro. Al continuar en la misma sesión, el agente conserva el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. Envía este prompt en Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente continuará desde el punto en que se quedó, volverá a explorar los cambios anteriores y propondrá los cambios necesarios para el modo claro.
3. Revisa cada edición nueva a medida que aparezca, igual que con el modo de alto contraste, y selecciona **Keep** o **Undo**.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta reorganizar los controles de una forma que no te convence—, selecciona **Stop** junto al campo de chat y envía una indicación de seguimiento para aclarar lo que necesitas.

> [!TIP]
> Orientar una sesión activa es más rápido que empezar de nuevo. Usa indicaciones de seguimiento para pedir ajustes, corregir el rumbo o ampliar el alcance; por ejemplo:
>
> - «Simplifica las hojas de estilo en cascada (CSS): usa variables CSS en lugar de duplicar selectores».
> - «No añadas un tercer control; integra el modo claro en el panel de configuración existente».
> - «Añade también atajos de teclado para ambos controles».

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa los archivos modificados.

1. Abre **Source Control** en VS Code.
2. Revisa todos los archivos. Deberías encontrar cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre un par de archivos modificados y revisa las diferencias. Comprueba que se aplican las pautas de accesibilidad del agente personalizado: atributos de aplicaciones de Internet enriquecidas accesibles (Accessible Rich Internet Applications, ARIA), navegación con teclado, estructura semántica del lenguaje de marcado de hipertexto (HTML) y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios en el panel Source Control. Los publicarás en la lección siguiente.

## Resumen y siguientes pasos

Has usado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición al aparecer, aceptado o rechazado cambios y enviado indicaciones de seguimiento para ampliar el trabajo sin perder el contexto. En esta lección, has:

- supervisado en tiempo real el trabajo del agente.
- aceptado y rechazado ediciones de archivos individuales.
- orientado una sesión en curso con indicaciones de seguimiento.

En la [lección siguiente][next-lesson], publicarás el trabajo de accesibilidad como solicitud de incorporación de cambios y revisarás los cambios que el agente personalizado local ha generado durante este laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
