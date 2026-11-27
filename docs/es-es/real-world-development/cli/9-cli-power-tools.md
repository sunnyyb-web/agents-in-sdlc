---
title: "Lección 9: comandos slash de GitHub Copilot CLI"
description: "Explora comandos slash para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos slash. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Exploremos otros que pueden ser útiles.

En esta lección vas a:

- usar `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- usar `/model` para explorar los modelos disponibles;
- aprender a exportar o compartir una sesión con `/share`;
- explorar comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Has terminado el flujo de trabajo principal de la CLI. Ahora verás otras capacidades: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar trabajo opcionalmente en [Copilot cloud agent][about-cloud-agent].

## Explorar el contexto de Copilot CLI

Al trabajar en tareas extensas o complejas, puede que alcances la ventana de contexto máxima del modelo. Copilot CLI compacta la conversación automáticamente cuando lo necesita, pero también puedes inspeccionarla o compactarla con comandos slash.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Introduce:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a introducir `/context` y compara el resultado. Si la conversación ya es corta, puede que apenas haya cambiado.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente cuando la ventana se llena. Usa `/compact` si quieres decidir cuándo hacerlo. Usa `/clear` o `/new` cuando cambies a una tarea distinta y quieras empezar una conversación nueva.

## Elegir un modelo

Los distintos modelos tienen puntos fuertes diferentes y cada desarrollador puede tener sus preferencias. Copilot CLI permite ver los modelos disponibles y seleccionar el que quieras usar.

1. Introduce:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Colaborar y compartir lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un vínculo para compartirla o publicar un gist de GitHub.

1. Introduce `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir esta sesión, elige un destino adecuado, como `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviárselo a alguien o publicarlo. Las exportaciones de sesiones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un vínculo o un gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no tenga previsto compartir.

## Opcional: ampliar el trabajo o delegarlo

Has completado el taller principal. Copilot CLI también ofrece comandos para tareas más amplias:

- `/fleet` puede repartir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva y bien acotada, y revisa el resultado con tu flujo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller de agentes en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos slash de Copilot CLI te permiten configurarlo, compartir sesiones y consultar lo que ocurre entre bastidores. En esta lección has:

- usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- usado `/model` para explorar los modelos disponibles;
- aprendido a exportar o compartir una sesión con `/share`;
- explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Copilot CLI ofrece muchos más comandos slash y funciones que puedes explorar. Para terminar el recorrido, [repasarás lo aprendido][next-lesson] y verás cómo seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
