# Integración Continua

**Proyecto:** To Do List

**Actividad:** Agregar despliegue continuo a Firebase Hosting usando GitHub Actions.

## Instrucciones Iniciales

1.  **Clonar el Repositorio:**
    * Abre tu terminal y ejecuta el siguiente comando para clonar el repositorio:
    ```bash
    git clone [https://github.com/GaMaXLabs/todo-list.git](https://www.google.com/search?q=https://github.com/GaMaXLabs/todo-list.git)
    cd todo-list
    ```

2.  **Revisión de la Estructura del Proyecto:**
    * Tómate unos minutos para familiarizarte con la estructura del proyecto "To Do List". Observa los archivos HTML, CSS y JavaScript que componen la aplicación web.

3.  **Revisión del Archivo de Workflow de GitHub Actions:**
    * Explora el archivo `.github/workflows/firebase-deploy.yml` (o un nombre similar). Este archivo define el workflow de GitHub Actions que utilizaremos para automatizar el despliegue.

## Actividades

4.  **Crear una Nueva Rama:**
    * Crea una nueva rama para realizar tus modificaciones. Esto mantiene el `main` limpio hasta que tus cambios estén listos.
    ```bash
    git checkout -b feature/firebase-deploy
    ```

5.  **Personalización del Workflow:**
    * Abre el archivo `.github/workflows/firebase-deploy.yml` en tu editor de código.
    * Revisa y personaliza el workflow según las necesidades del proyecto. Presta atención a las siguientes secciones:
        * `on`: Define los eventos que activan el workflow (por ejemplo, `push` a la rama `main`).
        * `jobs`: Define las tareas que se ejecutarán en el workflow (por ejemplo, build, test, deploy).
        * `steps`: Define los comandos y acciones que se ejecutan en cada tarea (por ejemplo, instalar dependencias, ejecutar pruebas, desplegar a Firebase).
    * Asegúrate de configurar correctamente los pasos para la compilación de la aplicación web si es necesario (ej: npm run build).

6.  **Configuración del Despliegue a Firebase Hosting:**
    * Asegúrate de que el workflow incluya un paso para desplegar la aplicación a Firebase Hosting. Puedes usar la acción `FirebaseExtended/action-hosting-deploy@v0`.
    * Deberás configurar el token de Firebase en los "Secrets" de tu repositorio de GitHub:
        1.  Obtén tu token de firebase ejecutando en tu terminal: `firebase login:ci`.
        2.  En tu repositorio de github, navega a `Settings > Secrets > Actions > New repository secret`.
        3.  Agrega el token, la clave de este secret debe de llamarse `FIREBASE_TOKEN`
    * Revisa que el archivo de configuración `firebase.json` en tu repositorio este configurado correctamente, ejemplo:

    ```json
    {
      "hosting": {
        "public": "dist",
        "ignore": [
          "firebase.json",
          "**/.*",
          "**/node_modules/**"
        ],
        "rewrites": [
          {
            "source": "**",
            "destination": "/index.html"
          }
        ]
      }
    }
    ```

    * Recuerda reemplazar `"dist"` con el directorio de compilación de tu proyecto, si es necesario.

7.  **Agregar un cambio al repositorio:**
    * Realiza un cambio sencillo en tu aplicación, modifica por ejemplo el titulo del html principal.
    * Agrega los cambios al commit y subelos a la rama creada:
    ```bash
    git add .
    git commit -m "feat: cambio en el titulo principal"
    git push origin feature/firebase-deploy
    ```

8.  **Observar la Ejecución de la Pipeline:**
    * Ve a la pestaña "Actions" en tu repositorio de GitHub.
    * Observa cómo se ejecuta el workflow que has configurado. Podrás ver los logs de cada paso y verificar si el despliegue a Firebase fue exitoso.

9.  **Merge de los cambios a main:**
    * Una vez que estes seguro que todo esta funcionando correctamente, haz merge de los cambios a la rama main.
    * Esto disparara de nuevo el workflow, desplegando los cambios a producción.
    ```bash
    git checkout main
    git merge feature/firebase-deploy
    git push origin main
    ```

## Soporte en Tiempo Real

* Los instructores estarán disponibles para responder a tus preguntas y guiarte a través del laboratorio. ¡No dudes en pedir ayuda!