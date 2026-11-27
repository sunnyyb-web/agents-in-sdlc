---
title: "Ejercicio 2: añadir una funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas adicionales, como ejecutar pruebas. El flujo de trabajo habitual consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, volver a cambiarlo... Y la lista continúa.

Para eso sirve el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, después realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usar el modo de agente para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE, con ciclos rápidos de comentarios e interacción. Cloud agent está diseñado para actuar como un compañero de programación que trabaja de forma asíncrona con el equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- usar el modo de agente de Copilot para explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- implementar funcionalidades en la interfaz y la capa de datos con el modo de agente de GitHub Copilot;
- revisar los cambios y las pruebas generados por el modo de agente antes de fusionarlos en el código.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya has añadido un helper de editoriales. Ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de programación. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan una base de datos SQLite local directamente desde su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los helpers reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con scripts pequeños y acotados de Astro que usan las API estándar del DOM.

### Iniciar el sitio web

Para facilitar la ejecución del sitio, se ha configurado la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarlo en tu GitHub Codespace así:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio con este comando:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador con `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar un enlace a la URL localhost en el terminal se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando el sitio esté en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre su página de detalles con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía según el ejercicio; cada paso lo indicará cuando sea pertinente.

La implementación inicial del sitio funciona, pero queremos añadir capacidades. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generaron automáticamente varias incidencias. Pide a GitHub Copilot que las muestre.

1. Selecciona **Agent** en el menú de agentes de la vista Chat. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo frente a otro. Al crear este taller usamos Claude Sonnet 4.5, por lo que lo indicamos para que las sugerencias de código sean relativamente coherentes y la experiencia sea más uniforme. Sin embargo, como los modelos de lenguaje son probabilísticos, es normal que las sugerencias difieran de las indicadas en el taller.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje, Copilot puede usar un comando MCP distinto y aun así completar la tarea.

3. Pide a Copilot que muestre las incidencias del backlog con este prompt:

   ```plaintext
   Muéstrame el backlog de incidencias de mi repositorio de GitHub. Ayúdame a priorizarlas según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según lo que considera más útil para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que usarás para proporcionar contexto a Copilot. Aprovecharás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que incluye contexto sobre cómo añadir funcionalidad al sitio.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidad, que incluyen:
   - una descripción de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones concretos para varios tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir enlaces a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto facilita la reutilización, la división de instrucciones complejas en partes manejables y el uso de ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, la aplicación necesita al menos tres cambios independientes:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y confirmar que se superan antes de fusionar los cambios. El modo de agente de Copilot puede encargarse de estas tareas.

1. Puedes continuar la conversación actual con Copilot o empezar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog con este prompt:

   ```plaintext
   Actualiza el sitio para incluir el filtrado por editorial y categoría según los requisitos de la incidencia de GitHub correspondiente del backlog. Ya existe un helper de editoriales del ejercicio anterior: consérvalo y mejóralo si hace falta al completar la capa de datos, la interfaz y las pruebas restantes. Asegúrate de que todas las pruebas se superan antes de terminar. El servidor ya está en ejecución, así que no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Deberías ver que encuentra los helpers de acceso a datos, la interfaz y las pruebas. Después modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, todos los cambios tendrán una opción **Keep** o **Undo**; no hace falta seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo en Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Captura de una sesión de Copilot Agent Mode completada en Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y comprueba si presenta algún problema.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir capacidades al sitio de Tailspin Toys. Has aprendido:

- cómo implementar funcionalidades en la interfaz y la capa de datos con el modo de agente de GitHub Copilot;
- cómo explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- cómo revisar los cambios y las pruebas generados antes de fusionarlos en el código.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar paginación

A medida que crezca la lista de juegos, necesitarás paginarla. Usa las habilidades de este ejercicio y pide a Copilot que actualice el sitio para implementar la paginación. Ten en cuenta lo siguiente:

- sigue los procedimientos recomendados existentes y usa los archivos de instrucciones;
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo;
- proporciona a Copilot las directrices necesarias para crear la implementación que quieres;
- puede que tengas que iterar, pedir cambios y aportar contexto. Es el flujo normal cuando se trabaja con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
