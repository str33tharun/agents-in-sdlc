---
title: "Lección 5 - Personaliza y usa una habilidad de comprobaciones de calidad"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya hemos validado manualmente que el código funciona y usado archivos de instrucciones para que siga nuestros estándares. ¿Y las pruebas? ¿El lint? ¿El resto de las tareas de integración continua (CI)?

Las **habilidades de agente** son una buena opción para estas tareas. Ayudan a Copilot a entender cómo ejecutarlas correctamente.

En esta lección:

- explorarás la habilidad existente `quality-checks`.
- personalizarás el formato de sus resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos auxiliares que el agente carga cuando los necesita. En su forma básica, una habilidad es una carpeta con un archivo Markdown llamado `SKILL.md`. El archivo contiene un frontmatter con el nombre y la descripción de la habilidad, una explicación de su función y directrices sobre cuándo usarla. También puede contener subcarpetas con scripts y otros recursos.

> [!NOTE]
> No es obligatorio incluir carpetas ni archivos adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque usa los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para compartirlas y reutilizarlas en el equipo, o en la carpeta de habilidades del usuario `~/.copilot/skills`.

## Explora la habilidad

Veamos la habilidad que ha creado el equipo Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos: `quality-checks`.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo usar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot por el proceso de pruebas y lint.
4. Fíjate en que la habilidad todavía no contiene una sección **Formato de salida de resultados**.

## Ejecuta la habilidad antes de hacer cambios

Puedes invocar habilidades directamente desde Copilot CLI o pedirlo en lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Usa este mensaje:

   ```plaintext
   Ejecuta la habilidad quality-checks para realizar pruebas unitarias, lint y comprobaciones de tipos.
   ```

3. Observa el informe final.

## Personaliza el informe

Queremos que el informe indique qué se ha ejecutado, si se ha superado y qué han comunicado las herramientas. Actualicemos la habilidad para que genere ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Formato de salida de resultados

   Al terminar, indica cada comando ejecutado y si se ha superado, ha fallado o ha quedado bloqueado. Incluye el recuento de pruebas, la duración, los errores, las advertencias y otras métricas solo cuando la herramienta los comunique. Indica el siguiente paso para cada fallo o bloqueo y nunca describas una comprobación omitida o incompleta como superada.
   ```

3. El archivo se guardará automáticamente.

## Ejecuta la habilidad actualizada

Ya hemos hecho el cambio; veamos cómo funciona. Copilot CLI puede volver a cargar las habilidades modificadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente el mismo mensaje que antes:

   ```plaintext
   Ejecuta la habilidad quality-checks para realizar pruebas unitarias, lint y comprobaciones de tipos.
   ```

3. Observa el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has vuelto a cargar y ejecutado la habilidad.

Este cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor Playwright MCP][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
