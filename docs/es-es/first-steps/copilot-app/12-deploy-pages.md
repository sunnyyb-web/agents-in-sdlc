---
title: "Lección 12: publica el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz con GitHub Pages para que todo el mundo pueda acceder. Copilot puede preparar el flujo de trabajo y guiar la publicación, pero tú debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección vas a:

- pedir a Copilot que configure la publicación con GitHub Pages;
- revisar el flujo de trabajo y la solicitud de incorporación de cambios antes de fusionarla;
- seguir la publicación y comprobar el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él mediante una URL pública sin añadir un servidor ni una dependencia de runtime.

## Pedir a Copilot que prepare la publicación

> [!WARNING]
> Crear una solicitud de incorporación de cambios puede confirmar y subir código. Revisa la configuración de aprobaciones y el repositorio de destino antes de enviar este prompt. Si has seleccionado **Approve all**, no esperes que aparezca otro mensaje de confirmación.

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Prepara la publicación de este repositorio en GitHub Pages. Crea un flujo de trabajo de GitHub Actions que, si hace falta, compile el sitio web y lo publique en GitHub Pages cuando los cambios lleguen a la rama predeterminada. Añade un README que describa la aplicación y explique cómo ejecutarla localmente, si todavía no existe. Crea una solicitud de incorporación de cambios con estos cambios e indica su enlace y cualquier configuración o permiso de Pages que haga falta. No fusiones la solicitud, no actives la fusión automática, no cambies la configuración de Pages ni publiques el sitio. Detente y espera a que lo revise.
```

## Revisar el flujo de trabajo y la solicitud de incorporación de cambios

1. Confirma que el README solo se ha añadido si el repositorio todavía no tenía uno.
2. Revisa los cambios de la solicitud, incluidos los pasos de compilación y publicación del flujo de trabajo, los desencadenadores y los permisos.
3. Confirma que el flujo publica desde la rama predeterminada, no desde una solicitud de incorporación de cambios que todavía no se ha fusionado.
4. Pide las correcciones necesarias y revisa el diff actualizado. Espera a que se superen las comprobaciones obligatorias de la solicitud antes de autorizar la fusión.

## Autorizar la fusión y la publicación

> [!WARNING]
> El prompt siguiente autoriza al agente a fusionar la solicitud revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíalo solo después de terminar la revisión y confirmar el repositorio y la solicitud. Según la configuración de aprobaciones, puede que estas acciones no pidan otra confirmación.

Sustituye `<PR_NUMBER>` por el número de la solicitud que has revisado y envía este prompt aparte:

```plaintext
He revisado la solicitud de incorporación de cambios <PR_NUMBER> de este repositorio y autorizo su fusión y la publicación. Habilita GitHub Pages con GitHub Actions, fusiona solo esa solicitud cuando se hayan superado las comprobaciones obligatorias y supervisa la publicación hasta que termine correctamente. Indica la URL final del sitio web publicado. Si no tienes permiso para habilitar Pages o fusionar la solicitud, dime exactamente qué debo aprobar o hacer; no indiques que ha terminado correctamente hasta que el sitio esté disponible.
```

![Diagrama de tres fases: una solicitud de space-quiz fusionada; una ejecución correcta de Deploy website en GitHub Actions, con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz disponible en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verificar la publicación

1. Sigue la ejecución de publicación de GitHub Actions hasta que termine correctamente.
2. Abre la URL indicada y confirma que se carga Space Quiz.

Si el agente no puede habilitar Pages o fusionar la solicitud, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que la publicación haya finalizado correctamente y el sitio esté disponible.

## Resumen y siguientes pasos

Space Quiz ya está disponible como sitio web publicado y has comprobado la publicación en lugar de confiar únicamente en el informe del agente. Continúa con la [lección 13: revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
