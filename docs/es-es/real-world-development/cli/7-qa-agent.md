---
title: "Lección 7 - Crear y usar un agente de control de calidad"
description: "Crea un agente personalizado de control de calidad que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora combinarás esas capacidades en un agente personalizado con un proceso de control de calidad definido.

En esta lección vas a:

- explorar cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP;
- crear e inspeccionar un perfil de control de calidad reutilizable;
- seleccionar el agente de control de calidad y revisar sus conclusiones sobre la incidencia de filtrado.

## Escenario

Antes de abrir una pull request (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el control de calidad y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil de Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller, definirás el rol de control de calidad en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has utilizado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` reúne comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de control de calidad indica a Copilot cómo utilizar esas capacidades para evaluar los requisitos e informar de sus conclusiones. No sustituye a las demás personalizaciones ni requiere otra conversación.

## Crear el perfil de control de calidad

Antes de abrir la PR de la funcionalidad, pedirás a Copilot que cree un perfil reutilizable de control de calidad. El perfil definirá las comprobaciones y los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspeccionar el perfil

Antes de usar el agente nuevo, revisa su perfil para confirmar que Copilot ha incluido el flujo de control de calidad y sus límites. Así evitarás que un perfil incompleto o demasiado amplio modifique la funcionalidad cuando solo quieres que se compruebe.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero incluirlo proporciona un nombre claro en la interfaz.
3. Lee las instrucciones del perfil y confirma que el control de calidad parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y usa Playwright MCP.
4. Confirma que el agente informa de las pruebas con evidencias, pregunta antes de modificar el código de implementación y no crea commits ni pull requests.
5. Si el perfil generado omite alguna responsabilidad o límite, pide al agente predeterminado que lo revise antes de continuar.

## Ejecutar el control de calidad con la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, retoma la misma conversación de filtrado y selecciona el agente de control de calidad para que pueda usar la incidencia y las decisiones del plan que ya están en el contexto.

1. Activa el agente con el prompt siguiente:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que no aparezca en la lista de agentes disponibles. Aun así, el comando anterior lo activará.

2. Usa el prompt siguiente para pedir al agente que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de control de calidad empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable y has revisado su trabajo. En esta lección has:

- explorado cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP;
- creado e inspeccionado un perfil de control de calidad reutilizable;
- seleccionado el agente de control de calidad y revisado sus conclusiones sobre la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil de control de calidad, las pruebas y el informe de verificación listos para revisión. A continuación, [reunirás los cambios en una PR de funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
