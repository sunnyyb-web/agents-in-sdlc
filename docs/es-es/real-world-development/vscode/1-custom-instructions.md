---
title: "Ejercicio 1: instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: requisitos previos][previous-lesson] · [Siguiente lección: modo de agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de fondo, debes asegurarte de que pueda acceder a ese contexto. Hay varias formas de proporcionárselo. Una de las principales son los [archivos de instrucciones][instruction-files], que permiten orientar la generación de código.

En este ejercicio vas a:

- explorar cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos de rutas;
- generar el primer conjunto de datos para el filtrado (un helper de editoriales) con las instrucciones *actuales*;
- añadir un estándar para todo el repositorio a `.github/copilot-instructions.md`;
- volver a enviar el mismo prompt y observar cómo el código generado sigue el nuevo estándar;
- confirmar en `main` los cambios de las instrucciones y la base del filtrado para que Copilot pueda usar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que hayas definido. Copilot no es determinista. El objetivo del ejercicio es observar la *tendencia* en el comportamiento después de actualizar las instrucciones, no conseguir una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos se incluyen:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe estar en modo oscuro y tener un aspecto moderno.
- Debe añadirse documentación al código en forma de comentarios TSDoc.
- Debe añadirse un bloque de comentarios al inicio de cada archivo para describir su propósito.

Con los archivos de instrucciones te asegurarás de que Copilot tenga la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas te permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función puede ayudarte a obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar tus convenciones de código, bibliotecas e incluso los tipos de comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y aportar así contexto específico para una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información a nivel de proyecto pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general de lo que se desarrolla, los procedimientos recomendados y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md`, que puedes crear para tareas o tipos de archivo concretos. Sirven para proporcionar directrices sobre lenguajes como TypeScript o Astro, o sobre tareas como crear un componente de interfaz o un conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las finalizaciones de código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite archivos de instrucciones de otros estándares][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Procedimientos recomendados para gestionar archivos de instrucciones

En este taller no podemos explicar en detalle cómo crear archivos de instrucciones. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Mantén las instrucciones de `copilot-instructions.md` centradas en directrices del proyecto, como su descripción, estructura y estándares globales de código.
- Usa archivos `*.instructions.md` para proporcionar directrices específicas para tipos de archivo (pruebas unitarias, componentes de Astro o capa de datos) o tareas concretas.
- Escribe en lenguaje natural y con claridad. Incluye ejemplos de cómo debería ser el código y de cómo no debería ser.

No existe una única forma de crear archivos de instrucciones, igual que no hay una única forma de usar la IA. La experimentación te ayudará a descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían contar con una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, puede que veas archivos para muchos tipos de tareas, incluidas las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). El taller correspondiente lo indicará cuando sea pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora los archivos. Verás instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un glob relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. Por ejemplo, coincide con cualquier archivo de pruebas TypeScript como los que se ajusten a `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos extensos de instrucciones en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que debe seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de comentario a nivel de archivo.

## Crear una rama para los cambios

Sigamos algunos procedimientos recomendados y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía según el ejercicio; cada paso lo indicará cuando sea pertinente.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas de editor abiertas de ejercicios anteriores para que Copilot solo tenga el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ir el helper.
3. Selecciona **Agent** en el menú de agentes de la vista Chat para que Copilot pueda aplicar cambios a los archivos.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva la lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios a los archivos.
   ```

5. Copilot explora el proyecto y aplica cambios al código, normalmente en el archivo del helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Sigue las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplican a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada: el objetivo sigue siendo observar la mejora de coherencia después de actualizar las instrucciones.

## Añadir un estándar para el repositorio

Como hemos visto, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añadamos los estándares de código del repositorio para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Documenta los estándares del proyecto, pero todavía no incluye reglas para documentar el código. Por eso el helper generado no contenía comentarios.
3. Añade estas líneas de Markdown debajo de los estándares existentes. Así indicarás a Copilot que añada encabezados de comentario y comentarios TSDoc:

   ```markdown
   - Todas las funciones exportadas deben incluir un comentario TSDoc que describa su propósito, sus parámetros y su valor de retorno.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares globales, como la regla de comentarios de documentación que acabas de añadir.

## Volver a enviar el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **el mismo prompt** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva la lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios a los archivos.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales de la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

   Copilot también añadirá comentarios TSDoc a las funciones exportadas.
5. Revisa los cambios y confirma que los comentarios describen el código con precisión.

## Crear y fusionar la solicitud de incorporación de cambios

Ahora que has actualizado las instrucciones y la base del helper de editoriales, confirma y sube los cambios para que el siguiente ejercicio pueda usarlos.

1. Abre la vista **Source Control** de VS Code.
2. Revisa los cambios y prepara los archivos para confirmarlos.
3. Introduce un mensaje de confirmación adecuado y crea la confirmación.
4. Publica la rama `custom-instructions` en el repositorio.

Con la rama publicada, crea una solicitud de incorporación de cambios y vincúlala a la incidencia del estándar de documentación del backlog. Copilot puede hacerlo con las herramientas de GitHub ya conectadas al proyecto. Pídele que encuentre la incidencia, cree una PR que la cierre y, después, fusiónala.

> [!NOTE]
> En este ejercicio Copilot actúa por primera vez en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub con el espacio de trabajo, así que no hay que configurar nada. La primera vez que Copilot use una herramienta, VS Code puede pedirte que inicies sesión en GitHub. Sigue las indicaciones. En un ejercicio posterior aprenderás cómo funciona esta conexión, basada en Model Context Protocol.

1. Abre Copilot Chat en tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual. Indícale que describa tanto la actualización de las instrucciones como la base del helper de editoriales:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una solicitud de incorporación de cambios desde la rama actual, indica que cierra esa incidencia e incluye que también hemos añadido la base del helper de editoriales para el próximo trabajo de filtrado.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se cree la PR, pídele a Copilot que la fusione y vuelva a `main`:

   ```plaintext
   Fusiona la PR en main. Después, vuelve localmente a main y descarga el código más reciente para que todo quede actualizado.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell en tu nombre, revisa el comando y selecciona **Allow** si corresponde.

¡Ya has creado y fusionado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *actuales*;
- añadir un estándar para todo el repositorio a `.github/copilot-instructions.md`;
- volver a enviar el mismo prompt y observar cómo el código propuesto sigue el nuevo estándar;
- confirmar en `main` la actualización de las instrucciones y la base del helper para que el siguiente ejercicio pueda aprovecharlas.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad nueva en varios archivos.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Procedimientos recomendados para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
