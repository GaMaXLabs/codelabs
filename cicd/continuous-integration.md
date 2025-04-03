# Laboratorio Práctico: Integración Continua

**Proyecto:** To Do List

**Actividad:** Agregar funcionalidad de "Mover tareas completadas a tareas pendientes".

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

    -   Tómense unos minutos para familiarizarse con la estructura del proyecto "To Do List" y el archivo de workflow de GitHub Actions.

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

12. **Validar los Checks de Integración:**

    -   Observa los checks de integración en el pull request. Asegúrate de que todas las pruebas y builds pasen correctamente.

### Reviewer

13. **Revisar el Pull Request:**

    -   Abre el pull request que te asignaron.
    -   Revisa el código cuidadosamente, línea por línea.
    -   Haz comentarios constructivos y pide aclaraciones si es necesario.

14. **Aprobar o Solicitar Cambios:**

    -   Si el código cumple con los estándares y funciona correctamente, aprueba el pull request.
    -   Si encuentras problemas o mejoras, solicita cambios al "Contributor".

### Contributor

15. **Resolver Comentarios (si es necesario):**

    -   Si el "Reviewer" solicitó cambios, realiza las modificaciones necesarias y actualiza el pull request.
    -   Una vez que el "Reviewer" apruebe, puedes hacer merge del pull request a la rama `main`.

16. **Observar la Pipeline:**

    -   Revisa que la pipeline de CI/CD se ejecute correctamente en la rama main, después de hacer merge de los cambios.