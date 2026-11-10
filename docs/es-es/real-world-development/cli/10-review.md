---
title: "Lección 10 - Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI en un flujo de trabajo continuo para Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y localizado la incidencia de filtrado preparada;
- añadido la valoración con estrellas, revisado el resultado en un navegador mediante un puerto reenviado y combinado manualmente tu primera pull request (PR);
- empezado por la incidencia de filtrado, definido el enfoque en modo Plan, creado la funcionalidad en modo Autopilot y revisado el resultado en modo Interactive;
- guiado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y utilizado esa habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadido el servidor Model Context Protocol (MCP) de Playwright y utilizado el navegador para explorar el filtrado;
- creado y seleccionado un agente personalizado de control de calidad para evaluar los requisitos, la cobertura, los resultados de la habilidad y las evidencias del navegador;
- revisado todos los cambios de filtrado y autorizado Agent Merge para la PR correspondiente;
- explorado comandos de barra para gestionar el contexto y los modelos, compartir sesiones y delegar tareas en la nube de forma opcional.

## Lo que has entregado

El taller incluye dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoración con estrellas:** mostrar el valor existente de `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de control de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de control de calidad e incluir las pruebas relacionadas.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y rama. Hemos combinado ese trabajo en una sola PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, examinando el sitio en el navegador y usando Copilot para explorar el navegador mediante MCP. La habilidad `quality-checks` ha ejecutado las pruebas unitarias, el lint y las comprobaciones de tipos, y ha incluido los resultados en el formato nuevo. Antes de la PR, el agente de control de calidad ha reunido esos resultados con una revisión de los requisitos y la cobertura de pruebas.

Las pruebas añadidas deben cubrir carencias reales; si una ejecución del control de calidad determina que no hace falta añadir pruebas, también puede ser correcto. Revisa el código y las evidencias antes de autorizar la integración, y actualiza las evidencias afectadas después de hacer cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado las instrucciones, personalizado una habilidad, creado un perfil de control de calidad y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** según la tarea. Usa **Plan** para estudiar un enfoque antes de empezar, **Interactive** para mantener el control de los cambios acotados y **Autopilot** para las tareas bien delimitadas. Elige un modelo más rápido para los cambios rutinarios y otro más capaz para los trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más cosas que explorar

Ya has recorrido el flujo de trabajo principal. Estas son algunas funciones más de la CLI que puedes probar:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar subtareas independientes en paralelo;
- `/worktree` para aislar una tarea distinta;
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala con código de producción, en proyectos personales o para crear esa pequeña aplicación que llevas años imaginando. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
