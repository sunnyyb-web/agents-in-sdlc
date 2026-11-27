---
title: "Lección 10: resumen y siguientes pasos"
description: "Repasa el flujo de trabajo con Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo de Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado ya creada;
- añadido valoraciones con estrellas, revisado el resultado en un navegador reenviado y fusionado manualmente tu primera solicitud de incorporación de cambios (PR);
- partido de la incidencia de filtrado, definido el enfoque en el modo Plan, implementado la funcionalidad en Autopilot y revisado el resultado en Interactive;
- guiado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y usado esa habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadido el servidor Model Context Protocol (MCP) de Playwright y usado el servidor para explorar el filtrado en un navegador real;
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas obtenidas en el navegador;
- revisado todo el cambio de filtrado y autorizado Agent Merge para la PR correspondiente;
- explorado comandos slash para gestionar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente en la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizado:

1. **Valoraciones con estrellas:** mostrar el valor existente de `starRating` y un estado explícito para los juegos sin valorar en sus tarjetas.
2. **Filtrado y flujo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y rama. Para simplificar el taller, hemos incluido todo ese trabajo en una sola PR.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, revisándolo personalmente en el navegador y pidiendo a Copilot que explorara el sitio con el navegador mediante MCP. La habilidad `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos, y ha incluido los resultados en el nuevo formato. Antes de abrir la PR, QA reunió esos resultados con la revisión de los requisitos y de la cobertura de pruebas.

Las pruebas nuevas deben cubrir carencias reales; una ejecución de QA que no necesite pruebas nuevas también puede ser correcta. Revisa el código y las pruebas antes de autorizar la fusión, y vuelve a generar las pruebas afectadas después de hacer cambios.

## Procedimientos recomendados

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para cada tarea. Usa **Plan** para definir un enfoque antes de empezar, **Interactive** para mantener el control de cambios concretos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más cosas que puedes explorar

Ya has recorrido el flujo de trabajo principal. Estas son algunas funciones más de la CLI que puedes probar:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar subtareas independientes en paralelo;
- `/worktree` para aislar una tarea distinta;
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo que aprendas con tu equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller de Copilot cloud agent][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
