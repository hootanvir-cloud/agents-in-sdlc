---
title: "Ejercicio 2: Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo habitual de desarrollo implica localizar los archivos necesarios, hacer cambios, ejecutar pruebas, depurar, averiguar qué archivo falta y volver a modificarlo. Y así sucesivamente.

El modo agente de Copilot ayuda con este proceso.

El modo agente puede trabajar con más autonomía dentro del IDE. Se comporta de forma parecida a un desarrollador: explora la estructura existente, realiza las actualizaciones necesarias, ejecuta tareas como pruebas y corrige automáticamente los errores que detecta. Veamos cómo usarlo para incorporar nuevas funciones al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, permite una interacción directa y ciclos de comentarios rápidos. El agente en la nube trabaja de forma asíncrona, como un miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto, localizar archivos pertinentes y coordinar cambios.
- el modo agente de GitHub Copilot puede implementar funciones en la interfaz y en la capa de datos.
- revisar los cambios y las pruebas generados antes de combinar el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir el filtrado por categoría y editorial. En el ejercicio anterior ya añadiste un asistente de editoriales. Ahora completarás con el modo agente el resto del trabajo de la capa de datos, la interfaz y las pruebas.

## Ejecutar el sitio Tailspin Toys

Antes de hacer cambios, explora el sitio para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite consultar los juegos y sus detalles. Es una única aplicación Astro que genera HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde el frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los asistentes reutilizables de acceso a datos están en `src/lib/`, y la interactividad se añade con un `<script>` Astro pequeño y acotado que usa las API estándar del DOM.

### Iniciar el sitio web

El comando `npm run dev` inicia el único servidor de desarrollo Astro.

1. Vuelve al Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando esté listo el servidor Astro, verás un mensaje con una URL similar a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si se abre un cuadro de diálogo para cargar `http://localhost:4321` en el navegador, ciérralo seleccionando **x**.

5. Para abrir el sitio, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal.

> [!NOTE]
> En un Codespace, al seleccionar el enlace local del terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando se esté ejecutando, explora sus funciones principales:

- **Página de inicio**: muestra los juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de seguir, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para empezar una conversación limpia. La selección de modo y modelo varía según el ejercicio; cada paso lo indicará cuando corresponda.

El sitio funciona, pero queremos añadir capacidades. Empecemos por revisar el backlog. Al crear el proyecto desde la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te las muestre.

1. En el menú de agentes de la vista Chat, selecciona **Agent**. Este agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal e invoca herramientas de forma autónoma.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. En la lista de modelos disponibles, selecciona **Claude Sonnet 4.5**.

> [!CAUTION]
> Los autores del taller no expresan preferencia por un modelo concreto. Usamos Claude Sonnet 4.5 al crear las instrucciones para que las sugerencias fueran relativamente coherentes y ofrecer una buena experiencia. Sin embargo, como los LLM son probabilísticos, las sugerencias pueden diferir de las indicadas. Es normal y esperado.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot podría usar otro comando MCP, pero debería poder completar la tarea.

3. Envía este prompt para consultar el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se ejecute el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot incluso ha priorizado las incidencias según su utilidad para las personas que usan el sitio.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales, entre ellas:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones para distintos tipos de archivos, incluidos:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar las instrucciones, dividir conjuntos complejos en partes más manejables e incluir ejemplos y plantillas.

## Implementar el filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios distintos en la aplicación:

- añadir o ajustar la lógica de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar pruebas del comportamiento del filtrado.
- actualizar la página que muestra los juegos para incorporar los controles de filtrado.

Además, las pruebas deben ejecutarse correctamente antes de combinar el código. El modo agente de Copilot puede encargarse de estas tareas.

1. Continúa en la conversación actual o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** siga seleccionado en el menú de agentes.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que siga seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad a partir de la incidencia correspondiente del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la función, incluidos los asistentes de la capa de datos, la interfaz y las pruebas. Después modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad del trabajo, puede tardar unos minutos. Mientras trabaja, quizá aparezcan los controles **Keep** y **Undo** en el editor. Cuando termine, tendrás un control **Keep** o **Undo** para cada cambio; no tienes que seleccionarlos mientras está trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Copilot Chat pide confirmación para ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Durante el proceso puede haber pausas y algunas pruebas pueden fallar. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio abierto y explora la nueva función.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo agente de GitHub Copilot para añadir capacidades al sitio Tailspin Toys:

- el modo agente implementa funciones en la interfaz y la capa de datos.
- el modo agente explora el proyecto, localiza archivos pertinentes y coordina cambios.
- puedes revisar los cambios y las pruebas antes de combinar el código.

Ahora [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Usa lo aprendido para pedir a Copilot que actualice el sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas, incluidas las instrucciones existentes.
- decide cómo debe funcionar la paginación: por ejemplo, si la persona usuaria puede elegir el tamaño de página o si será fijo.
- proporciona a Copilot la información necesaria para implementar el enfoque que quieras.
- quizá tengas que iterar con GitHub Copilot, pedir cambios y aportar contexto. Es lo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
