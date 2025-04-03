# Laboratorio Práctico: Integración Continua

**Proyecto:** To Do List

**Actividad:** Agregar funcionalidad de "Mover tareas completadas a tareas pendientes" y configurar GitHub Actions para CI.

## Instrucciones Iniciales y Configuración de Firebase

1.  **Formar Equipos de 2 Personas:**

    -   Reúnanse en equipos de dos personas. Uno será el "Contributor" y el otro el "Reviewer".

2.  **Fork del Repositorio:**

    -   Cada miembro del equipo debe hacer un fork del repositorio [https://github.com/GaMaXLabs/todo-list](https://github.com/GaMaXLabs/todo-list) a su propia cuenta de GitHub.

3.  **Clonar el Repositorio Forjado:**

    -   Cada miembro del equipo debe clonar su propio repositorio forked a su máquina local.

    ```bash
    git clone [https://github.com/](https://github.com/)[tu-usuario-de-github]/todo-list.git
    cd todo-list
    ```

4.  **Configuración de Firebase Realtime Database (IMPORTANTE):**

    -   **Crear una Cuenta de Firebase:**
        -   Si aún no tienes una cuenta de Google, crea una.
        -   Ve a [https://console.firebase.google.com/](https://console.firebase.google.com/) e inicia sesión con tu cuenta de Google.

    -   **Crear un Nuevo Proyecto de Firebase:**
        -   Haz clic en "Agregar proyecto".
        -   Asigna un nombre a tu proyecto (por ejemplo, "todo-list-cicd-workshop").
        -   Sigue los pasos para configurar el proyecto. Puedes habilitar o deshabilitar Google Analytics según tus preferencias.
        -   Haz clic en "Crear proyecto".

    -   **Activar Realtime Database:**
        -   En el panel de navegación izquierdo de tu proyecto de Firebase, busca la sección "Desarrollar" y haz clic en "Realtime Database".
        -   Haz clic en "Crear base de datos".
        -   Selecciona "Iniciar en modo de prueba" (esto facilita la configuración inicial, pero recuerda que en producción debes configurar las reglas de seguridad).
        -   Elige la ubicación de tu base de datos y haz clic en "Habilitar".

    -   **Obtener la URL de la Base de Datos:**
        -   Una vez que se haya creado tu base de datos, verás la URL de la misma en la parte superior del panel de Realtime Database.
            -   **Ejemplo:** `https://tu-proyecto-default-rtdb.firebaseio.com/`
        -   Copia esta URL. La necesitarás en el siguiente paso.

    -   **Configurar el Archivo `.env`:**
        -   En la copia local de tu repositorio (la que clonaste en el paso 3), busca o crea el archivo `.env` en la raíz del proyecto.
        -   Abre el archivo `.env` y agrega la URL de tu base de datos de Firebase de la siguiente manera:

            ```
            REACT_APP_FIREBASE_RTDB_URL=[https://tu-proyecto-default-rtdb.firebaseio.com/](https://tu-proyecto-default-rtdb.firebaseio.com/)
            ```

            -   **IMPORTANTE:**
                * Reemplaza `https://tu-proyecto-default-rtdb.firebaseio.com/` con la URL que copiaste en el paso anterior.
                * **Para que `react-scripts` reconozca las variables de entorno, debes prefijarlas con `REACT_APP_`.**
                * Asegúrate de que no haya espacios adicionales alrededor del signo igual (=).

5.  **Instalar Dependencias (Si aún no lo has hecho):**

    -   En la terminal, dentro del directorio de tu proyecto, ejecuta el siguiente comando para instalar las dependencias:

        ```bash
        npm install
        ```

6.  **Ejecutar la Aplicación Localmente (Opcional, pero recomendado para verificar):**

    -   Para asegurarte de que todo está configurado correctamente, puedes ejecutar la aplicación localmente:

        ```bash
        npm start
        ```

    -   Abre tu navegador y ve a la URL que se muestra en la consola (normalmente `http://localhost:3000/`). Deberías ver la aplicación To Do List funcionando.

7.  **Revisión de la Estructura del Proyecto:**

    -   Tómense unos minutos para familiarizarse con la estructura del proyecto "To Do List".

8.  **Dar Permisos al Compañero de Equipo:**

    -   El "Contributor" debe agregar al "Reviewer" como colaborador en su repositorio forked.
        -   Ve a la pestaña "Settings" del repositorio en GitHub.
        -   Selecciona "Collaborators" y añade el nombre de usuario de GitHub del "Reviewer".

## Configuración de GitHub Actions para Integración Continua

9.  **Crear el Workflow de GitHub Actions:**

    -   En la raíz de tu repositorio, crea un directorio llamado `.github/workflows`.
    -   Dentro de este directorio, crea un archivo YAML para definir tu workflow de CI, por ejemplo, `ci.yml`.

10. **Configurar el archivo `ci.yml`:**

    -   Abre el archivo `ci.yml` y pega el siguiente código.  A continuación, se explicará cada sección:

    ```yaml
    name: Build and Test

    on:
      pull_request:
        branches:
          - main

    jobs:
      check:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout code
            uses: actions/checkout@v3
            with:
              fetch-depth: 0

          - name: Set up Node.js
            uses: actions/setup-node@v3
            with:
              node-version: 22

          - name: Install dependencies
            run: npm ci

          - name: Build Project
            run: npm run build

          - name: Run Tests
            run: npm test -- --reporters=jest-junit

          - name: Test Report
            uses: dorny/test-reporter@v2
            if: success() || failure()    
            with:
              path: junit.xml  
              reporter: jest-junit 
              name: JEST Tests
    ```

    -   **Explicación Detallada del Workflow:**

        -   `name: Build and Test`:
            -   **Paso:** Define el nombre del workflow.  Esto aparecerá en la interfaz de GitHub Actions.
            -   **Acción:** Nombra tu workflow de manera descriptiva. En este caso, indica claramente que el workflow se encarga de construir y testear el proyecto.

        -   `on:`
            -   **Paso:** Especifica los eventos que dispararán la ejecución del workflow.
            -   **Acción:**
                -   `pull_request:`:  Configura el workflow para que se ejecute automáticamente cuando se crea un pull request.
                -   `branches: - main`:  Limita la ejecución del workflow a los pull requests que tienen como destino la rama `main`.  Esto es importante para asegurar que solo los cambios que se van a integrar a la rama principal pasen por el proceso de CI.

        -   `jobs:`
            -   **Paso:** Define los trabajos (jobs) que componen el workflow. Un job es una serie de pasos que se ejecutan en un entorno virtual.
            -   **Acción:**
                -   `check:`:  Nombre del job.  Puedes nombrar tus jobs de acuerdo a su función.  En este caso, "check" sugiere que este job se encarga de verificar la calidad del código.
                -   `runs-on: ubuntu-latest`:  Especifica el tipo de máquina virtual donde se ejecutará el job. `ubuntu-latest` asegura que se use la última versión estable de Ubuntu.

        -   `steps:`
            -   **Paso:** Lista de pasos que se ejecutan secuencialmente dentro de un job.  Cada paso ejecuta una acción o un comando.
            -   **Acciones:**
                -   `- name: Checkout code`:
                    -   **Paso:** Describe lo que hace el paso.  Es importante poner nombres descriptivos para facilitar la lectura del workflow.
                    -   `uses: actions/checkout@v3`:  Utiliza la acción predefinida `actions/checkout@v3` para clonar el código del repositorio en la máquina virtual.  `@v3` especifica la versión de la acción.
                    -   `with: fetch-depth: 0`:  Configura la acción para realizar un checkout completo del historial del repositorio.  Esto puede ser necesario para algunas herramientas de análisis de código o tests que requieren acceso al historial.
                -   `- name: Set up Node.js`:
                    -   **Paso:** Configura el entorno de Node.js.
                    -   `uses: actions/setup-node@v3`:  Utiliza la acción `actions/setup-node@v3` para instalar Node.js.
                    -   `with: node-version: 22`:  Especifica la versión de Node.js que se va a utilizar.  **Asegúrate de que esta versión coincida con la que utiliza tu proyecto.**
                -   `- name: Install dependencies`:
                    -   **Paso:** Instala las dependencias del proyecto.
                    -   `run: npm ci`:  Ejecuta el comando `npm ci`.  `npm ci` es similar a `npm install`, pero está optimizado para entornos de CI.  Asegura una instalación limpia basada en el `package-lock.json`.  **Si tu proyecto no usa npm, ajusta este comando (ej. `yarn install`).**
                -   `- name: Build Project`:
                    -   **Paso:** Construye el proyecto.
                    -   `run: npm run build`:  Ejecuta el script `build` definido en el `package.json`.  Este comando compila el código (ej., de TypeScript a JavaScript, o para producción).  **Asegúrate de que este comando sea el correcto para tu proyecto.**
                -   `- name: Run Tests`:
                    -   **Paso:** Ejecuta los tests del proyecto.
                    -   `run: npm test -- --reporters=jest-junit`:  Ejecuta el script `test` definido en el `package.json`.  El argumento `-- --reporters=jest-junit` es específico de Jest (un framework de testing de JavaScript) y configura Jest para generar un reporte en formato JUnit, que es un estándar para reportes de tests.  **Si usas un framework de testing diferente, ajusta este comando y los argumentos.**
                -   `- name: Test Report`:
                    -   **Paso:** Genera un reporte de los tests.
                    -   `uses: dorny/test-reporter@v2`:  Utiliza la acción `dorny/test-reporter@v2` para procesar el reporte de tests JUnit.
                    -   `if: success() || failure()`:  Asegura que este paso se ejecute incluso si los tests fallan.  Esto es importante para obtener un reporte completo.
                    -   `with: path: junit.xml`:  Especifica la ruta al archivo del reporte JUnit generado por Jest.
                    -   `reporter: jest-junit`:  Indica el formato del reporte.
                    -   `name: JEST Tests`:  Asigna un nombre al reporte que aparecerá en la interfaz de GitHub Actions.

11. **Commit y Push del Workflow:**

    -   Agrega el archivo `ci.yml` a tu repositorio, haz commit y push a la rama `main`.

    ```bash
    git add .github/workflows/ci.yml
    git commit -m "feat: add CI with GitHub Actions"
    git push origin main
    ```

12. **Verificar la Ejecución del Workflow:**

    -   Ve a la pestaña "Actions" en tu repositorio de GitHub.
    -   Deberías ver la ejecución del workflow "Build and Test".
    -   Haz clic en el workflow para ver los detalles de cada paso y asegurarte de que todo pasa correctamente.  Si algún paso falla, puedes revisar los logs para identificar el problema.

## Actividades

### Contributor

13.  **Crear una Rama:**

    -   Crea una nueva rama para implementar la funcionalidad de "Mover tareas completadas a tareas pendientes".

    ```bash
    git checkout -b feature/restore-completed-tasks
    ```

14. **Implementar la Funcionalidad y Empujar los Cambios:**

    -   Modifica el código para agregar la funcionalidad de mover tareas completadas de regreso a la lista de tareas pendientes.
    -   Realiza los commits necesarios y sube los cambios a tu rama.
    -   **Edita el archivo:** `src/features/todos/components/todo-item/TodoItem.tsx`
    -   **Agrega el siguiente código:**

    ```tsx
    const handleRestoreClick = () => {
      dispatch(
        updateTodoAsync({
          ...props.todo,
          status: TodoStatus.active,
          completedDate: undefined,
        })
      );
      setAnchorEl(null);
    };
    ```

    -   **Agrega el siguiente código dentro del componente Menu:**

    ```tsx
    {
      props.todo.status === TodoStatus.completed && (
        <MenuItem onClick={handleRestoreClick}>
          Mover a tareas pendientes
        </MenuItem>
      );
    }
    ```

    ```bash
    # Realiza tus modificaciones...
    git add .
    git commit -m "feat: add restore completed tasks feature"
    git push origin feature/restore-completed-tasks
    ```

15. **Crear un Pull Request:**

    -   Ve a tu repositorio en GitHub y crea un pull request desde tu rama `feature/restore-completed-tasks` hacia la rama `main`.
    -   Asigna al "Reviewer" como revisor del pull request.

16. **Validar los Checks de Integración:**

    -   Observa los checks de integración en el pull request. Asegúrate de que todas las pruebas y builds pasen correctamente.  **Esto ahora incluye el workflow de GitHub Actions que configuraste.**

### Reviewer

17. **Revisar el Pull Request:**

    -   Abre el pull request que te asignaron.
    -   Revisa el código cuidadosamente, línea por línea.
    -   Haz comentarios constructivos y pide aclaraciones si es necesario.
    -   **Además, revisa que el workflow de GitHub Actions haya pasado correctamente.** Presta atención a los detalles del reporte de tests y a los logs de cada paso.

18. **Aprobar o Solicitar Cambios:**

    -   Si el código cumple con los estándares, funciona correctamente y el workflow de CI pasa, aprueba el pull request.
    -   Si encuentras problemas o mejoras, solicita cambios al "Contributor".

### Contributor

19. **Resolver Comentarios (si es necesario):**

    -   Si el "Reviewer" solicitó cambios, realiza las modificaciones necesarias y actualiza el pull request.
    -   Una vez que el "Reviewer" apruebe, puedes hacer merge del pull request a la rama `main`.

20. **Observar la Pipeline:**

    -   Revisa que la pipeline de CI/CD (GitHub Actions) se ejecute correctamente en la rama main, después de hacer merge de los cambios.