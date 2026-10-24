---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de empezar las lecciones de Copilot CLI, prepara todo lo necesario. Crearás tu propia copia del repositorio Tailspin Toys e iniciarás un [codespace][codespaces]. En la siguiente lección usarás su terminal integrada para instalar y ejecutar Copilot CLI.

## Configurar el repositorio del taller

Trabajarás con tu propia copia del proyecto Tailspin Toys. Créala ahora desde el [repositorio de plantilla][tailspin-template]. El nuevo repositorio incluye todos los archivos que necesita el taller.

1. En una ventana nueva del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Crea tu propia copia del repositorio seleccionando el botón **Use this template** en la página del repositorio del laboratorio. Después, selecciona **Create a new repository**.

    ![Captura del botón Use this template](../../../_images/ex0-use-template.png)

3. Si estás realizando el taller como parte de un evento dirigido por GitHub o Microsoft, sigue las instrucciones proporcionadas por el personal mentor. En caso contrario, puedes crear el nuevo repositorio en una organización en la que tengas acceso a GitHub Copilot.

    ![Captura de la configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (`organization-or-user-name/repository-name`), ya que la necesitarás más adelante en el taller.

> [!NOTE]
> **El backlog ya está listo**
>
> Al crear el repositorio desde la plantilla, se genera automáticamente un backlog de incidencias de GitHub. Trabajarás con estas incidencias durante el taller; no tienes que crear ninguna.

Usa una copia nueva de la plantilla del taller. Incluye instrucciones del repositorio, código de la aplicación, pruebas, la habilidad `quality-checks` y el backlog que utilizarás. Si usas una copia anterior, comprueba con la persona que facilita el taller que contiene los archivos necesarios.

## Crear un Codespace

A continuación, usarás un Codespace para completar el taller.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Ofrece un editor completo compatible con varios lenguajes de programación, extensiones y herramientas.

1. Ve a tu repositorio recién creado.
2. Selecciona el botón verde **Code**.

    ![Botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, **Create codespace on main**.

    ![Crear un codespace nuevo](../../../_images/ex0-create-codespace.png)

4. Espera a que termine la configuración del Codespace. La plantilla instala las dependencias del proyecto, Playwright Chromium y la base de datos local.
5. Si aparece el mensaje **Do you trust the authors of the files in this folder?**, selecciona **Trust Folder & Continue**.
6. Abre un terminal en la raíz del repositorio e inicia la aplicación:

   ```bash
   npm run dev
   ```

7. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser** y confirma que se carga el sitio Tailspin Toys.
8. Vuelve al terminal y detén el servidor de desarrollo con <kbd>Ctrl</kbd>+<kbd>C</kbd>.

> [!CAUTION]
> Volverás al Codespace en una lección posterior. Por ahora, déjalo abierto en una pestaña del navegador.

## Resumen

Ya tienes todo listo. En esta lección:

- creaste tu propia copia del proyecto Tailspin Toys a partir de la plantilla.
- creaste un Codespace y confirmaste que el proyecto estaba listo.

## Siguiente paso

A continuación, [instalarás GitHub Copilot CLI][next-lesson] en el Codespace y la autenticarás con tu cuenta de GitHub.

## Recursos

- [Descripción general de GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
