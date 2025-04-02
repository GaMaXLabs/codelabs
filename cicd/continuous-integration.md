# Laboratorio Práctico: Integración Continua 

**Proyecto:** To Do List

**Actividad:** Cambiar el almacenamiento a Firebase Database Storage.

## Instrucciones Iniciales

1.  **Formar Equipos de 2 Personas:**
    * Reúnanse en equipos de dos personas. Uno será el "Contributor" y el otro el "Reviewer".

2.  **Fork del Repositorio:**
    * Cada miembro del equipo debe hacer un fork del repositorio [https://github.com/GaMaXLabs/todo-list](https://github.com/GaMaXLabs/todo-list) a su propia cuenta de GitHub.

3.  **Clonar el Repositorio Forjado:**
    * Cada miembro del equipo debe clonar su propio repositorio forked a su máquina local.

    ```bash
    git clone [https://github.com/](https://github.com/)[tu-usuario-de-github]/todo-list.git
    cd todo-list
    ```

4.  **Revisión de la Estructura del Proyecto:**
    * Tómense unos minutos para familiarizarse con la estructura del proyecto "To Do List" y el archivo de workflow de GitHub Actions.

5.  **Dar Permisos al Compañero de Equipo:**
    * El "Contributor" debe agregar al "Reviewer" como colaborador en su repositorio forked.
        * Ve a la pestaña "Settings" del repositorio en GitHub.
        * Selecciona "Collaborators" y añade el nombre de usuario de GitHub del "Reviewer".

## Actividades

### Contributor

6.  **Crear una Rama:**
    * Crea una nueva rama para implementar la funcionalidad de Firebase Database Storage.

    ```bash
    git checkout -b feature/firebase-database
    ```

7.  **Implementar la Funcionalidad y Empujar los Cambios:**
    * Modifica el código para cambiar el almacenamiento de la aplicación To Do List a Firebase Database Storage.
    * Realiza los commits necesarios y sube los cambios a tu rama.

    ```bash
    # Realiza tus modificaciones...
    git add .
    git commit -m "feat: cambia almacenamiento a Firebase Database"
    git push origin feature/firebase-database
    ```

8.  **Crear un Pull Request:**
    * Ve a tu repositorio en GitHub y crea un pull request desde tu rama `feature/firebase-database` hacia la rama `main`.
    * Asigna al "Reviewer" como revisor del pull request.

9.  **Validar los Checks de Integración:**
    * Observa los checks de integración en el pull request. Asegúrate de que todas las pruebas y builds pasen correctamente.

### Reviewer

10. **Revisar el Pull Request:**
    * Abre el pull request que te asignaron.
    * Revisa el código cuidadosamente, línea por línea.
    * Haz comentarios constructivos y pide aclaraciones si es necesario.

11. **Aprobar o Solicitar Cambios:**
    * Si el código cumple con los estándares y funciona correctamente, aprueba el pull request.
    * Si encuentras problemas o mejoras, solicita cambios al "Contributor".

### Contributor

12. **Resolver Comentarios (si es necesario):**
    * Si el "Reviewer" solicitó cambios, realiza las modificaciones necesarias y actualiza el pull request.
    * Una vez que el "Reviewer" apruebe, puedes hacer merge del pull request a la rama `main`.

13. **Observar la Pipeline:**
    * Revisa que la pipeline de CI/CD se ejecute correctamente en la rama main, despues de hacer merge de los cambios.

## Soporte en Tiempo Real

* Los instructores estarán disponibles para responder a tus preguntas y guiarte a través del laboratorio. ¡No dudes en pedir ayuda!