---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya combinaste personalmente la solicitud de incorporación de cambios (PR) de valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección vas a:

- aprender qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- examinar la PR completa de la funcionalidad y las pruebas de verificación;
- autorizar Agent Merge solo después de la revisión y confirmar que la PR se ha combinado.

## Escenario

A lo largo del flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere ahora automatizar el trabajo restante de la PR, pero mantener la autorización para combinar bajo el control del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de cambios en la aplicación GitHub Copilot. Al habilitarlo, la sesión de la aplicación lee la PR y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y reorganiza la base cuando es necesario. La combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando aunque se reinicie la aplicación y se desactiva cuando se combina la PR.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede asumir esa responsabilidad, pero su capacidad para editar código y combinar todavía requiere tu autorización explícita. Revisa las acciones que tiene permitidas y el trabajo realizado antes de concederle permiso para combinar.

## Usar Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Ahora vamos a dejar que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se iniciará el proceso de Agent Merge. Hará lo siguiente:

- crear la PR con un título y una descripción;
- si iniciaste la sesión desde una incidencia, incluir una referencia a la incidencia relacionada en el cuerpo de la descripción;
- reorganizar la base o gestionar posibles conflictos con la rama de destino;
- supervisar el proceso de CI para comprobar que se superen todas las verificaciones;
- supervisar la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot y realizar cambios para resolverlos;
- combinar automáticamente la PR cuando todo se haya completado correctamente, si así se lo permites.

Vamos a permitir que Agent Merge también combine la PR cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** está marcado.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Combina la PR cuando se hayan superado todas las comprobaciones y aprobaciones necesarias. No eludas las protecciones del repositorio.
    ```

9. Sigue el progreso y resuelve cualquier bloqueo que se comunique. Antes de continuar, confirma que el estado de la PR es **Merged**; que el permiso para combinar esté habilitado o que haya un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora también el proceso de la PR. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- has examinado la PR completa de la funcionalidad y las pruebas de verificación;
- has autorizado Agent Merge solo después de la revisión y confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
