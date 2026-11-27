---
title: "Ejercicio 5: supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: agentes personalizados][previous-lesson] |
|:--|

Cuando usas GitHub Copilot en modo de agente, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu máquina y en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición de archivo a medida que se producen. Puedes revisar cada diff en línea, aceptar o rechazar cambios concretos y guiar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio vas a:

- supervisar en tiempo real el trabajo del agente de accesibilidad;
- revisar y aceptar las ediciones propuestas;
- guiar al agente durante la sesión para añadir un control del modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has usado en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado también un modo claro para las personas a quienes les resulte demasiado oscuro el tema predeterminado. En vez de iniciar una conversación nueva, guiarás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar cada llamada a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que pide ejecutar.
3. Cuando el agente proponga una edición, el diff aparecerá en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta pide permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al final debería mostrar un mensaje que resuma los cambios.

## Guiar al agente para añadir el modo claro

Ahora que ya está listo el modo de alto contraste, ampliarás la misma conversación para añadir un control del modo claro. Como continuarás en la misma sesión, el agente conserva el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. En el campo de Copilot Chat, envía este prompt de seguimiento:

   ```plaintext
   ¡Buen trabajo! Ahora añade un control del modo claro junto al del modo de alto contraste. El modo claro debe seguir el mismo patrón: conservarse mediante el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo de alto contraste y el modo claro de forma independiente.
   ```

2. El agente continuará desde donde lo dejó, explorará los cambios anteriores y propondrá los cambios necesarios para añadir el modo claro.
3. Revisa cada cambio nuevo cuando aparezca, igual que con el modo de alto contraste. Selecciona **Keep** o **Undo** para cada diff.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar la interfaz de los controles de una forma que no te gusta—, detenlo con el botón **Stop** junto al campo de chat y envía otro mensaje para aclarar lo que necesitas.

> [!TIP]
> Guiar una sesión activa es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración actual») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisar el conjunto de trabajo

Antes de confirmar los cambios, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver componentes Astro, estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa los diffs. Confirma que se siguen las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia con almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y guiar una sesión activa: has revisado las ediciones a medida que aparecían, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio has aprendido a:

- supervisar el trabajo de un agente en tiempo real;
- aceptar y rechazar ediciones de archivos individuales;
- guiar una sesión activa con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás los cambios de accesibilidad en una solicitud de incorporación de cambios y revisarás el trabajo del agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: agentes personalizados][previous-lesson] | [Siguiente lección: iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
