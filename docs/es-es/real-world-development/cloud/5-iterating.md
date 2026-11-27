---
title: "Ejercicio 5: iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que añadiera documentación al código, desarrollara una función de juegos relacionados para que el equipo de diseño pudiera probarla e implementara mejoras de accesibilidad, incluidos controles para activar los modos de alto contraste y claro. Revisemos los cambios y, si hace falta, demos indicaciones a Copilot para mejorar el resultado.

### Escenario

Como ya hemos señalado, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisemos las sugerencias de GitHub Copilot para la documentación, la función de juegos relacionados y las mejoras de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y GitHub Copilot cloud agent

Como Copilot cloud agent realiza tareas de forma asíncrona y sin supervisión, se han establecido restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para su código.
- Cloud agent se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que se ejecute.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) creada por GitHub Copilot cloud agent: añadir documentación al código. Usarás la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al revisar la PR, puede que aparezca una advertencia que indique que el firewall ha bloqueado a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall de Copilot cloud agent][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar varios minutos; puedes descansar un momento o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

   Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que has revisado al principio del ejercicio 2.

6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar varios minutos.

## Pedir cambios a GitHub Copilot

Trabajar con Copilot en una solicitud no es un proceso unidireccional. Puedes mencionar a Copilot en los comentarios de la PR, como harías con otra persona del equipo, o añadir comentarios en línea al código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos recomendarte un texto concreto. Puedes pedirle, por ejemplo:

- que añada encabezados de comentario al principio de cada archivo de código con una breve descripción de su propósito;
- que añada comentarios TSDoc a los archivos TypeScript y Astro;
- que cree un README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores o sugiere otro cambio de documentación que quieras ver en el código.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer una confirmación nueva en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, convierte la PR de borrador en una solicitud lista para revisión y fusiónala en la rama principal.

![Convertir la PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

Volvamos a la PR que Copilot ha creado para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar varios minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Ve al Codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama que ha creado Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Puedes seguir pidiendo cambios a Copilot cloud agent. Por ejemplo, puedes solicitar que ajuste el número de juegos que muestra o que añada encabezados de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de que actualizaras las instrucciones personalizadas. Para pedir cambios, añade otro comentario en la pestaña **Conversation**. Copilot lo verá e iniciará otra sesión.

## Revisar las mejoras de accesibilidad

Por último, revisemos las mejoras de accesibilidad implementadas por el agente personalizado. La PR debería incluir el modo de alto contraste que has asignado en el ejercicio 3 y el modo claro que has solicitado desde Mission Control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar varios minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para alternar entre modos;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios de CSS o estilos para los modos de alto contraste y claro;
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado;
   - el código JavaScript/TypeScript que gestiona los cambios de modo.

6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar varios minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las mejoras de accesibilidad. Ve al Codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama que ha creado Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Confirma que funcionan como esperas y que las preferencias se conservan al volver a cargar la página.

El agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo los procedimientos recomendados de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y pedirle que haga cambios, como con las PR anteriores.

## Ejercicio opcional: seguir delegando

Cloud agent resulta especialmente útil cuando puedes asignarle incidencias reales del backlog y ocuparte de otras cosas. Para adquirir esa costumbre, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés para patrocinadores en la página de detalles del juego;
- implementar la paginación en la lista de juegos;
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el entorno de cloud agent. A lo largo de estas lecciones has:

- **inspeccionado los archivos de instrucciones personalizadas del repositorio** para observar más adelante su efecto en el resultado del agente;
- **asignado incidencias a Copilot cloud agent** y observado cómo configura el entorno, planifica y trabaja de forma asíncrona;
- **creado y usado un agente personalizado** para accesibilidad, con controles de modo de alto contraste y claro;
- **usado la página de agentes de Copilot como Mission Control** para supervisar y guiar la sesión de accesibilidad;
- **revisado e iterado sobre las solicitudes de cloud agent**, mencionando a `@copilot` para pedir cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el entorno de cloud agent. Si quieres seguir explorando, los otros entornos complementan lo que has practicado:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` hacia cloud agent.

En tu propio repositorio, prueba estas ideas:

- asignar a cloud agent una incidencia de refactorización o cobertura de pruebas;
- crear un agente personalizado para otro ámbito;
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Procedimientos recomendados para el flujo de trabajo de configuración de Copilot cloud agent][cloud-agent-best-practices]
- [Configurar el firewall de Copilot cloud agent][agent-firewall]

---

| [← Lección anterior: gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
