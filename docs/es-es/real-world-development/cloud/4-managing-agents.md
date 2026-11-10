---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente de Copilot en la nube que se ocupe de tres tareas para mejorar la experiencia y añadir funcionalidad. Aunque el agente trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Tienes varias herramientas para gestionar las tareas asignadas al agente, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes consultar todas las tareas de agentes que tienen pull requests (PR) abiertas, revisar las operaciones realizadas e incluso orientar una sesión que sigue en curso.

En esta lección vas a:

- explorar la página de agentes para supervisar las tareas del agente en la nube;
- dirigir una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación de un modo de alto contraste, el equipo se ha dado cuenta de que también sería un buen momento para añadir un modo claro. Como ya se están actualizando los estilos y se está añadiendo una opción para alternarlos, tiene sentido incluirlo ahora. Quieres orientar el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisar las tareas del agente de Copilot en la nube

Consulta el estado de las tareas que has asignado al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - preparar el entorno para Copilot;
   - configurar servidores MCP;
   - realizar distintas tareas para completar el trabajo asignado.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de pull request (PR) de la derecha. Desde allí puedes consultar la PR y los archivos modificados.

## Dirigir el agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que le haya dado Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión unos minutos, hasta que indique que ha terminado de preparar el entorno y ha empezado a trabajar. Verás mensajes que indican ese progreso.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarea del agente en la nube con el cuadro de diálogo para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo recibe y lo incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot seguirá trabajando en la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba ejecutando al recibir el mensaje.

El proceso tardará varios minutos. Puedes aprovechar el tiempo para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente de Copilot en la nube. Puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para ajustar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- usar Mission Control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las pull requests creadas por Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
