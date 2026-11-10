---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas Copilot en modo de agente, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code te muestra cada llamada a herramientas y cada edición a medida que se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cambios y orientar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio vas a:

- supervisar en tiempo real el trabajo del agente de accesibilidad;
- revisar y aceptar las ediciones de archivos propuestas;
- pedir al agente durante la sesión que añada un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has utilizado en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha añadido otra solicitud: también quiere un modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En vez de iniciar una conversación nueva, ampliarás el trabajo desde la sesión actual.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Las ediciones aparecen en línea en el chat con las opciones **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas. Aparecerán las lecturas y ediciones de archivos, además de los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, aparecerá una diferencia en línea:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para deshacerlo y dejar que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te resulten aceptables.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Pedir al agente que añada el modo claro

Ahora que está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un modo claro. Como continuarás en la misma sesión de chat, el agente conservará el contexto del trabajo anterior, incluidos los componentes que modificó, el enfoque de estilos y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento desde el cuadro de entrada de Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente retomará el trabajo, explorará los cambios que ya ha hecho y propondrá los cambios necesarios para el modo claro.
3. Revisa cada edición nueva a medida que aparezca, igual que hiciste con el modo de alto contraste. Acepta o deshaz cada diferencia con **Keep** o **Undo**.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta reorganizar el selector de modos de una forma que no te gusta—, puedes detenerlo con el botón **Stop** junto al cuadro de entrada del chat y enviar un mensaje de seguimiento con instrucciones más claras.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes («simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer selector; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos modos»).

## Revisar el conjunto de cambios

Antes de confirmar el trabajo, revisa brevemente todos los cambios del agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que se aplican las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has explorado:

- cómo supervisar el trabajo de un agente en tiempo real;
- cómo aceptar y rechazar ediciones individuales;
- cómo orientar una sesión en curso mediante prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una pull request y revisarás los cambios que ha producido el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
