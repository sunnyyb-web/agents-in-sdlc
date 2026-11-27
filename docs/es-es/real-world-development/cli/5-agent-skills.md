---
title: "Lección 5: personaliza y usa la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato del informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Hemos comprobado manualmente que el código funciona y hemos usado archivos de instrucciones para asegurarnos de que cumple nuestros estándares. Pero ¿qué ocurre con las pruebas, el lint y las demás partes de la integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son la mejor opción. Ayudan a Copilot a entender cómo realizar correctamente operaciones como estas.

En esta lección vas a:

- explorar la habilidad existente `quality-checks`;
- personalizar el formato de los resultados;
- volver a cargar la habilidad y ejecutarla.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos auxiliares que el agente carga cuando los necesita. En esencia, cada habilidad es una carpeta con su nombre y un archivo Markdown llamado `SKILL.md`. El archivo Markdown incluye un frontmatter con el nombre y la descripción de la habilidad, una explicación de lo que hace y directrices sobre cuándo usarla. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la habilidad necesite.

> [!NOTE]
> Las carpetas y los archivos adicionales son opcionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque usa los comandos existentes del proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` de un proyecto para que el equipo las comparta y reutilice, o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explorar la habilidad

Exploremos la habilidad que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos: `quality-checks`.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo usar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el proceso de lint.
4. Fíjate en que la habilidad todavía no incluye la sección **Results output formatting**.

## Ejecutar la habilidad antes de cambiarla

Puedes llamar a las habilidades directamente desde Copilot CLI o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en el modo Interactive.
2. Usa este prompt:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Observa el informe final.

## Personalizar el informe

Queremos un informe mejor que indique qué se ha ejecutado, si se ha superado y qué resultados han devuelto las herramientas. Actualiza la habilidad para que genere ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Formato de los resultados

   Al terminar, indica cada comando ejecutado y si se ha superado, ha fallado o ha quedado bloqueado. Incluye el número de pruebas, la duración, los errores, las advertencias y otras métricas solo cuando la herramienta las proporcione. Indica el siguiente paso necesario para cada error o bloqueo. Nunca describas como superada una comprobación omitida o incompleta.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la habilidad actualizada

Ya has hecho el cambio. ¡Veámoslo en acción! Copilot CLI puede volver a cargar las habilidades modificadas sin reiniciar la conversación.

1. Introduce:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente el mismo prompt que antes:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Observa el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una habilidad de agente existente. En esta lección has:

- explorado la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos;
- personalizado el formato de los resultados;
- vuelto a cargar y ejecutado la habilidad.

Este cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
