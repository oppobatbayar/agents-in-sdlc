---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia y añadir funcionalidad. Le has pedido que documente el código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar y añada funciones de accesibilidad, incluidos los modos de alto contraste y claro. Revisa los cambios de código y, si es necesario, envía comentarios a Copilot para mejorar el resultado.

### Escenario

Como hemos señalado varias veces, la incorporación de la IA generativa no cambia los fundamentos del diseño de software ni de DevOps. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Ahora revisarás las sugerencias de GitHub Copilot para la documentación, la función de juegos relacionados y las mejoras de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube trabaja de forma asíncrona y sin supervisión, cuenta con algunas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente se ejecuta en GitHub Actions, dentro de un entorno independiente y efímero.
- Los flujos de trabajo de GitHub Actions necesitan la aprobación de una persona antes de ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada][agent-firewall], incluidos los servidores MCP.

## Revisar la documentación generada

Empieza por la primera pull request (PR) creada por el agente de Copilot en la nube: la que añade documentación al código. La revisarás desde la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Es posible que veas una advertencia que indica que un firewall bloquea a GitHub Copilot. Es **lo esperado**, ya que el acceso del agente en la nube a recursos externos, incluidos los servidores MCP, está limitado de forma predeterminada. Si quieres, puedes [personalizar o desactivar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de pull requests.
3. Abre la pull request titulada algo parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la pull request incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá comentarios de documentación TSDoc y otros cambios. El contenido exacto puede variar.

   Busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones que has revisado al principio del Ejercicio 2.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto de la aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

Trabajar con Copilot en una pull request no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios insertados en el código, como harías con cualquier integrante del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados son probabilísticos, no es posible dar una frase exacta para todos los casos. Puedes pedirle, por ejemplo, que:

- añada a cada archivo de código un encabezado con una breve descripción de su contenido;
- añada comentarios de documentación TSDoc a los archivos TypeScript y Astro;
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier persona. Puedes usar una de las ideas anteriores o pedir otra mejora de documentación.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la PR.

   ![Volver a la pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, debería aparecer un commit nuevo en la pull request.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

Vuelve a la PR que Copilot ha creado para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que avisa de que algunos flujos de trabajo esperan aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto de la aplicación Astro se superarán. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en Codespaces para probar manualmente la función de juegos relacionados. Ve a Codespaces, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Puedes seguir iterando con el agente en la nube para solicitar cambios, por ejemplo, ajustar cuántos juegos relacionados se muestran o añadir encabezados de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Como en la PR anterior, añade un comentario en la pestaña **Conversation** para que Copilot vea la solicitud e inicie otra sesión.

## Revisar las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas mediante el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste asignado en el Ejercicio 3 y el modo claro solicitado desde Mission Control en el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía está trabajando, la pull request incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para alternar entre los modos;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios de CSS o de estilos para los modos de alto contraste y claro;
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado;
   - el código JavaScript/TypeScript que gestiona el cambio de modo.
6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que avisa de que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto de la aplicación Astro se superarán. El proceso puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en Codespaces para probar manualmente las funciones de accesibilidad. Ve a Codespaces, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha guiado a Copilot para aplicar buenas prácticas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionarlo en un comentario con **@copilot**, como hiciste con las PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta útil cuando puedes asignarle incidencias reales del backlog y ocuparte de otra cosa mientras trabaja. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés para personas que apoyan los juegos en la página de detalles;
- añadir paginación a la lista de juegos;
- validar las entradas y gestionar errores en los helpers de acceso a datos.

## Resumen

Has completado el taller del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para observar cómo influyen después en los resultados del agente en la nube.
- **Has asignado incidencias al agente de Copilot en la nube** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para mejorar la accesibilidad con modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como Mission Control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Has revisado e iterado sobre las pull requests del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Repaso y siguientes pasos

Has completado el taller del agente en la nube. Si quieres seguir explorando, los otros talleres complementan lo que has aprendido:

- 🖥️ **[Taller de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Taller de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de control de calidad, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
