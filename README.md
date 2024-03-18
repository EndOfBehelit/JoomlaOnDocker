# **JoomlaOnDocker**
Creación de contenedor Docker con Joomla y configuración del sitio.
- [JoomlaOnDocker](#joomlaondocker)
  - [Arranque de máquina docker](#arranque-de-máquina-docker)

  - [Creación de imagen docker](#creación-de-imagen-docker)





## **Arranque de máquina docker**
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


## **Descarga de Joomla**
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

## **Creación de base de datos**
Creamos una base de datos, por ejemplo `JoomlaGuia`, en este caso he utilizado la interfaz gráfica de Workbech sobre un servidor ya existente.

![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/839f9419-34ae-4bf6-a39a-91ef11ed4966)

También debemos crear un usuario con todos los permisos para esa base de datos, por ejemplo `joomlaguia`.

![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/1767ac97-5b6f-4d74-adb9-3c396701262c)

## **Crear contenedor docker de joomla**
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

## **POSIBLES ERRORES**

En caso de error, el comando devolverá un id igualmente, pero si hacemos `docker ps` no saldrá nada.

![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/628c0e45-4944-433f-9354-bf4fe174cac3)

Debemos usar `docker ps -a` que nos permite ver los contenedores pausados. Si usamos el comando `docker logs NombreContenedor` nos permite ver el posible error.
```
docker logs ContenedorJoomla
```

![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/a1680e96-0378-4e3b-83ca-cf286d7a3d78)

Puede darse el caso también de que el contenedor se esté ejecutando y de un error al hacer el `exec`, esto es porque la máquina arranca, pero no del todo, con el mismo comando de antes podemos ver posibles errores o si ha arrancado correctamente.

## **Acceso al contenedor**

```
docker exec -it 0 bash
```
Importante cambiar el "0" por las siglas de cada contenedor, si no hay ningún otro que empiece por 0 con un caracter es suficiente.

![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/f16e761f-1873-405f-8780-c6f6db880224)


## **Configuración del contenedor Joomla**
  
  * **Instalación de nano, systemctl, unzip, y wget** <br>
    Como estamos en docker las imágenes vienen con los recursos mínimos, necesitamos estos servicios:
    * **Nano**
        ```
          apt-get install nano
        ```
      ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/42c9401a-f24b-4142-8a0f-273fe61d8ea5)


    * **Systemctl**
        ```
          apt-get install systecmtl
        ```
        ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/65d0fafb-2167-4203-ade6-bde7bc48caab)

    * **Unzip**
        ```
          apt-get install unzip
        ```
        ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/f00b1aab-4cef-4c8e-9f44-8eca511fb6b5)

    * **Wget**
        ```
          apt-get install wget
        ```
        ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/6fc0302d-da3b-4756-99bb-14c56fbb79a8)


  * **Descarga de joomla** <br>

    * **Descargar joomla, descomprimir y moverlo a `/var/www/html/joomlaguia`**<br>
        ```
          wget https://downloads.joomla.org/cms/joomla4/4-1-2/Joomla_4-1-2-Stable-Full_Package.zip
        ```
        ```
          unzip Joomla_4-1-2-Stable-Full_Package.zip -d /var/www/html/joomlagioa
        ```
    * **Cambiar propietario y permisos**  <br>
      ```
       chown www-data:www-data -R /var/www/html/jooomlaguia
      ```
      ```
        chmod 755 -R /var/www/html/joomlaguia
      ```
        ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/c445d806-29e7-42ef-aa19-35d6864e44a5)


  * **Configuración apache2** <br>

    Vamos al directorio `/etc/apache2/sites-availables`
    
    * **Copiar el archivo default para hacer uno personalizado para Joomla** <br>
      ```
        cp 000-default.conf joomlaguia.conf
      ```
      ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/572447a0-1495-4a99-9ae7-92a9ac7524f4)

    * **Editar `joomlaguia.conf`** <br>
      ```
        nano -c joomlaguia.conf
      ```
      Debe quedar algo así
      ```  
        <VirtualHost *:80>                
                ServerName joomlaguia.com
                ServerAlias www.joomlaguia.com
                ServerAdmin admin@joomla.com
                DocumentRoot /var/www/html/joomlaguia/
      
                ErrorLog ${APACHE_LOG_DIR}/joomla_error.log
                CustomLog ${APACHE_LOG_DIR}/joomla_access.log combined

                <Directory /var/www/html/joomlaguia/>
                            Options Indexes FollowSymLinks
                            AllowOverride All
                            Require all granted
                            RewriteEngine on
                            RewriteBase /
                            RewriteCond %{REQUEST_FILENAME} !-f
                            RewriteCond %{REQUEST_FILENAME} !-d
                            RewriteRule ^(.*)$ index.php?q=$1 [L,QSA]
                 </Directory>
        </VirtualHost>
      ```
      ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/f5f74189-818a-4680-8494-7713880d36d8)


  
  * **Habilitar sitio y reiniciar el servicio de apache2**<br>
      ```
        a2ensite joomlaguia.conf
      ```
      ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/0a3e49c7-e042-4367-8fb1-5bffff6d86c4)

      ```
        systemctl restart apache2
      ```
      ```
        systemctl status apache2
      ```
      ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/7b5ac936-9b07-49b3-9000-b39ff652711b)


      
## **Configuración del servidor DNS**

  Para esta guía al igual que con la base de datos, se usa un servidor DNS ya creado (en este caso un Windows Server)

  * **Creación de una nueva zona `joomlaguia.com`**<br>
    ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/85f58c59-6485-41ba-974f-c3016d0eb4d8)

  * **Nuevo host A e inversa `www`**<br>
  ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/40d7b913-6770-4afb-b6cb-03045be07045)
<br>

  ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/a7f85c55-e712-4cbd-a4b8-f8f05eec5bef)

<br>
Con esto creamos el nombre DNS `www.joomlaguia.com`<br><br>
      

## **Instalación de Joomla**<br>

  Accedemos a la web mediante nuestro cliente (en este caso al igual que con el DNS y la BBDD, tenemos un cliente desde el que acceder ya creado), buscamos en la URL el nombre que le pusimos en el DNS: `www.joomlaguia.com:8182` <br>
      ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/15ada2d2-ee60-4461-8c58-e44cd7464ffc)
<br>
Continuamos completando los pasos de la instalación, como vemos en las imágenes, aquí los datos son independientes para cada persona:<br>
      ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/4e2599cb-165b-47c5-864d-da91975bd76c)
<br>
        ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/598c0cef-e161-4f19-8ea6-d73e8b50baa9)
<br>
        ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/c7a99bd5-63f1-4616-b7e5-82cccfb8a338)
<br>
    ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/7147a34f-8678-473b-9ad8-13c3ac4f110d)
<br>
    ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/d3c2ecc3-f6c1-406b-97b4-1a11cf52567e)
<br>
    ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/b0832b86-466c-482a-842c-c1c2c6ad4438)

## **Solución al uso del puerto**<br>
El uso de una URL y a la vez un puerto puede ser confusa y a veces olvidarse, para evitar esto podemos solucionarlo configurando en nuestra máquina docker (no en el contenedor) una redirección de puertos: <br>
  * **Instalar iptables**
    ```
      apt install iptables
    ```
    ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/e9eb4ceb-16b5-4c14-a8d9-b6a0c2248a23)
<br>
  * **Creación de tablas de redirección** <br>
    Con esto redireccionamos todos los paquetes que lleguen al puerto `80` hacia nuestro puerto `8182`, esta solución sirve si sólo estamos usando un contenedor que necesite del uso del puerto `80`, sino habría que configurar en la cración del contenedor diferentes puertos.<br>
    ```
      iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8182
    ```
## **Creación de imagen docker** <br>
  Una vez finalizado el proceso de instalación de Joomla!, debemos crear una imagen de la máquina para evitar la pérdida de datos, ya que docker pierde la información de las máquinas al apagarse.<br>
  Debemos salir del contenedor con la secuencia `Ctrl + p + Ctrl + q`. (También puede usarse exit, pero este comando puede apagar la máquina)<br><br>
  ![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/310423f1-dea2-47df-b30b-d71dd6f5fbfb)
<br>
```
  docker commit 0 joomlaguia_completa_imagen
```
```
  docker images
```
![imagen](https://github.com/EndOfBehelit/JoomlaOnDocker/assets/154753826/4704c3a2-60f3-451b-b579-1e0cc7d9e616)


