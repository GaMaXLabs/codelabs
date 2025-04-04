#   Laboratorio Práctico: Despliegue continuo

**Proyecto:** To Do List

**Actividad:** Configurar un workflow de GitHub Actions dedicado para el despliegue continuo a Firebase Hosting.

**Pre-requisito:** Se asume que has completado exitosamente el laboratorio de Integración Continua. Este laboratorio continúa donde lo dejaste. Si no tienes ese avance, puedes usar la rama `lab/cicd/add-ci-pipeline` que contiene el resultado final del laboratorio de Integración Continua.

##   Instrucciones Iniciales

1.  **Clonar el Repositorio (o Continuar con el Existente):**

    * Si estás continuando desde el laboratorio de Integración Continua, puedes seguir trabajando con el repositorio que ya tienes clonado.
    * Si necesitas comenzar desde el punto final del laboratorio de Integración Continua, clona el repositorio y luego haz checkout a la rama `lab/cicd/add-ci-pipeline`:

        ```bash
        git clone [https://github.com/GaMaXLabs/todo-list](https://github.com/GaMaXLabs/todo-list)
        cd todo-list
        git checkout lab/cicd/add-ci-pipeline
        ```

2.  **Revisión de la Estructura del Proyecto:**

    * Tómate unos minutos para familiarizarte con la estructura del proyecto "To Do List". Observa los archivos HTML, CSS y JavaScript que componen la aplicación web.

3.  **Revisión de los Archivos de Workflow de GitHub Actions (si existen):**

    * Explora los archivos en el directorio `.github/workflows/`. Puede que ya exista un archivo de CI (`ci.yml`) del laboratorio anterior. Ahora crearemos uno nuevo para CD.

##   Actividades

4.  **Crear una Nueva Rama:**

    * Crea una nueva rama para realizar tus modificaciones. Esto mantiene el `main` limpio hasta que tus cambios estén listos.

        ```bash
        git checkout -b feature/firebase-cd
        ```

5.  **Configurar el Archivo `.firebaserc`:**

    * **IMPORTANTE:** Dado que creaste tu propio proyecto de Firebase en el laboratorio de Integración Continua, es crucial que el archivo `.firebaserc` apunte a tu proyecto específico.
    * Abre el archivo `.firebaserc` en tu editor de código. Deberías ver una estructura similar a esta:

        ```json
        {
          "projects": {
            "default": "tu-proyecto-aqui"
          }
        }
        ```

    * Reemplaza `"tu-proyecto-aqui"` con el **ID de tu proyecto de Firebase**. Puedes encontrar el ID de tu proyecto en la consola de Firebase.
    * Guarda el archivo.

6.  **Crear el Workflow de Despliegue (`cd.yml`):**

    * En el directorio `.github/workflows/`, crea un nuevo archivo llamado `cd.yml`.
    * Agrega el siguiente contenido a `cd.yml`:

        ```yaml
        name: Deploy to Firebase Hosting on merge

        on:
          push:
            branches:
              - main

        jobs:
          build_and_deploy:
            runs-on: ubuntu-latest
            steps:
              - uses: actions/checkout@v4
              - run: echo "REACT_APP_FIREBASE_RTDB_URL=${{ secrets.REACT_APP_FIREBASE_RTDB_URL }}" >> $GITHUB_ENV
              - run: npm ci && npm run build
              - uses: FirebaseExtended/action-hosting-deploy@v0
                with:
                {% raw %}
                  repoToken: '${{ secrets.GITHUB_TOKEN }}'
                  firebaseServiceAccount: '${{ secrets.FIREBASE_SERVICE_ACCOUNT }}'
                  channelId: live
                  projectId: #[firebase_project_id] # ¡Reemplaza con tu Project ID!
                  target: app
                {% endraw %}
        ```

    * **IMPORTANTE:**
        * Reemplaza `#[firebase_project_id]` con el **ID de tu proyecto de Firebase**. Asegúrate de que coincida con el que usaste en el `.firebaserc`.
        * Este workflow se activará automáticamente cuando se haga push a la rama `main`.
        {% raw %}
        * El paso `echo "REACT_APP_FIREBASE_RTDB_URL=${{ secrets.REACT_APP_FIREBASE_RTDB_URL }}" >> $GITHUB_ENV` es crucial para pasar la URL de la base de datos de Firebase a tu entorno de construcción.
        {% endraw %}

7.  **Configurar los Secretos de GitHub Actions:**

    * Para que GitHub Actions pueda desplegar a tu proyecto de Firebase, necesitas configurar los siguientes secretos:
        * `FIREBASE_SERVICE_ACCOUNT`: Contiene la clave privada de una cuenta de servicio de Firebase.
        * `GITHUB_TOKEN`: Este token lo proporciona automáticamente GitHub Actions y se usa para acceder al repositorio. Ya lo estamos usando en el yml.
        * `REACT_APP_FIREBASE_RTDB_URL`: Contiene la URL de tu base de datos de Firebase Realtime Database.

8.  **Obtener la Clave de la Cuenta de Servicio de Firebase:**

    * En la consola de Firebase, ve a tu proyecto.
    * Ve a "Configuración del proyecto" (el ícono del engranaje).
    * Selecciona "Cuentas de servicio".
    * Haz clic en "Generar nueva clave privada".
    * Selecciona "Firebase Admin SDK" y haz clic en "Generar clave".
    * Se descargará un archivo JSON con la clave privada. **¡Guarda este archivo en un lugar seguro!** No lo compartas ni lo subas a tu repositorio.
    * Abre el archivo JSON en un editor de texto y copia todo su contenido.

9.  **Agregar el Secreto `FIREBASE_SERVICE_ACCOUNT` a GitHub:**

    * En tu repositorio de GitHub, ve a "Settings" (Configuración).
    * Selecciona "Security" y luego "Secrets and variables" y "Actions".
    * Haz clic en "New repository secret".
    * En el campo "Name", escribe `FIREBASE_SERVICE_ACCOUNT`.
    * En el campo "Value", pega el contenido del archivo JSON que copiaste en el paso anterior.
    * Haz clic en "Add secret".

10. **Agregar el Secreto `REACT_APP_FIREBASE_RTDB_URL` a GitHub:**

    * En tu repositorio de GitHub, ve a "Settings" (Configuración).
    * Selecciona "Security" y luego "Secrets and variables" y "Actions".
    * Haz clic en "New repository secret".
    * En el campo "Name", escribe `REACT_APP_FIREBASE_RTDB_URL`.
    * En el campo "Value", pega la URL de tu base de datos de Firebase Realtime Database. **(¡Asegúrate de que sea la misma que usaste en el archivo `.env` en el laboratorio de CI! )**
    * Haz clic en "Add secret".

11. **Personalización del Workflow (Opcional):**

    * Revisa el archivo `cd.yml` y comprende cada sección:
        * `on`: Define el evento que dispara el workflow (push a `main`).
        * `jobs`: Define el trabajo a realizar (`build_and_deploy`).
        * `steps`: Define los pasos dentro del trabajo (checkout, configuración de variables de entorno, build, deploy).
    * Si tu proyecto tiene un comando de build diferente a `npm run build`, ajústalo.

12. **Agregar y Commitear los Cambios:**

    * Agrega el nuevo archivo `cd.yml` a tu repositorio, haz commit y push a la rama `feature/firebase-cd`.

        ```bash
        git add .github/workflows/cd.yml
        git commit -m "feat: add Firebase CD workflow and secrets"
        git push origin feature/firebase-cd
        ```

13. **Crear un Pull Request:**

    * Crea un pull request desde tu rama `feature/firebase-cd` hacia la rama `main`.

14. **Observar la Ejecución del Workflow:**

    * Ve a la pestaña "Actions" en tu repositorio de GitHub.
    * Después de hacer merge a `main`, observa cómo se ejecuta el workflow `Deploy to Firebase Hosting on merge`.
    * Verifica que el despliegue a Firebase Hosting sea exitoso.

15. **Merge de los Cambios a `main`:**

    * Una vez que hayas verificado que el workflow se ejecuta correctamente y el despliegue es exitoso, haz merge de tu pull request a la rama `main`.
    * Esto activará automáticamente el workflow de CD y desplegará tu aplicación a Firebase Hosting.

        ```bash
        git checkout main
        git merge feature/firebase-cd
        git push origin main
        ```

16. **Verificar el Despliegue:**

    * Ve a la consola de Firebase y verifica que tu aplicación se haya desplegado correctamente en Firebase Hosting.

##   Soporte en Tiempo Real

* Los instructores estarán disponibles para responder a tus preguntas y guiarte a través del laboratorio. ¡No dudes en pedir ayuda!