---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Pide a GitHub Copilot app que configure un flujo de trabajo de Pages, revisa la pull request y comprueba el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica el Space Quiz en GitHub Pages para que todo el mundo pueda acceder a él. Copilot puede preparar el flujo de trabajo y guiarte durante la publicación, pero debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección vas a:

- pedir a Copilot que configure la publicación en GitHub Pages;
- revisar el flujo de trabajo y la pull request antes de combinarla;
- seguir el proceso de publicación y comprobar el sitio publicado.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio de un único archivo con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni una dependencia de entorno de ejecución.

## Pedir a Copilot que publique el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres etapas: una pull request de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y combine una pull request y cambie la configuración de Pages del repositorio. Revisa las diferencias, los permisos de publicación y cualquier solicitud de aprobación de GitHub antes de continuar.

## Revisar el flujo de trabajo y comprobar la publicación

1. Confirma que el archivo README solo se ha añadido si el repositorio todavía no tenía uno.
2. Revisa los cambios de la pull request, incluidos los pasos de compilación y publicación del flujo de trabajo.
3. Combina la pull request solo cuando se hayan superado las comprobaciones obligatorias.
4. Sigue la ejecución de publicación de GitHub Actions hasta que termine correctamente.
5. Abre la URL indicada y confirma que se carga el Space Quiz.

Si el agente no puede habilitar Pages o combinar la pull request, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que la publicación haya finalizado correctamente y el sitio esté disponible.

## Resumen y siguientes pasos

El Space Quiz ya está publicado y has comprobado la publicación en lugar de confiar solo en el informe del agente. Continúa con la [Lección 13: Repaso y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
