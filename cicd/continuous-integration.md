# Laboratorio Práctico: Integración Continua

**Proyecto:** To Do List

**Actividad:** Agregar funcionalidad de "Mover tareas completadas a tareas pendientes".

## Instrucciones Iniciales

1.  **Formar Equipos de 2 Personas:**

    - Reúnanse en equipos de dos personas. Uno será el "Contributor" y el otro el "Reviewer".

2.  **Fork del Repositorio:**

    - Cada miembro del equipo debe hacer un fork del repositorio [https://github.com/GaMaXLabs/todo-list](https://github.com/GaMaXLabs/todo-list) a su propia cuenta de GitHub.

3.  **Clonar el Repositorio Forjado:**

    - Cada miembro del equipo debe clonar su propio repositorio forked a su máquina local.

    ```bash
    git clone https://github.com/[tu-usuario-de-github]/todo-list.git
    cd todo-list
    ```

4.  **Revisión de la Estructura del Proyecto:**

    - Tómense unos minutos para familiarizarse con la estructura del proyecto "To Do List" y el archivo de workflow de GitHub Actions.

5.  **Dar Permisos al Compañero de Equipo:**
    - El "Contributor" debe agregar al "Reviewer" como colaborador en su repositorio forked.
      - Ve a la pestaña "Settings" del repositorio en GitHub.
      - Selecciona "Collaborators" y añade el nombre de usuario de GitHub del "Reviewer".

## Actividades

### Contributor

6.  **Crear una Rama:**

    - Crea una nueva rama para implementar la funcionalidad de "Mover tareas completadas a tareas pendientes".

    ```bash
    git checkout -b feature/restore-completed-tasks
    ```

7.  **Implementar la Funcionalidad y Empujar los Cambios:**

    - Modifica el código para agregar la funcionalidad de mover tareas completadas de regreso a la lista de tareas pendientes.
    - Realiza los commits necesarios y sube los cambios a tu rama.
    - **Edita el archivo:** `src/features/todos/components/todo-item/TodoItem.tsx`
    - **Agrega el siguiente código:**

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

    - **Agrega el siguiente código dentro del componente Menu:**

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

8.  **Crear un Pull Request:**

    - Ve a tu repositorio en GitHub y crea un pull request desde tu rama `feature/restore-completed-tasks` hacia la rama `main`.
    - Asigna al "Reviewer" como revisor del pull request.

9.  **Validar los Checks de Integración:**
    - Observa los checks de integración en el pull request. Asegúrate de que todas las pruebas y builds pasen correctamente.

### Reviewer

10. **Revisar el Pull Request:**

    - Abre el pull request que te asignaron.
    - Revisa el código cuidadosamente, línea por línea.
    - Haz comentarios constructivos y pide aclaraciones si es necesario.

11. **Aprobar o Solicitar Cambios:**
    - Si el código cumple con los estándares y funciona correctamente, aprueba el pull request.
    - Si encuentras problemas o mejoras, solicita cambios al "Contributor".

### Contributor

12. **Resolver Comentarios (si es necesario):**

    - Si el "Reviewer" solicitó cambios, realiza las modificaciones necesarias y actualiza el pull request.
    - Una vez que el "Reviewer" apruebe, puedes hacer merge del pull request a la rama `main`.

13. **Observar la Pipeline:**
    - Revisa que la pipeline de CI/CD se ejecute correctamente en la rama main, después de hacer merge de los cambios.

