---
title: "Ejercicio 4: agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para determinadas tareas o áreas del flujo de desarrollo. Al definirlos mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones específicas, procedimientos recomendados, patrones de código y conocimientos del dominio para que Copilot trabaje con mayor eficacia. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que incluye al menos `name` y `description`, seguido de un prompt en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se parecen en algunos aspectos. Ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La forma más sencilla de distinguirlos es pensar que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes para realizar el trabajo. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad. Como parte de su trabajo, podría usar habilidades como un flujo de trabajo de solicitudes de incorporación de cambios o una habilidad para ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única forma «correcta» de crear un agente personalizado. Como con cualquier tecnología de IA, prueba distintas opciones e itera para encontrar lo que funciona en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que algunas personas tienen dificultades para leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, por lo que quieres implementar esta mejora cuanto antes. Para generar la funcionalidad, usarás un agente personalizado.

En este ejercicio vas a:

- revisar un agente personalizado de accesibilidad existente;
- usar el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

Vuelve a tu codespace y abre un terminal. Crea una rama nueva a partir de `main` para el trabajo de accesibilidad, de modo que la PR de filtrado del ejercicio 3 siga separada:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Lee las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - los procedimientos recomendados de accesibilidad;
   - ejemplos de código HTML, CSS y JavaScript;
   - errores y problemas habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el menú de agentes de la parte inferior de Copilot Chat. Puedes seleccionar un agente personalizado para limitar una sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía según el ejercicio; cada paso lo indicará cuando sea pertinente.

1. Si todavía no está seleccionado, elige **Agent** en el menú de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú de agentes de la parte inferior de la vista Chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes.
4. Envía este prompt al agente de accesibilidad:

   ```plaintext
   Añade un modo de alto contraste al sitio. Debe haber un control que permita al usuario activarlo y desactivarlo, y la configuración debe conservarse al volver a cargar la página mediante el almacenamiento local del navegador.
   ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff que puedes revisar.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos para que puedas ver el conjunto de trabajo.

> [!NOTE]
> Es probable que el proceso tarde unos minutos. Copilot está haciendo cambios reales en el repositorio: editará componentes Astro, CSS y las pruebas relacionadas, si las hay.

En el ejercicio siguiente revisarás y guiarás el trabajo mientras está en curso.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas y áreas concretas. Con ellos puedes convertir la experiencia y los estándares del equipo en agentes reutilizables que guíen a Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios a medida que se hagan y añadirás un control para el modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
