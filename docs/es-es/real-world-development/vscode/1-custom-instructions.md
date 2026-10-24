---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe hacerse de una manera concreta —o Copilot debe conocer información de fondo—, asegúrate de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] son uno de los principales recursos para proporcionar orientación sobre la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones asociados a rutas.
- generarás la primera parte de los datos para el filtrado (una función auxiliar de acceso a datos de editoriales) con las instrucciones actuales.
- añadirás un nuevo estándar global a `.github/copilot-instructions.md`.
- repetirás el mismo prompt y observarás cómo el código generado adopta el nuevo estándar.
- confirmarás en `main` la actualización de las instrucciones y la primera parte del filtrado para que Copilot pueda usar las nuevas directrices en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no cumplir algunos de los estándares definidos. Copilot no es determinista. El objetivo es observar la *tendencia* del comportamiento después de actualizar las instrucciones, no obtener resultados idénticos carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones te asegurarás de que Copilot disponga del contexto necesario para realizar el trabajo de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que pueda comprender mejor tu estilo y tus requisitos. Permiten orientar las sugerencias y los fragmentos de código con convenciones de programación, bibliotecas y tipos de comentarios. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe incluir información global pertinente, como la pila tecnológica, una descripción del proyecto, prácticas recomendadas y directrices.
- `.github/instructions/*.instructions.md`: archivos con directrices para lenguajes (como TypeScript o Astro), tareas o tipos de archivo concretos (como crear un componente de interfaz o pruebas unitarias).

> [!NOTE]
> En un IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, pero no para completar código ni para sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también [admite archivos de instrucciones de otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices globales del proyecto, como su descripción, estructura y estándares de programación.
- Usa archivos `*.instructions.md` para instrucciones específicas sobre tipos de archivo (pruebas unitarias, componentes Astro o capa de datos) o tareas.
- Escribe en lenguaje natural. Expresa las directrices con claridad e incluye ejemplos de código correctos e incorrectos.

No existe una única forma correcta de crear archivos de instrucciones, del mismo modo que no hay una única manera de usar IA. La experimentación te permitirá descubrir qué funciona mejor en cada proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto hay instrucciones para varias tareas, incluidas las [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada interfaz lo ofrece de una forma distinta, como **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección de cada interfaz lo indicará cuando corresponda.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], que reúne archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizados del proyecto

El repositorio incluye un archivo principal `copilot-instructions.md` y varios `*.instructions.md` para tareas concretas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Revisa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas **GitHub Actions Workflows**. Se aplican a las interacciones con Copilot.
3. Abre `.github/instructions` y explora sus archivos, con instrucciones para Astro, Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` define un patrón glob relativo a la raíz del repositorio; por ejemplo, coincide con los archivos de prueba TypeScript `**/*.test.ts`.
5. Revisa las instrucciones para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate al final. Observa sus enlaces a otros archivos de instrucciones y a archivos del proyecto. Así puedes dividir conjuntos amplios de instrucciones en archivos reutilizables y proporcionar ejemplos. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares del proyecto, pero todavía no exige documentación en el código. Añadirás reglas para comentarios TSDoc y encabezados de comentario.

## Crear una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea y selecciona una rama:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La selección de modo y modelo varía en cada ejercicio; cada paso lo indicará cuando sea pertinente.

Para observar el efecto de las instrucciones personalizadas, primero envía un prompt con el archivo de instrucciones actual. Más adelante lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas de editor de los ejercicios anteriores para que Copilot reciba solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar dónde debe ubicarse la función auxiliar.
3. En la vista Chat, selecciona **Agent** en el menú de agentes para permitir que Copilot modifique archivos.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, normalmente al archivo de la función auxiliar y a sus pruebas.
6. Observa que la función auxiliar propuesta es una función tipada que acepta un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto sigue las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario del archivo.

> [!CAUTION]
> Como Copilot es probabilístico, quizá añada comentarios incluso sin que se lo pidas. No pasa nada; el objetivo es observar la mejora de coherencia tras actualizar las instrucciones.

## Añadir un estándar para todo el repositorio

El archivo `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añadiremos un estándar para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que todavía no incluye documentación en el código, de ahí que la función auxiliar generada no tuviera comentarios.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear instrucciones en `.github/copilot-instructions.md` para directrices globales o archivos `*.instructions.md` para lenguajes, tipos de archivo y tareas concretos. El archivo del repositorio es el lugar adecuado para estándares globales como la regla que acabas de añadir.

## Repetir el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el **mismo prompt**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Acabas de orientar a Copilot para que siga un nuevo estándar y lo aplique a código real sobre el que se trabajará en el siguiente ejercicio.

## Confirmar las instrucciones y subir la rama

Los archivos de instrucciones son recursos del repositorio y se gestionan con el mismo control de versiones que los demás archivos. Confirmemos y subamos la rama.

1. Abre otro terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que estén presentes los cambios de instrucciones y de la función auxiliar:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos modificados por Copilot para la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Sube la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Ya has subido la rama. Ahora crearás una solicitud de incorporación de cambios vinculada a la incidencia sobre el estándar de documentación del backlog. Copilot puede localizar la incidencia, crear una PR vinculada y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub con el espacio de trabajo, así que no tienes que configurarlas. La primera vez que Copilot use una herramienta, VS Code quizá te pida que inicies sesión en GitHub. Sigue las indicaciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que localice la incidencia sobre las instrucciones y cree una PR desde la rama actual, describiendo tanto los cambios de instrucciones como la base de la función auxiliar de acceso a datos de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Cuando se te solicite, selecciona **Continue** para permitir que Copilot reúna información y realice operaciones. La primera vez que use una herramienta de GitHub, quizá tengas que autenticarte.
5. Observa cómo Copilot busca la incidencia correcta y crea la PR.
6. Cuando se haya creado, pide a Copilot que la combine y vuelva a `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando se te solicite permitir acciones de GitHub o comandos de shell, revisa el comando y selecciona **Allow** según corresponda.

¡Ya has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un estándar global nuevo a `.github/copilot-instructions.md`.
- repetir el mismo prompt y observar cómo el código propuesto adopta el estándar.
- confirmar en `main` la actualización de instrucciones y la base de la función auxiliar para que el siguiente ejercicio pueda continuar.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson] mientras Copilot añade una funcionalidad al código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
