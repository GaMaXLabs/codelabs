# Laboratorio: ¡Mi Primera Contribución a GitHub! 🚀

## Objetivo

Aprender el flujo básico de contribución en GitHub: fork, clonar, crear una rama, modificar un archivo, crear un pull request y solicitar una revisión.

## Requisitos Previos

* Cuenta de GitHub. (Si no tienes una, sigue el paso 1).
* Git instalado en tu computadora.
* Editor de código (recomendamos Visual Studio Code).
* Terminal de Linux o WSL (Windows Subsystem for Linux) si estás en Windows.

## Pasos

### 1. Creación de Cuenta en GitHub (Si no tienes una)

* Sigue las instrucciones en el siguiente enlace: [Crear una cuenta en GitHub](https://docs.github.com/es/get-started/start-your-journey/creating-an-account-on-github#signing-up-for-a-new-personal-account)

### 2. Generación y Configuración de Claves SSH

* Para una conexión segura, genera llaves SSH y agrégalas a tu cuenta de GitHub: [Configurar llaves SSH](https://docs.github.com/es/authentication/connecting-to-github-with-ssh/managing-deploy-keys#set-up-deploy-keys)

### 3. Fork del Repositorio

* Navega a [https://github.com/GaMaXLabs/collaborative-timeline](https://github.com/GaMaXLabs/collaborative-timeline)
* Haz clic en el botón "Fork" en la esquina superior derecha para crear una copia del repositorio en tu cuenta.

### 4. Clonar tu Fork Localmente

* En tu repositorio forked, haz clic en el botón "Code" y copia la URL SSH.
* Abre tu terminal y navega al directorio donde quieres guardar el repositorio (por ejemplo, `~/repos`).
    ```bash
    mkdir repos
    cd repos
    ```
* Clona el repositorio usando la URL SSH copiada.
    ```bash
    git clone [https://www.ssh.com/academy/ssh/copy-id](https://www.ssh.com/academy/ssh/copy-id)
    ```

### 5. Abrir el Proyecto en Visual Studio Code

* Navega al directorio del repositorio clonado y abre VS Code.
    ```bash
    cd collaborative-timeline
    code .
    ```

### 6. Crear una Rama Local

* Crea una nueva rama para tus cambios, reemplazando `[nombre-de-la-rama]` con un nombre descriptivo (por ejemplo, `add-tu-nombre`).
    ```bash
    git checkout -b [nombre-de-la-rama]
    ```

### 7. Copiar y Renombrar el Archivo de Contribución

* Navega a `public/template` y copia el archivo `my_username.md`.
* Navega a `public/contributions` y pega el archivo copiado.
* Renombra el archivo a `[nombre-del-alumno].md`.

### 8. Modificar el Archivo de Contribución

* Abre `[nombre-del-alumno].md` en VS Code.
* Modifica el contenido del archivo:
    * Agrega tu emoji favorito.
    * Escribe una frase personal.
    * Guarda los cambios.

### 9. Preparar los Cambios para el Commit

* Agrega los cambios al área de preparación.
    ```bash
    git add -A
    ```

### 10. Crear un Commit

* Crea un commit con un mensaje descriptivo.
    ```bash
    git commit -m "[Mensaje de commit explicando tus cambios]"
    ```

### 11. Subir los Cambios a GitHub

* Sube tu rama al repositorio remoto.
    ```bash
    git push --set-upstream origin [nombre-de-la-rama]
    ```

### 12. Crear un Pull Request (PR)

* Navega a [https://github.com/GaMaXLabs/collaborative-timeline](https://github.com/GaMaXLabs/collaborative-timeline) en tu navegador.
* Ve a la pestaña "Pull requests" y haz clic en "New pull request".
* Asegúrate de que:
    * "base repository" sea `GaMaXLabs/collaborative-timeline`.
    * "base" sea `main`.
    * "head repository" sea `[tu-usuario]/collaborative-timeline`.
    * "compare" sea `[tu-rama]`.
* Revisa los cambios y haz clic en "Create pull request".
* Agrega un título y una descripción clara para tu PR.

### 13. Notificar la Revisión

* Informa a los ponentes para que revisen y aprueben tu PR.

¡Felicitaciones! Has realizado tu primera contribución a GitHub. 🎉👏🚀