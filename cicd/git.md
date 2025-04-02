# Mi primer contribución

1. Crea una cuenta de Github siguiendo las instrucciones del siguiente enlace:
   https://docs.github.com/es/get-started/start-your-journey/creating-an-account-on-github#signing-up-for-a-new-personal-account

2. Generar las llaves SSH y agregarlas a la configuración de Github siguiendo las instrucciones:
   https://docs.github.com/es/authentication/connecting-to-github-with-ssh/managing-deploy-keys#set-up-deploy-keys
3. Navegar a https://github.com/GaMaXLabs/collaborative-timeline
4. Crear un nuevo fork del repositorio "collaborative-timeline".
5. Clonar el fork del repositorio localmente usando el botón de "Code" y especificando la opción SSH.
6. Abrir la terminal de Linux (o WSL)
7. Navegar al directorio del usuario con:

```bash
  cd ~
```

8. Crear un directorio llamado "repos":

```bash
  mkdir repos
```

9. Navegar al directorio "repos" e ingresar el comando:

```bash
  git clone git@github.com:[usuario]/collaborative-timeline.git
```

10. Navegar al directorio "collaborative-timeline" y abrir Visual Studio Code con el comando:

```bash
  code .
```

11. Crear una rama local basada en la rama "main" con el comando:

```bash
  git checkout -b [nombre-de-la-rama]
```

12. Navegar al directorio "public/template" y copiar el archivo "my_username.md"
13. Navegar al directorio "public/contributions" y pegar el archivo "my_username.md"
14. Renombrar el archivo "my_username.md" a "[nombre-del-alumno].md".
15. Modificar el contenido del archivo "[nombre-del-alumno].md", ingresando el emoji favorito y una frase a elegir.
16. Guardar cambios.
17. Agregar cambios locales al area de preparación con el comando:

```bash
  git add -A
```

18. Crear un commit con un mensaje con el commando:

```bash
  git commit -m "[Mensaje de commit a elegir]"
```

19. Empujar cambios a el repositorio remoto (en GitHub)

```bash
  git push --set-upstream origin [nombre-de-la-rama]
```

20. Navegar a https://github.com/GaMaXLabs/collaborative-timeline
21. Ir al menú "Pull Requests"
22. Hacer click en el botón "Nuevo Pull Request"
23. Seleccionar repositorio base: "GaMaXLabs/collaborative-timeline"
24. Seleccionar la rama base: "main"
25. Seleccionar el repositorio head: [nombre-de-usuario]/collaborative-timeline
26. Seleccionar rama de comparación: [nombre-de-la-rama]
27. Verificar cambios a unir.
28. Hacer click en el botón "Crear Pull Request"

Notifica a los ponentes para que revisen y aprueben los cambios propuestos.
Has publicado tu nueva propuesta de cambio al repositorio. ¡Felicidades! 🚀👏
