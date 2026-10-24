---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que documentara el código, creara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera probarla e implementara mejoras de accesibilidad, incluidos los modos de alto contraste y claro. Revisaremos los cambios y, si hace falta, enviaremos comentarios a Copilot para que los mejore.

### Escenario

Como hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian con la llegada de la IA generativa. Siempre debemos revisar el código generado y seguir el proceso normal de DevOps. Revisaremos las propuestas de GitHub Copilot para crear documentación, añadir los juegos relacionados y mejorar la accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube trabaja de forma asíncrona y sin supervisión, se han establecido algunas medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente se ejecuta en GitHub Actions, donde crea un entorno separado y efímero para trabajar.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) creada por el agente de Copilot en la nube: añadir documentación al código. Usaremos la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al abrir la PR, quizá veas una advertencia de que Copilot está bloqueado por un firewall. Es **lo esperado**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente de Copilot en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot aún trabaja en la tarea, la PR tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, incluidos los comentarios TSDoc recién creados y el resto de la documentación. Los cambios concretos pueden variar. Busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían terminar correctamente para la aplicación Astro. Esto puede tardar unos minutos.

## Pedir cambios a GitHub Copilot

La colaboración con Copilot en una PR no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios sobre líneas concretas del código, igual que harías con otra persona del equipo. Copilot verá los comentarios e iniciará otra sesión para atenderlos. Como los resultados no son deterministas, no hay un texto único que debas usar. Por ejemplo, puedes pedirle que:

- añada encabezados breves al principio de cada archivo de código para describir su función.
- añada comentarios TSDoc a archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario en el que pidas un cambio a la documentación generada y menciona a **@copilot**, igual que mencionarías a cualquier usuario. Usa una de las ideas anteriores u otra sugerencia de documentación.
2. Selecciona **View Session** para observar el trabajo. Copilot iniciará otra sesión para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la PR.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás una confirmación nueva en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta quedar satisfecho. Después, puedes marcar la PR como lista para revisión y combinarla con la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Volvamos a la PR que Copilot creó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a **Conversation** y desplázate hacia abajo.
6. Busca el indicador de flujos de trabajo pendientes de aprobación.
7. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos deberían aparecer en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían terminar correctamente para la aplicación Astro. Esto puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en el Codespace para probar manualmente los juegos relacionados. Abre el terminal del Codespace y ejecuta estos comandos, sustituyendo `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes iterar y pedir actualizaciones. Por ejemplo, puedes ajustar el número de juegos relacionados o pedir que añada encabezados y comentarios TSDoc (recuerda: la tarea se asignó **antes** de actualizar las instrucciones personalizadas). Para solicitar cambios, añade otro comentario en la pestaña **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisar las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad que se implementaron con el agente personalizado de accesibilidad. Esta PR debe incluir tanto el modo de alto contraste que asignaste en el ejercicio 3 como el modo claro solicitado desde el panel de agentes en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía trabaja en la tarea, la PR tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para alternar entre modos.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.).
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca el indicador de flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían terminar correctamente para la aplicación Astro. Esto puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en el Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos, sustituyendo `<branch-name>` por la rama que creó Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Confirma que funcionan como esperas y que la configuración se conserva al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a Copilot a aplicar prácticas recomendadas. Si detectas problemas de accesibilidad o mejoras posibles, menciona a **@copilot** en un comentario para solicitar cambios, como hiciste con las otras PR.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta especialmente útil cuando puedes asignarle incidencias reales del backlog y dedicarte a otra cosa. Para crear este hábito, registra algunas incidencias más y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para expresar interés en la página de detalles del juego.
- implementar la paginación de la lista de juegos.
- añadir validación de entradas y gestión de errores a los asistentes de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizados incluidos en el repositorio** para observar después su efecto en el trabajo del agente.
- **Asignaste incidencias al agente de Copilot en la nube** y observaste cómo configuraba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y usaste un agente personalizado** de accesibilidad para añadir modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como panel de control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Revisaste e iteraste sobre las solicitudes de incorporación de cambios del agente**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Revisión y próximos pasos

Has terminado el recorrido del agente en la nube. Si quieres seguir explorando, los demás recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración de MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y una integración opcional con `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
