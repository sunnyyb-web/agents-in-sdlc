---
title: "Ejercicio 3: probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: modo de agente][previous-lesson] · [Siguiente lección: agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, confirma que funciona en el navegador. En vez de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** y dejarás que Copilot controle un navegador real. Después publicarás la rama y abrirás la PR.

En este ejercicio vas a:

- aprender qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot;
- añadir el servidor MCP de Playwright al espacio de trabajo;
- pedir a Copilot que lo use para probar manualmente la funcionalidad en un navegador;
- publicar la rama y abrir una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos al editor. Model Context Protocol (MCP) permite que Copilot lo haga: es un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente y gestiona la comunicación con los servicios externos, como las API existentes o herramientas locales como los paquetes de NPM. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP populares son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a varias API para gestionar repositorios de GitHub. Permite al agente crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona funciones de automatización del navegador con Playwright. Permite al agente abrir páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles, cada uno con acceso a herramientas y recursos diferentes. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa cuidadosamente su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Usa solo servidores MCP en los que confíes y ten cuidado al conceder acceso a recursos o acciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Deberías ver que ya hay un servidor `github` configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La plantilla del proyecto ya incluye la entrada `github`, por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hace falta instalar nada localmente: VS Code se conecta por HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añadir el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

A diferencia del servidor `github` remoto, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin una ventana visible. Es necesario en un codespace, donde no hay un escritorio que pueda mostrarla.

> [!NOTE]
> Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, así que normalmente el navegador que necesita Playwright ya está instalado. Si Copilot informa más adelante de que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Iniciar el servidor y confiar en él

VS Code inicia los servidores MCP **cuando se necesitan**. La primera vez que Copilot usa las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pide que confirmes que confías en el servidor, revisa la configuración y selecciona la opción para confiar en él.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code iniciará el servidor y te pedirá que confirmes que confías en él la primera vez que Copilot necesite sus herramientas.

## Probar la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En vez de probar la aplicación tú mismo, el agente puede abrir una página, navegar por ella, aplicar filtros y leer los resultados. Después, te resumirá lo que ha observado. Es una forma rápida de confirmar que la funcionalidad funciona sin salir de la conversación.

El servidor MCP de Playwright usa el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. Por eso, el agente analiza elementos estructurados y etiquetados —como botones, enlaces y elementos de lista— de una forma parecida a las tecnologías de asistencia. Así, una comprobación funcional rápida también sirve como una revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la funcionalidad de filtrado:

```text
Usa el servidor MCP de Playwright para abrir un navegador con la aplicación en ejecución en http://localhost:4321 y verificar la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos e indica cuántos juegos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra los juegos de esa categoría.
3. Quita el filtro y aplica uno de editorial. Confirma que la lista solo muestra los juegos de esa editorial.
4. Combina un filtro de categoría y uno de editorial. Confirma que los resultados cumplen ambos criterios.

Describe lo que observas en cada paso e indica cualquier comportamiento inesperado.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará lo que ha observado. Compara su resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pídele que corrija el código antes de abrir una solicitud de incorporación de cambios.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot usa el servidor MCP de Playwright puede necesitar descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de incorporación de cambios

Has confirmado que la funcionalidad funciona y ya puedes abrir una solicitud de incorporación de cambios (PR) para que el equipo la revise. Primero publicarás la rama `filtering-vscode`.

1. Ve al panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para subir la rama al repositorio.

## Crear la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud, por ejemplo, desde github.com o mediante GitHub CLI. Como ya estás trabajando con GitHub Copilot, deja que la cree por ti. Puede buscar la incidencia pertinente y asociarla a la PR.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que está seleccionado **Agent** en el menú de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editorial. Crea una solicitud de incorporación de cambios para la rama actual y asóciala a la incidencia correcta.
   ```

4. Cuando se te solicite, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones necesarias. La primera vez que use una herramienta de GitHub, puede pedirte que te autentiques; sigue las instrucciones.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el vínculo que genera Copilot para revisar la solicitud, pero **todavía no la fusiones**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has:

- aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot;
- añadido el servidor MCP de Playwright al espacio de trabajo;
- usado el servidor para probar manualmente la funcionalidad de filtrado antes de publicarla;
- publicado la rama y abierto una solicitud de incorporación de cambios para el trabajo de filtrado.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
