# JoomlaOnDocker
Creación de contenedor Docker con Joomla e instalación de este

## Arranque de máquina docker
Iniciamos sesión, pasamos a modo superusuario.
```
sudo su
```
Comprobamos que imágenes tenemos y la versión de nuestro Docker
```
docker images
```
```
docker --version 
```
![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/b046c829-4fce-4086-a9a7-6d2a4797c6a1)


## Descarga de Joomla
Buscamos cual es el comando para la descarga de la imagen de Joomla y la descargamos 

![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/3a911a74-ea4b-4c4b-b87d-946199993f48)

```
docker pull Joomla
```
Comprobamos que se haya descargado
```
docker images
```

![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/601c51d4-a3a9-4fc2-9586-cafcb34ab590)

## Creación de base de datos
Creamos una base de datos, por ejemplo "JoomlaGuia", en este caso he utilizado la interfaz gráfica de Workbech sobre un servidor ya existente.

![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/839f9419-34ae-4bf6-a39a-91ef11ed4966)

También debemos crear un usuario con todos los permisos para esa base de datos, por ejemplo "joomlaguia".

![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/1767ac97-5b6f-4d74-adb9-3c396701262c)

## Crear contenedor docker de joomla
Comprobamos los contenedores que tenemos activos con:
```
docker ps
```
![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/2586f396-e625-4a89-94ba-46f3ea3b0def)

Creamos un nuevo contenedor con el comando run:
```
docker run -it -d --name ContenedorJoomla -e JOOMLA_DB_HOST=10.33.4.130 -e JOOMLA_DB_USER=joomlauser -e "JOOMLA_DB_PASSWORD=jorgecm" -e "JOOMLA_DB_NAME=JoomlaGuia" -p 8182:80 joomla
```
![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/a2d0549b-00e3-488a-a546-8829d2dbb067)

Esto nos devolverá un id, es el id de nuestro contenedor, a partir de ahora, con mencionar los 3-4 primeros caracteres, será suficiente para referenciarlo.
Volvemos a comprobar que se ha creado correctamente y está activo.
```
docker ps
```

## POSIBLES ERRORES

En caso de error, el comando devolverá un id igualmente, pero si hacemos "docker ps" no saldrá nada.
![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/628c0e45-4944-433f-9354-bf4fe174cac3)

Debemos usar "docker ps -a" que nos permite ver los contenedores pausados. Si usamos el comando "docker logs NombreContenedor" nos permite ver el posible error.
![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/a1680e96-0378-4e3b-83ca-cf286d7a3d78)

Puede darse el caso también de que el contenedor se esté ejecutando y de un error al hacer el exec, esto es porque la máquina arranca, pero no del todo, con el mismo comando de antes podemos ver posibles errores o si ha arrancado correctamente.
