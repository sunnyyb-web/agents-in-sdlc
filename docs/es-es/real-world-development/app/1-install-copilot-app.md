---
title: "Lección 1: instala la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, y automatizas tareas repetitivas en lugar de hacerlo todo por tu cuenta. Con Node.js instalado y tu copia del proyecto preparada, el siguiente paso es instalar la aplicación y conectar el repositorio.

En esta lección vas a:

- instalar la aplicación GitHub Copilot e iniciar sesión;
- añadir el proyecto a la aplicación desde su repositorio de GitHub;
- familiarizarte con el espacio de trabajo y el backlog que ha creado la plantilla;
- probar un chat rápido para conocer la propia aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para abordar un backlog cada vez mayor. La aplicación Copilot ofrece un único lugar desde el que dirigir ese trabajo: elegir incidencias, ejecutar agentes, revisar cambios y fusionar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación, y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Necesitas un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si usas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva de la **aplicación GitHub Copilot**.

## Instalar y configurar la aplicación GitHub Copilot

El primer paso para usar la aplicación GitHub Copilot es instalarla. Hay versiones para Windows, macOS y Linux. Vamos a instalarla, iniciar sesión y añadir el repositorio de Tailspin Toys.

1. En un navegador, abre la [página de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación después de instalarla.
4. Selecciona **Sign in to GitHub** y sigue las instrucciones para iniciar sesión con una cuenta de GitHub.com o GHE.com. Si usas GitHub Enterprise Cloud con residencia de datos, elige **GitHub Enterprise Cloud** y escribe el nombre de host `*.ghe.com` cuando se te solicite.

   Si usas GitHub Enterprise Server, primero completa la incorporación con tu cuenta en la nube. Después, abre **Settings**, selecciona **Accounts**, selecciona **Add account** y elige **GitHub Enterprise Server**. Sigue las instrucciones para autorizar la aplicación en el navegador, como se explica en la [guía de inicio de sesión en Enterprise Server][ghes-sign-in].

5. Si durante la incorporación se te pide que selecciones un repositorio, selecciona el repositorio de Tailspin Toys que has creado a partir de la plantilla. También puedes omitir este paso y añadir el proyecto al terminar la incorporación.
6. Cuando se te solicite, elige un tema y completa la incorporación.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece, no hace falta que lo añadas de nuevo.
8. Si todavía no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Escribe el propietario y el nombre del repositorio que anotaste en los requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En el diseño de este taller, las carpetas de repositorios, como `tailspin-toys`, aparecen agrupadas en **Sessions**, con sus sesiones debajo de cada repositorio.

## Familiarizarte con el espacio de trabajo

Con el proyecto conectado, dedica un momento a explorar el espacio de trabajo. La aplicación organiza sus funciones en varias áreas de la barra lateral:

- **New**: permite iniciar una sesión de chat nueva con Copilot.
- **Pull requests**: muestra tus solicitudes de incorporación de cambios mediante la integración nativa con GitHub. Desde aquí puedes explorarlas y filtrarlas, comprobar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues**: muestra tus incidencias de la misma forma. Puedes explorarlas y filtrarlas, e iniciar una sesión a partir de una incidencia.
- **Automations**: contiene tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Sirven, por ejemplo, para gestionar listas de tareas, realizar mantenimiento periódico del proyecto o delegar otras tareas tediosas. El resumen final las presenta como un siguiente paso, no como otro ejercicio del taller.
- **Customize**: permite añadir funciones a la aplicación Copilot mediante servidores MCP, plugins, habilidades y otros componentes. La usarás para configurar Playwright MCP.
- **Chats**: contiene conversaciones breves para preguntas y lluvias de ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para empezar un chat.
- **Sessions**: agrupa las carpetas de repositorio y las sesiones de agente. Selecciona un repositorio, como `tailspin-toys`, para abrir su vista, o una sesión para volver a esa conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, así que puedes ejecutar varias a la vez sin que sus cambios entren en conflicto. Iniciarás tu primera sesión al añadir valoraciones con estrellas.

Explorarás el espacio de trabajo durante el taller.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, Copilot puede orientarte.

### Encontrar el backlog inicial

Es difícil encontrar un proyecto sin trabajo pendiente, y Tailspin Toys no es una excepción. Vamos a explorar el backlog creado al generar el repositorio a partir de la plantilla.

1. En la barra lateral, dentro de **Sessions**, selecciona la carpeta del repositorio de Tailspin Toys, no una sesión que aparezca debajo. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título, no por el número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer sus detalles. Cada incidencia también sirve para iniciar una sesión de agente. Empezarás desde la incidencia de filtrado después de hacer un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** se filtran automáticamente para mostrar solo los elementos de los repositorios que has añadido a la aplicación Copilot. Para ver elementos de otros repositorios, añádelos a la aplicación.

## Probar un chat rápido

Una buena forma de familiarizarte con la aplicación es usarla para aprender sobre la propia aplicación. Un **chat rápido** es justo lo que necesitas: puedes hacer una pregunta o plantear ideas sin crear una rama ni un árbol de trabajo. Es ideal para consultas breves y desechables que no requieren una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat nuevo. Si **Chats** todavía no aparece, selecciona **+** junto a **Sessions** para iniciar el primer chat. **Chats** aparecerá después.
2. Pregunta cómo funcionan las sesiones de la aplicación:

   ```plaintext
   ¿Cómo usa la aplicación GitHub Copilot los worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo de Git aislado, lo que te permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar otra cuando quieras.

## Resumen y siguientes pasos

¡Enhorabuena! Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub;
- añadir un proyecto desde su repositorio de GitHub;
- familiarizarte con el espacio de trabajo y encontrar el backlog inicial en **Issues**;
- usar un chat rápido para hacer una pregunta breve y desechable.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y la usarás para mostrar las valoraciones con estrellas en las tarjetas de los juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
