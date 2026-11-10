---
title: "Ejercicio 2 - Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo de desarrollo suele consistir en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, hacer otra actualización… y así sucesivamente.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a ver cómo puedes usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE para que puedas iterar y colaborar rápidamente. El agente en la nube trabaja de forma asíncrona como integrante del equipo y se comunica contigo mediante incidencias y pull requests.

En este ejercicio aprenderás a:

- usar el modo de agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y realizar cambios coordinados;
- utilizarlo para implementar funcionalidades en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generados por el modo de agente antes de combinarlos con el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que se filtren por categoría y editorial. En el ejercicio anterior ya creaste un helper de editoriales; ahora completarás con el modo de agente de Copilot el trabajo pendiente en la capa de datos, la interfaz y las pruebas.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio ofrece financiación colectiva para juegos de mesa con temática de desarrollo. Permite consultar juegos y ver información detallada sobre ellos. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los helpers reutilizables para acceder a los datos están en `src/lib/`. La interactividad se añade mediante pequeños elementos `<script>` de Astro con ámbito limitado y las API DOM estándar.

### Iniciar el sitio web

El proyecto incluye la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve a tu Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal en el Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, verás un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si se abre un cuadro de diálogo para preguntarte si quieres abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace `localhost` en el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso te indicará qué seleccionar cuando sea necesario.

El sitio ya funciona, pero queremos añadir nuevas funciones. Empieza por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generaron automáticamente varias incidencias. Pide a GitHub Copilot que te las muestre.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente puede planificar e implementar cambios en varios archivos, ejecutar comandos de terminal e invocar herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no expresan preferencia por un modelo concreto. Al crearlo, utilizamos Claude Sonnet 4.5 y lo indicamos para que las sugerencias sean relativamente coherentes y la experiencia resulte satisfactoria. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, las sugerencias pueden diferir de los ejemplos. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot puede utilizar un comando MCP distinto, pero debería poder completar la tarea.

3. Pide a Copilot que muestre las incidencias del backlog con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según el valor que podrían aportar a las personas usuarias.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidad. Incluyen:
   - una descripción de la arquitectura;
   - principios para diseñar componentes, realizar pruebas y aplicar la accesibilidad;
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir enlaces a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones, lo que permite reutilizar contenido, dividir conjuntos complejos en partes más pequeñas y proporcionar ejemplos y plantillas a Copilot.

## Implementar el filtrado

Para completar el filtrado hay que hacer al menos tres cambios independientes en la aplicación:

- añadir o ajustar la lógica restante del filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben superarse antes de combinar los cambios con el código base. El modo de agente de Copilot puede encargarse de estas tareas.

1. Continúa la conversación actual con Copilot o inicia otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de cómo seleccionar el archivo de instrucciones de la interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes. Deberías ver que encuentra los helpers de la capa de datos, la interfaz y las pruebas. Después, empezará a modificar archivos y ejecutar las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar unos minutos. Mientras trabaja, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, podrás aceptar o deshacer todos los cambios; no es necesario seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat para confirmar la ejecución de pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión de Copilot Chat completada con el modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el modo de agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Has aprendido a:

- utilizar el modo de agente de GitHub Copilot para implementar funcionalidades en la interfaz y la capa de datos;
- dejar que Copilot explore el proyecto, identifique los archivos pertinentes y realice cambios coordinados;
- revisar los cambios y las pruebas generados por el modo de agente antes de combinarlos con el código base.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una pull request.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crezca la lista de juegos, puede que sea necesario añadir paginación. Utiliza lo que has aprendido para pedir a Copilot que implemente esta función. Ten en cuenta lo siguiente:

- sigue las buenas prácticas, incluidos los archivos de instrucciones existentes;
- decide cómo quieres implementar la paginación, por ejemplo, si las personas usuarias podrán seleccionar el tamaño de página o si será fijo;
- proporciona a Copilot las directrices necesarias para implementar el comportamiento que quieres;
- puede que tengas que iterar con GitHub Copilot, pedir cambios y aportar más contexto. Es lo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
