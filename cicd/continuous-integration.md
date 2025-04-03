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

## Actividades

### Contributor

9.  **Crear una Rama:**

    -   Crea una nueva rama para implementar la funcionalidad de "Mover tareas completadas a tareas pendientes".

    ```bash
    git checkout -b feature/restore-completed-tasks
    ```

10. **Implementar la Funcionalidad y Empujar los Cambios:**

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

11. **Crear un Pull Request:**

    -   Ve a tu repositorio en GitHub y crea un pull request desde tu rama `feature/restore-completed-tasks` hacia la rama `main`.
    -   Asigna al "Reviewer" como revisor del pull request.

12. **Validar los Checks de Integración (Antes de Configurar CI):**

    -   **Antes de configurar GitHub Actions, observa el estado actual de tu proyecto.**
    -   Ve a la pestaña "Actions" de tu repositorio.  Verás que no hay workflows personalizados definidos aún.  Esto significa que solo los checks por defecto de GitHub (si los hay) se ejecutarán al crear el pull request.
    -   **Nota:** Es posible que veas algunos checks básicos relacionados con la capacidad de hacer merge, pero no tests automáticos ni builds.

### Reviewer

13. **Revisar el Pull Request (Antes de Configurar CI):**

    -   Abre el pull request que te asignaron.
    -   Revisa el código cuidadosamente, línea por línea.
    -   Haz comentarios constructivos y pide aclaraciones si es necesario.
    -   **En este punto, la revisión se centra principalmente en la lógica y la calidad del código, ya que aún no hemos automatizado las verificaciones.**

14. **Aprobar o Solicitar Cambios:**

    -   Si el código cumple con los estándares y funciona correctamente (según la revisión manual), aprueba el pull request.
    -   Si encuentras problemas o mejoras, solicita cambios al "Contributor".

### Contributor

15. **Resolver Comentarios (si es necesario):**

    -   Si el "Reviewer" solicitó cambios, realiza las modificaciones necesarias y actualiza el pull request.
    -   Una vez que el "Reviewer" apruebe, **NO HAGAS MERGE TODAVÍA**.  Vamos a configurar GitHub Actions primero.

## Configuración de GitHub Actions para Integración Continua

16. **Crear el Workflow de GitHub Actions:**

    -   Ahora, después de haber creado el pull request y haber tenido la experiencia de revisarlo (sin CI), vamos a configurar GitHub Actions para automatizar este proceso.
    -   En la raíz de tu repositorio, crea un directorio llamado `.github/workflows`.
    -   Dentro de este directorio, crea un archivo YAML para definir tu workflow de CI, por ejemplo, `ci.yml`.

17. **Configurar el archivo `ci.yml` con explicaciones paso a paso:**

    -   Abre el archivo `ci.yml` y agrega el siguiente contenido:

    ```yaml
    name: Build and Test

    on:
      pull_request:
        branches:
          - main
      push:
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

    A continuación tienes el mismo script pero con comentarios para explicar el funcionamiento:

    ```yaml
    # 1.  Define el nombre del workflow.  Esto aparecerá en la interfaz de GitHub Actions.
    name: Build and Test

    # 2. Especifica los eventos que dispararán la ejecución del workflow.
    on:
      pull_request: # Configura el workflow para que se ejecute automáticamente cuando se crea un pull request.
        branches:
          - main # Limita la ejecución del workflow a los pull requests que tienen como destino la rama `main`.
      push:
        branches:
          - main

    # 3. Define los trabajos (jobs) que componen el workflow. Un job es una serie de pasos que se ejecutan en un entorno virtual.
    jobs:
      check: # Nombre del job.  Puedes nombrar tus jobs de acuerdo a su función.
        runs-on: ubuntu-latest # Especifica el tipo de máquina virtual donde se ejecutará el job. `ubuntu-latest` asegura que se use la última versión estable de Ubuntu.
        # 4. Lista de pasos que se ejecutan secuencialmente dentro de un job.  Cada paso ejecuta una acción o un comando.
        steps:
          # 4.1  Clona el código del repositorio en la máquina virtual.
          - name: Checkout code # Describe lo que hace el paso.
            uses: actions/checkout@v3 # Utiliza la acción predefinida `actions/checkout@v3` para clonar el código. `@v3` especifica la versión de la acción.
            with:
              fetch-depth: 0 # Configura la acción para realizar un checkout completo del historial del repositorio.

          # 4.2  Configura el entorno de Node.js.
          - name: Set up Node.js
            uses: actions/setup-node@v3 # Utiliza la acción `actions/setup-node@v3` para instalar Node.js.
            with:
              node-version: 22 # Especifica la versión de Node.js que se va a utilizar.  Asegúrate de que esta versión coincida con la que utiliza tu proyecto.
          # 4.3 Instala las dependencias del proyecto.
          - name: Install dependencies
            run: npm ci # Ejecuta el comando `npm ci`.  `npm ci` está optimizado para entornos de CI.  Asegura una instalación limpia basada en el `package-lock.json`.  Si tu proyecto no usa npm, ajusta este comando (ej. `yarn install`).
          # 4.4 Construye el proyecto.
          - name: Build Project
            run: npm run build # Ejecuta el script `build` definido en el `package.json`.  Este comando compila el código (ej., de TypeScript a JavaScript, o para producción).  Asegúrate de que este comando sea el correcto para tu proyecto.
          # 4.5 Ejecuta los tests del proyecto.
          - name: Run Tests
            run: npm test -- --reporters=jest-junit # Ejecuta el script `test` definido en el `package.json`.  El argumento `-- --reporters=jest-junit` es específico de Jest y configura Jest para generar un reporte en formato JUnit.  Si usas un framework de testing diferente, ajusta este comando y los argumentos.

          # 4.6 Genera un reporte de los tests.
          - name: Test Report
            uses: dorny/test-reporter@v2 # Utiliza la acción `dorny/test-reporter@v2` para procesar el reporte de tests JUnit.
            if: success() || failure() # Asegura que este paso se ejecute incluso si los tests fallan.
            with:
              path: junit.xml # Especifica la ruta al archivo del reporte JUnit generado por Jest.
              reporter: jest-junit # Indica el formato del reporte.
              name: JEST Tests # Asigna un nombre al reporte que aparecerá en la interfaz de GitHub Actions.
    ```

18. **Commit y Push del Workflow:**

    -   Agrega el archivo `ci.yml` a tu repositorio, haz commit y push a la rama `main`.

    ```bash
    git add .github/workflows/ci.yml
    git commit -m "feat: add CI with GitHub Actions"
    git push origin main
    ```

19. **Verificar la Ejecución del Workflow:**

    -   Ve a la pestaña "Actions" en tu repositorio de GitHub.
    -   Deberías ver la ejecución del workflow "Build and Test".
    -   Haz clic en el workflow para ver los detalles de cada paso y asegurarte de que todo pasa correctamente. Si algún paso falla, puedes revisar los logs para identificar el problema.

20. **Validar los Checks de Integración (Después de Configurar CI):**

    -   Ahora, vuelve a tu pull request.  **Observa la diferencia!**
    -   Deberías ver los checks de GitHub Actions ejecutándose.
    -   Asegúrate de que todas las pruebas y builds pasen correctamente.  **Esto ahora incluye el workflow de GitHub Actions que configuraste.**

### Reviewer

21. **Revisar el Pull Request (Después de Configurar CI):**

    -   Abre el pull request que te asignaron.
    -   Revisa el código cuidadosamente, línea por línea.
    -   Haz comentarios constructivos y pide aclaraciones si es necesario.
    -   **Ahora, la revisión incluye la validación de los resultados de GitHub Actions.** Presta atención a los detalles del reporte de tests y a los logs de cada paso.

22. **Aprobar o Solicitar Cambios:**

    -   Si el código cumple con los estándares, funciona correctamente y el workflow de CI pasa, aprueba el pull request.
    -   Si encuentras problemas o mejoras, solicita cambios al "Contributor".

### Contributor

23. **Resolver Comentarios (si es necesario):**

    -   Si el "Reviewer" solicitó cambios, realiza las modificaciones necesarias y actualiza el pull request.
    -   Una vez que el "Reviewer" apruebe, puedes hacer merge del pull request a la rama `main`.

24. **Observar la Pipeline (Final - Después del Merge):**

    -   Después de hacer merge, ve a la pestaña "Actions" y observa la ejecución del workflow en la rama `main`.
    -   Esto confirma que la integración continua sigue funcionando para la rama principal, asegurando que los nuevos cambios no rompan la aplicación.