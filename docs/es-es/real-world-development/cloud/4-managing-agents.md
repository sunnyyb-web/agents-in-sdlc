---
title: "Ejercicio 4: supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, pediste a Copilot cloud agent que se encargara de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde Mission Control puedes consultar todas las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas. También puedes explorar las operaciones que han realizado e incluso guiar una sesión activa.

En esta lección vas a:

- explorar la página de agentes para supervisar las tareas del agente en la nube;
- guiar una sesión activa para solicitar otra funcionalidad.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se ha dado cuenta de que también convendría añadir un modo claro. Como ya se estaba trabajando en actualizar el estilo del sitio y añadir controles para cambiarlo, parece lógico incluir esta funcionalidad. Guiarás el trabajo del agente para que añada tanto el modo claro como el alto contraste.

## Revisar las tareas de Copilot cloud agent

Veamos el estado actual de las tareas asignadas a Copilot cloud agent.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en ejecución. Revisa las operaciones realizadas por Copilot, que pueden incluir:
   - obtener el código del repositorio;
   - crear el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar los distintos pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya adoptado.

4. Observa también el panel de solicitudes de incorporación de cambios (PR), que aparece a la derecha. Desde allí puedes ver la PR y los archivos modificados para supervisar el trabajo.

## Guiar al agente en la nube

Ahora que has visto las tareas activas, pídele a Copilot que añada el control de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que le haya dado Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Supervisa la sesión durante unos minutos hasta que termine de configurar el entorno y empiece a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   Mientras trabajamos en el modo de alto contraste, añadamos también un modo claro. Debe haber un control para este modo, de forma que cada persona pueda elegir cómo quiere ver el sitio.
   ```

   ![Captura de la tarea de cloud agent en la página de agentes, con el cuadro de diálogo para guiar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora a su flujo de trabajo.

## Dejar que Copilot trabaje

Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba realizando al recibir el mensaje.

Como antes, el proceso tardará varios minutos. Aprovecha este tiempo para repasar todo lo que has aprendido y explorado.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y guiar las tareas de Copilot cloud agent. Desde Mission Control puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para cambiar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- usar Mission Control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión activa para solicitar una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: agentes personalizados][previous-lesson] | [Siguiente lección: iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
