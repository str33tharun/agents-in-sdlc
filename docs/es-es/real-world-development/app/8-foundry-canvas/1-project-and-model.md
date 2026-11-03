---
title: "Preparar el proyecto y el modelo"
description: "Exporta el catálogo de Tailspin, crea un proyecto de Foundry y una implementación de modelo, y valídalos en Canvas."
authors:
  - juliamuiruri4
lastUpdated: 2026-10-05
prev:
  link: /copilot-workshops/es-es/real-world-development/app/8-foundry-canvas/
  label: "Opcional: Incorporar Foundry"
next:
  link: /copilot-workshops/es-es/real-world-development/app/8-foundry-canvas/2-build-and-deploy/
  label: Crear e implementar el agente
---

Este primer módulo prepara los datos y los recursos de Azure para Backer Concierge. Todavía no se necesita código del agente ni una implementación hospedada.

Al terminar, tendrás:

- Una exportación del catálogo con límites explícitos sobre la información en la que se pueden basar las respuestas.
- Un proyecto de Foundry y una implementación de modelo elegida según los requisitos de la funcionalidad.
- Una implementación validada en Canvas y una comprobación básica del modelo limitada al catálogo.

## Escenario

Los mecenas de Tailspin Toys pueden filtrar los juegos por categoría y editorial, pero preguntas como *¿Qué juegos serían adecuados para alguien a quien le encantan los juegos de palabras sobre Git?* no se responden con un menú desplegable. Backer Concierge debe recomendar solo juegos del catálogo de Tailspin y nunca inventar juegos, editoriales, valoraciones, importes recaudados, cifras de mecenas, precios, números de jugadores, duraciones de partida ni fechas de lanzamiento. Un catálogo fiable y un modelo adecuado son la base de esas respuestas.

## Preparar las herramientas y la sesión de la incidencia

La configuración conecta la aplicación GitHub Copilot con Azure y mantiene unido todo el trabajo de la funcionalidad.

1. Confirma que tienes una suscripción de Azure. Si necesitas una, las opciones disponibles incluyen una [suscripción gratuita de Azure con 200 $ de crédito][azure-free] o [Azure for Students con 100 $ de crédito][azure-students].
2. Instala la [CLI de Azure][install-azure-cli] para tu sistema operativo y verifica la instalación con `az version`.
3. Instala [Azure Developer CLI][install-azd] y verifica con `azd version` que esté instalada la versión 1.27.1 o posterior.
4. Abre la aplicación GitHub Copilot, abre **Customize** y selecciona **Plugins**. Busca `microsoft-foundry` y selecciona **Install** para el complemento Microsoft Foundry, que incluye Canvas y las habilidades de Foundry.

   ![Instalar el complemento Microsoft Foundry](../../../../_images/app-8-install-foundry-plugin.png)

5. En **Customize**, selecciona **Plugins**, busca `azure` o selecciónalo en la lista **Featured** y, después, selecciona **Install** para el complemento Azure.
6. Selecciona **Issues** en la barra lateral y, a continuación, busca y abre la incidencia titulada **Add a Backer Concierge assistant for catalog questions** en el repositorio de Tailspin Toys. Selecciona **New session** para iniciar una sesión vinculada a la incidencia en un worktree nuevo. Conserva este repositorio, esta rama del worktree y esta sesión de la incidencia durante los tres módulos.
7. Escribe `/microsoft-foundry` y, después, `/azure` para confirmar que ambas habilidades están instaladas y disponibles; no envíes ninguna indicación todavía. Si un complemento no aparece de inmediato, reinicia la aplicación, vuelve a esta misma sesión de la incidencia y compruébalo de nuevo.

## Generar la exportación del catálogo

El repositorio de ejemplo incluye un script de exportación que proporciona al agente un archivo que puede leer.

8. En esta sesión del worktree vinculada a la incidencia, sustituye la indicación predeterminada `/fix-issue` del cuadro de indicaciones por:

   ```plaintext
   Instala las dependencias del proyecto, carga los datos iniciales en la base de datos y ejecuta el script db:export existente. Muéstrame la salida de los comandos y resume la estructura y los límites de la información de db/catalog.json en la que se pueden basar las respuestas.
   ```

9. Revisa la salida de los comandos. Copilot debería ejecutar el equivalente a:

   ```bash
   npm install
   npm run db:setup
   npm run db:export
   ```

   ![Generar la exportación del catálogo](../../../../_images/app-8-generate-catalog-export.png)

10. Abre `db/catalog.json` y confirma que contiene 21 juegos con título, descripción, categoría, editorial y valoración por estrellas. Comprueba el campo `note`: el catálogo no contiene importes recaudados, cifras de mecenas, niveles de aportación ni fechas de lanzamiento. Considera también como no disponibles los precios, números de jugadores y duraciones de partida que falten, en lugar de rellenar los huecos con conocimientos externos. Si la exportación falla o es distinta, pide a Copilot que lo investigue y vuelva a ejecutarla antes de continuar.

   ![Exportación del catálogo abierta en la aplicación Copilot](../../../../_images/app-8-view-catalog.png)

## Configurar un proyecto de Foundry y un modelo

Crear primero el proyecto y la implementación en el chat permite que Canvas se conecte solo a recursos que ya existen.

11. Selecciona **+**, selecciona **Terminal** e inicia sesión en Azure:

    ```bash
    az login
    ```

12. Comprueba la suscripción seleccionada y enumera sus grupos de recursos:

    ```bash
    az account show --output table
    az group list --output table
    ```

    Si la suscripción no es correcta, ejecuta `az account set --subscription <subscription-id>` y repite ambos comandos.

    Si aparece `rg-tailspin-toys`, examina sus recursos:

    ```bash
    az resource list --resource-group rg-tailspin-toys --output table
    ```

    Si el grupo contiene recursos ajenos o compartidos, detente y elige un nombre dedicado antes de utilizar la indicación siguiente. Sustituye los nombres de ejemplo de todas las indicaciones y comandos posteriores por los nombres que apruebes.
13. En la misma sesión de la incidencia, introduce:

    ```plaintext
    Usa la habilidad Microsoft Foundry para crear un grupo de recursos llamado rg-tailspin-toys y un proyecto de Foundry llamado tailspin-toys.
    ```

    ![Crear un proyecto de Foundry](../../../../_images/app-8-foundry-project-created.png)

14. Pide a Copilot que recomiende un modelo. Los criterios de aceptación de la incidencia ya están en el contexto porque la sesión se inició desde ella:

    ```plaintext
    Usa la habilidad Microsoft Foundry para recomendar dos o tres modelos de chat actuales del proyecto tailspin-toys que cumplan los criterios de aceptación de esta incidencia. Explica las ventajas e inconvenientes y espera a que elija uno.
    ```

15. Confirma que Copilot carga la habilidad `microsoft-foundry` y elige un modelo disponible según sus ventajas e inconvenientes. La guía de inicio rápido de agentes hospedados de Microsoft Foundry utiliza actualmente `gpt-5.4-mini`, pero la disponibilidad y la cuota varían según la región.

    ![Seleccionar un modelo](../../../../_images/app-8-select-model.png)

16. Pide a Copilot que implemente el modelo elegido y revisa el proyecto de destino y el coste antes de aprobarlo:

    ```plaintext
    Implementa el modelo que he seleccionado en el proyecto de Foundry tailspin-toys y usa el nombre del modelo como nombre de la implementación.
    ```

> [!TIP]
> La disponibilidad de los modelos cambia con el tiempo. La opción adecuada es el modelo que Copilot confirme que está disponible en tu proyecto, no un modelo fijado de antemano en este módulo.

## Validar el modelo y realizar una comprobación básica en Canvas

Esta comprobación verifica el proyecto y el modelo antes de que exista código del agente. Una comprobación básica del modelo no sustituye las pruebas del módulo 2 que verifican que el agente hospedado se basa en el catálogo.

17. Selecciona **+**, después **Canvas** y, a continuación, **Microsoft Foundry (Preview)**.
18. Abre el menú **More options** en la esquina superior derecha de Canvas y selecciona **Sign in**.
19. Selecciona el proyecto de Foundry **tailspin-toys**. Expande **Models** y confirma que la implementación aparece con el nombre y el estado esperados.

    ![Validar el proyecto y el modelo en Canvas](../../../../_images/app-8-validate-project-model.png)

20. En la misma sesión, escribe:

    ```plaintext
    Usa la habilidad Microsoft Foundry para probar directamente el modelo que he implementado en el proyecto tailspin-toys, sin crear un agente. Basa la prueba en el contenido de @db/catalog.json y pregunta: «Me encantan los juegos de puzles sobre la búsqueda de errores. ¿Cuál debería apoyar y cuánto dinero ha recaudado?». Muéstrame la respuesta y metadatos útiles, como los tokens utilizados y el tiempo de respuesta, solo si están disponibles. Usa mi inicio de sesión de Azure existente. No muestres credenciales, cambies archivos ni crees recursos.
    ```

21. Revisa la respuesta. Debe recomendar únicamente un juego real de `db/catalog.json`, usar el título, la editorial y la valoración correctos, y explicar que la información de financiación no está disponible. Si el modelo inventa un juego, datos del catálogo o una cantidad de financiación, compara otro modelo recomendado antes de continuar.

> [!NOTE]
> Canvas recuerda el proyecto seleccionado cuando se vuelve a abrir. Sus etapas son **Create new hosted agents** para generar la estructura inicial, **Build current hosted agent** para conectar modelos, conjuntos de herramientas, habilidades y medidas de protección, y **Deploy and test** para las ejecuciones locales y la implementación en Microsoft Foundry.

## Punto de control y pasos siguientes

Has preparado las herramientas de Azure, exportado el catálogo y probado un modelo implementado con las reglas de fundamentación de Backer Concierge. El punto de control de este módulo es un modelo que recomienda juegos reales del catálogo sin inventar información que no está disponible.

A continuación, usarás el mismo repositorio de Tailspin Toys, la misma rama del worktree, la misma sesión vinculada a la incidencia, el mismo proyecto de Foundry y la implementación del modelo seleccionada para [crear e implementar el agente][next-module]. Si te detienes aquí, [limpia los recursos de Azure][cleanup] para evitar costes continuos.

[azure-free]: https://azure.microsoft.com/pricing/purchase-options/azure-account
[azure-students]: https://azure.microsoft.com/free/students
[install-azure-cli]: https://learn.microsoft.com/cli/azure/install-azure-cli
[install-azd]: https://learn.microsoft.com/azure/developer/azure-developer-cli/install-azd
[next-module]: ../2-build-and-deploy/
[cleanup]: ../#limpiar-los-recursos
