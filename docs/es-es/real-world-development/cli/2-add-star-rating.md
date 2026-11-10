---
title: "Lección 2 - Añadir valoraciones con estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un cambio pequeño en las tarjetas de juegos, revísalo en un navegador mediante un puerto reenviado y combínalo en tu primera pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y has probado una conversación. Ahora harás tu primer cambio en el proyecto. Será pequeño: los juegos ya tienen una valoración con estrellas en los datos, pero todavía no se muestra en las tarjetas de la página de inicio. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera pull request.

En esta lección vas a:

- iniciar una conversación específica de Copilot en una rama de funcionalidad;
- pedir al agente que haga un cambio pequeño en el proyecto;
- revisar el cambio con `/diff`;
- ejecutar la aplicación y comprobar el cambio en un navegador mediante un puerto reenviado;
- abrir y combinar tu primera pull request.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de juegos de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre en cada tarjeta la valoración existente. Es un cambio pequeño e independiente, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de la CLI utiliza el repositorio y la rama de Git que están seleccionados en el terminal, en lugar de crear un worktree específico. Las conversaciones guardadas te permiten volver a la misma conversación más adelante, mientras que los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás tres elementos: tus prompts y las respuestas del agente, la actividad de las herramientas que usa el agente para explorar y editar archivos, y los cambios que puedes inspeccionar con `/diff`.

## Iniciar una conversación y solicitar el cambio

Inicia una conversación nueva para implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal anterior no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot CLI aún no está abierto, inicia Copilot con el comando siguiente:

   ```bash
   copilot --yolo
   ```

4. Para iniciar una sesión nueva, escribe el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa este prompt para solicitar el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. Ya has añadido una funcionalidad con Copilot CLI.

## Revisar las diferencias

Todos los cambios generados con IA deben revisarse antes de combinarlos, por pequeños que sean. Inspecciona los cambios desde Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` tiene el valor `null`.
3. Confirma que las pruebas cubren ambos estados.
4. Revisa los resultados de las comprobaciones ejecutadas por Copilot y pídele que corrija cualquier error.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, es probabilístico y no determinista, por lo que el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación idéntica.

## Comprobar los cambios

No basta con leer el código y dar por sentado que funciona. Pide a Copilot que inicie el sitio web para poder examinar la interfaz de usuario (UI) actualizada en el navegador abierto desde Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir y combinar tu primera pull request

Ya has creado la funcionalidad. Ahora abrirás una pull request (PR) para incorporar el código al proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI enviará el commit existente al crear la PR y mostrará la URL.
3. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la URL que muestra Copilot CLI para abrir la PR.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto, has:

- iniciado una conversación específica de Copilot en una rama de funcionalidad;
- pedido al agente que hiciera un cambio pequeño en las tarjetas de juegos;
- revisado el cambio con `/diff`;
- ejecutado la aplicación y comprobado la valoración con estrellas en un navegador mediante un puerto reenviado;
- abierto y combinado tu primera pull request.

A continuación, [empezarás por la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
