---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad y las pruebas; después crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos juntos y abrir una pull request. Combinaste tú mismo la pull request (PR) de valoración con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de las instrucciones, la actualización de la habilidad y el agente de control de calidad en varias PR. Para simplificar el taller, has mantenido todo el flujo de trabajo de filtrado y control de calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección vas a:

- aprender qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- inspeccionar todos los cambios de la funcionalidad y las evidencias de verificación;
- crear la PR de filtrado;
- activar Agent Merge solo después de la revisión y confirmar que la PR se ha combinado.

## Escenario

Durante el flujo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una pull request. Al activarlo, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones fallidas de integración continua (CI), responde a comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando sea necesario. Después, activa la combinación automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora has seleccionado tú la opción **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usar Agent Merge para gestionar la PR

Ya tienes todo el código. Revísalo, crea la PR y deja que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se haya superado, la combinará.
7. Confirma que aparece un mensaje de Copilot parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo que se muestra y completa manualmente la combinación revisada cuando la configuración del repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y ahora gestionar la pull request. En concreto, has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación;
- inspeccionado todos los cambios de la funcionalidad y las evidencias de verificación;
- creado la PR de filtrado;
- activado Agent Merge después de la revisión y confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto y los modelos, compartir sesiones y delegar tareas en la nube de forma opcional.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
