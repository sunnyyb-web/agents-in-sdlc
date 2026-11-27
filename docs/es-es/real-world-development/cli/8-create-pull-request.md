---
title: "Lección 8: crea y fusiona la PR de la funcionalidad"
description: "Revisa en conjunto el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlo todo y abrir una solicitud de incorporación de cambios. Tú mismo fusionaste la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería dividir la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una sola conversación y rama, y lo incluirás en esta PR.

En esta lección vas a:

- aprender qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- inspeccionar todos los cambios de la funcionalidad y las pruebas de verificación;
- crear la PR de filtrado;
- activar Agent Merge solo después de la revisión y confirmar que la PR se ha fusionado.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo pendiente de la PR sin quitar al desarrollador el control sobre la autorización de la fusión.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una solicitud de incorporación de cambios. Cuando lo activas, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando hace falta. Después, activa la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado tú **Merge pull request**. Agent Merge puede encargarse de ese paso. Antes de activarlo, revisa el trabajo y decide si está listo.

## Usar Agent Merge para gestionar la PR

Ya has creado todo el código. Revisémoslo en conjunto, creemos la PR y dejemos que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo de agentes introduciendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo termine correctamente, realizará la fusión.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, lee el motivo y, cuando el repositorio lo permita, completa manualmente la fusión revisada.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora también el proceso de la solicitud de incorporación de cambios. En esta lección has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación;
- creado la PR de filtrado;
- activado Agent Merge solo después de la revisión y confirmado que la PR se ha fusionado.

A continuación, [explorarás otros comandos slash de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
