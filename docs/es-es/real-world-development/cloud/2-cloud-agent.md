---
title: "Ejercicio 2: GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan mucho tiempo en el backlog por falta de tiempo. Copilot cloud agent de GitHub está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a cuestiones de mayor alcance.

Con Copilot cloud agent explorarás:

- cómo personalizar el entorno de generación de código;
- cómo garantizar que las operaciones se realicen de forma segura;
- por qué es importante acotar bien las incidencias;
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: verás que no hay documentación. El primer paso será añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. Quiere mostrar en la página de detalles de cada juego otros títulos de la misma categoría para que los visitantes puedan seguir explorando. No necesitan una implementación pulida todavía; quieren algo que les permita probar la experiencia de usuario. Aunque esta tarea bloquea el trabajo actual, hay otras incidencias de mayor prioridad.

Son tareas que pueden perder prioridad con facilidad y que se pueden asignar a Copilot cloud agent. El agente trabaja en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, puede revisar el trabajo y comprobar que el resultado es correcto.

## Introducción a GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Al igual que con una persona, puedes asignarle trabajo de distintas maneras, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una tarea, Copilot crea una solicitud de incorporación de cambios en borrador para seguir el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue activa o después de que termine. Cuando la solución esté lista para revisarla, Copilot te mencionará en la solicitud.

## La importancia de acotar bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el análisis. Incluso las tareas aparentemente sencillas pueden ser complejas. Por eso, [define cuidadosamente el alcance de las tareas para Copilot cloud agent][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La incorporación de IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya has completado el ejercicio sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo a Copilot cloud agent, inspecciona en modo de solo lectura los archivos de instrucciones que ya incluye este repositorio. Así podrás observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace que ya tengas en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los requisitos de comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes a cloud agent la incidencia `Code lacks documentation` en la sección siguiente, busca en la solicitud de incorporación de cambios los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario. Proceden de estos archivos de instrucciones. Volveremos a señalarlo cuando revisemos las PR en una lección posterior.

## Configurar el entorno de desarrollo de Copilot cloud agent

Crear código suele requerir un entorno específico y algunos scripts de configuración para asegurarse de que todo esté listo, independientemente de quién realice el trabajo. Lo mismo sucede cuando se asignan tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

Cloud agent usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Así, el agente dispone de las herramientas y dependencias de desarrollo necesarias. El taller ya incluye esta configuración para facilitar los ejercicios. Proporciona Node.js, las dependencias del proyecto, los navegadores necesarios para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña «Actions» del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna a tus pasos el nivel mínimo de permisos necesario. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en esos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación y del frontend: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

Es parecido a cualquier otro archivo de flujo de trabajo de GitHub, pero hay algunos detalles importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona correctamente sin esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar desatendida, lo que reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot puede ayudar mucho a crear documentación; por eso, esta es una tarea ideal para asignar a Copilot cloud agent. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio futuro volverás para revisar el resultado.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Nuestra organización requiere que las funciones y los métodos incluyan comentarios TSDoc cuando sean útiles. Por desgracia, las actualizaciones recientes no han seguido este estándar. Tenemos que actualizar el código existente para incluir comentarios de documentación cuando ayuden a aclarar el comportamiento.
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) nueva, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando su entorno —ejecuta el flujo de trabajo que acabamos de ver—, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la PR. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene a pensar y decidir el plan adecuado y crea código.

El proceso tardará varios minutos. Uno de los objetivos principales de Copilot cloud agent es que pueda realizar tareas de forma asíncrona y dejarte tiempo para otras cosas. Aprovecharemos esa función asignándole otra tarea y, mientras tanto, nos ocuparemos de escribir código para añadir funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las ventajas de GitHub Copilot cloud agent es que permite dividir el trabajo: tú puedes centrarte en un conjunto de tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño no debería llevar mucho tiempo, puedes aprovechar ese tiempo para otras tareas. Asignemos el trabajo a Copilot cloud agent.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```markdown
   Queremos ayudar a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y hacer pruebas de aceptación. Estos son los requisitos:

   - Añadir un helper de acceso a datos en `src/lib/` que devuelva otros juegos de la misma categoría que un juego determinado, sin incluir ese juego
   - Mostrar en la página de detalles del juego una sección «Related games» que use el helper
   - Gestionar el caso en que un juego no tenga juegos relacionados
   - Crear pruebas unitarias para el nuevo helper
   - Asegurarse de que todas las pruebas se superan antes de crear la PR
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, aparecerá una serie de 👀 en el primer comentario de la incidencia. Indica que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. Copilot cloud agent funciona de manera parecida a un ingeniero de software, así que no tienes que supervisarlo constantemente; podrás revisar el resultado cuando termine. Mientras tanto, vamos a crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado [GitHub Copilot cloud agent][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Copilot puede resolver deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- cómo personalizar el entorno de generación de código;
- cómo garantizar que las operaciones se realicen de forma segura;
- por qué es importante acotar bien las incidencias;
- cómo asignar incidencias a Copilot.

Mientras Copilot cloud agent trabaja en segundo plano, puedes crear y usar agentes personalizados. [Copilot cloud agent también puede usar servidores MCP][cloud-agent-mcp] y acceder a las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca de Copilot cloud agent][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Procedimientos recomendados para el flujo de trabajo de configuración de Copilot cloud agent][cloud-agent-best-practices]

| [Siguiente lección: agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
