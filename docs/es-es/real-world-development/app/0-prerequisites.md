---
title: "Lección 0: requisitos previos"
description: "Prepara el entorno para el taller de la aplicación GitHub Copilot: instala Node.js para Tailspin Toys y crea tu copia del repositorio a partir de la plantilla."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La aplicación GitHub Copilot es una aplicación de escritorio que sirve como centro para Copilot y GitHub. Proporciona acceso rápido a las incidencias y las solicitudes de incorporación de cambios y permite desarrollar con GitHub Copilot. Durante este taller trabajarás localmente con la aplicación Tailspin Toys, creada con Astro. Antes de empezar, instala Node.js en tu equipo y, después, instala la aplicación Copilot.

En esta lección vas a:

- instalar Node.js para poder ejecutar las pruebas del proyecto en tu equipo;
- crear una copia propia del proyecto Tailspin Toys a partir de la plantilla.

## Instalar Node.js

En varias lecciones pedirás a un agente que implemente funciones y ejecute en local las pruebas de Tailspin Toys. Para ello necesitas [**Node.js**][nodejs], el único runtime que requiere el proyecto. Instala la versión **LTS** actual.

La opción más sencilla en cualquier sistema operativo es usar el instalador oficial:

1. Abre un terminal en tu sistema operativo, por ejemplo, Windows Terminal, Terminal de macOS o la aplicación que utilices habitualmente.
2. Ejecuta los comandos siguientes para comprobar las versiones instaladas de Node.js y npm:

   ```shell
   node --version
   npm --version
   ```

3. Si ambos comandos funcionan y Node.js cumple los requisitos de `README` y `package.json` del proyecto, pasa a la sección siguiente.

> [!TIP]
> Solo tienes que completar estos pasos si no tienes Node.js instalado o necesitas actualizarlo.

4. Abre la [página de descarga de Node.js][node-download].
5. Descarga la versión **LTS** para tu sistema operativo.
6. Ejecuta el instalador y acepta las opciones predeterminadas. En Windows, deja seleccionada la opción **Add to PATH**.
7. Cuando termine la instalación, abre una ventana de terminal nueva.
8. Comprueba la instalación desde ese terminal:

   ```bash
   node --version
   npm --version
   ```

9. Deberías ver las versiones instaladas de Node.js y npm.

> [!IMPORTANT]
> El agente de la aplicación Copilot también debe poder ejecutar `node --version` y `npm --version`. Que funcionen en tu terminal habitual no garantiza que estén disponibles en la aplicación. Cierra completamente la aplicación y vuelve a abrirla después de instalar Node.js o cambiar `PATH`. Si alguno de los comandos sigue fallando en una sesión, sigue los [pasos para solucionar problemas del entorno de sesión][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada árbol de trabajo también necesita las dependencias del proyecto y Chromium de Playwright para las comprobaciones E2E. Sigue el README del repositorio de Tailspin Toys al preparar un árbol de trabajo y revisa cualquier solicitud de instalación antes de aprobarla.

## Configurar el repositorio del taller

Trabajarás con una copia propia del proyecto Tailspin Toys. Créala ahora a partir del [repositorio de plantilla][template-repository]. El nuevo repositorio incluye todos los archivos necesarios para el taller y lo conectarás cuando instales la aplicación.

1. En una ventana nueva del navegador, ve al repositorio de GitHub de este taller: `https://github.com/github-samples/tailspin-toys`.
2. Crea una copia propia del repositorio seleccionando **Use this template** en la página del repositorio del taller. Después, selecciona **Create a new repository**.

   ![Botón Use this template con la opción Create a new repository seleccionada en el menú desplegable](../../../_images/app-0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

   ![Formulario Create a new repository con github-samples/tailspin-toys como plantilla y el nombre del repositorio completado](../../../_images/app-0-create-repository.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), ya que la necesitarás más adelante en el taller.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se crea automáticamente un backlog de incidencias de GitHub. Trabajarás con estas incidencias durante el taller; no tienes que crear ninguna.

Usa una copia nueva de la plantilla del taller. Incluye las instrucciones del repositorio, el código de la aplicación, las pruebas, una habilidad `quality-checks` y una extensión de lienzo existente. Durante el taller personalizarás la habilidad y crearás un agente de QA. Si usas una copia antigua, confirma con la persona que imparte el taller que contiene los archivos necesarios.

## Resumen y siguientes pasos

Ya tienes todo preparado. En esta lección has:

- instalado Node.js para compilar y probar el proyecto en tu equipo;
- creado una copia propia del repositorio Tailspin Toys a partir de la plantilla.

A continuación, [instalarás la aplicación GitHub Copilot][next-lesson], conectarás el repositorio que acabas de crear y te familiarizarás con el espacio de trabajo.

## Recursos

- [Descargar Node.js][node-download]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#iniciar-una-sesión-desde-la-incidencia
