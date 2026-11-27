---
title: "Lección 4: guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot disponga de esas indicaciones. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha usado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección vas a:

- explorar cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas;
- actualizar el archivo de instrucciones para garantizar que se siguen los estándares de código;
- observar el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos se incluyen:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no limitarse a repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, sus parámetros y sus valores devueltos mediante TSDoc/JSDoc. Si existe un argumento `db` inyectable, también debe documentarse.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Hay que conservar las directrices de formato y lint existentes.

Con los archivos de instrucciones te asegurarás de que Copilot tenga la información necesaria para realizar las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas te permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función puede ayudarte a obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar tus convenciones de código, bibliotecas e incluso los tipos de comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y aportar así contexto específico para una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información a nivel de proyecto pertinente para la mayoría de las solicitudes.
- Los archivos `.github/instructions/*.instructions.md`, que proporcionan directrices para determinados lenguajes, tipos de archivo o tareas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones personalizadas de este proyecto

Para empezar, el proyecto de ejemplo ya incluye varios archivos de instrucciones. Explora lo que contienen antes de hacer cambios para poder observar sus efectos.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y en las directrices de código. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Verás instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros ámbitos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos extensos de instrucciones en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que debe seguir.

## Actualizar las instrucciones para reflejar las directrices del equipo

Los archivos actuales son un buen comienzo, pero falta una directriz. Modifica el archivo principal `copilot-instructions.md` para que se añadan comentarios TSDoc al código TypeScript nuevo.

1. En `.github/copilot-instructions.md`, busca la sección titulada **Code formatting guidance**, que debería estar alrededor de la línea 35.
2. Añade este elemento al final de la lista:

   ```markdown
   - Todo el TypeScript nuevo debe incluir comentarios TSDoc con fines de documentación.
   ```

El archivo se guardará automáticamente.

## Usar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones nuevas, sal de la conversación de filtrado y retómala para que Copilot aplique las directrices actualizadas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, introduce `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio y en la rama `game-filters-cli`, retoma la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones actualizadas:

   ```plaintext
   Acabamos de actualizar nuestras instrucciones y directrices de código. ¿Puedes actualizar el código que has generado para que las cumpla?
   ```

4. Introduce `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc generados y confirma que describen el código con precisión.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI incorpora el contexto de las instrucciones y has aplicado un estándar nuevo a tu funcionalidad. En concreto, has:

- explorado cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas;
- actualizado el archivo de instrucciones para garantizar que se siguen los estándares de código;
- observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable de comprobaciones de calidad][next-lesson] para ejecutar lint y pruebas de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Procedimientos recomendados para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
