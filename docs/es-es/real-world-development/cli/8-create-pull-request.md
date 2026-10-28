---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones actualizadas, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora los revisarás juntos y abrirás una solicitud de incorporación de cambios (PR). La PR de las calificaciones con estrellas la combinaste manualmente; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> En un proyecto real, normalmente dividirías la funcionalidad, las instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, todo el trabajo de filtrado y calidad se ha mantenido en una conversación y una rama.

En esta lección, vas a:

- aprender qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionar el cambio completo de la funcionalidad y las pruebas de verificación.
- crear la PR del filtrado.
- activar Agent Merge solo después de revisar el trabajo y confirmar que la PR se ha combinado.

## Escenario

Has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener el control de la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Cuando lo activas, Copilot resuelve los bloqueos de la PR, como comprobaciones de integración continua (CI) fallidas, comentarios de revisión y necesidad de reorganizar la rama. Después, activa la combinación automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de ese proceso, pero revisa el trabajo y confirma que está listo antes de activarlo.

## Usa Agent Merge para gestionar la PR

Revisa el código, crea la PR y permite que Agent Merge gestione el resto.

1. Vuelve al codespace.
2. Escribe `/agent` para abrir el cuadro de diálogo de agentes.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea una PR con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR y la combinará cuando se superen todas las comprobaciones.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo y, cuando el repositorio lo permita, realiza manualmente la combinación revisada.

## Resumen y siguientes pasos

Has automatizado partes del proceso de desarrollo: generación de código, pruebas y verificación, y ahora la gestión de la PR. En esta lección, has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionado el cambio completo de la funcionalidad y las pruebas de verificación.
- creado la PR del filtrado.
- activado Agent Merge después de revisar el trabajo y confirmado la combinación.

A continuación, explorarás otros [comandos de barra de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestiona solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
