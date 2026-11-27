---
title: "Ejercicio 1: instrucciones personalizadas (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: requisitos previos][previous-lesson] · [Siguiente lección: Copilot cloud agent →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de fondo, debes asegurarte de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esas directrices para que Copilot entienda no solo *qué* debe hacer, sino también *cómo* quieres que lo haga.

En este ejercicio vas a:

- explorar cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos de rutas;
- añadir un estándar para todo el repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no compararás prompts antes y después. Copilot cloud agent trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, cuando revises las solicitudes de incorporación de cambios que genere.

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

## Explorar los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones de este repositorio: hay un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora los archivos. Verás instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un glob relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. Por ejemplo, coincide con cualquier archivo de pruebas TypeScript como los que se ajusten a `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos extensos de instrucciones en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que debe seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de comentario a nivel de archivo.

## Añadir un estándar para el repositorio

Este es el único cambio que harás en este ejercicio: añadir una regla para que la documentación esté en el código mediante comentarios TSDoc y un comentario al principio de cada archivo. Copilot cloud agent aplicará esta regla a las incidencias que le asignes más adelante.

Antes de editar, crea una rama. Aunque el agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, confirmar los cambios por separado es una buena práctica:

1. Desde el terminal del codespace, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como ya hemos indicado, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añadamos los estándares del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Explica los estándares del proyecto, pero aún no incluye reglas para documentar el código; por eso el helper generado no contenía comentarios.
3. Añade estas líneas de Markdown justo después de los estándares existentes. Así indicarás a Copilot que añada encabezados de comentario y comentarios TSDoc:

   ```markdown
   - Todas las funciones exportadas deben incluir un comentario TSDoc que describa su propósito, sus parámetros y su valor de retorno.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares globales, como la regla de comentarios de documentación que acabas de añadir.

## Confirmar, subir y fusionar la actualización de instrucciones

Copilot cloud agent lee los archivos de instrucciones de la rama de destino de la incidencia. En el siguiente ejercicio, cuando asignes una incidencia a Copilot, creará una rama desde `main`. Por tanto, tus instrucciones deben llegar a `main` para que el agente pueda usarlas.

1. Prepara los cambios y crea una confirmación:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Sube la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y fusiónala. Copilot cloud agent leerá estas instrucciones cuando trabaje en las incidencias del siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y crear la confirmación directamente en `main`. Incluimos el paso de la rama para reflejar cómo se haría en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has añadido un estándar para todo el repositorio a `.github/copilot-instructions.md`. Verás cómo se aplica este estándar en las solicitudes de incorporación de cambios que genere el agente durante el resto de este entorno.

A continuación, [asignarás tu primera incidencia a Copilot cloud agent][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Procedimientos recomendados para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
