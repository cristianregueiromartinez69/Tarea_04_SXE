### Tarea 4 de SXE

## Índice
Crear un  fichero (nombre "Readme.md") con formato markdown en un repositorio en github para subir este fichero y hacer commits que indiquen lo realizado.

1. Utiliza la imagen de Ubuntu , tag 22 y apoyandote en esta guía sigue sus instrucciones para instalar LAMP en dicho contenedor.

2. Utiliza esta guía para instalar wordpress en el contenedor.

3. Comprueba que puedes acceder a wordpress.

**OPCIONAL**: Instala phpmyadmin en el contenedor siguiendo esta guía. Comprueba que puedes acceder.


### 1. Utiliza la imagen de Ubuntu , tag 22 y apoyandote en esta guía sigue sus instrucciones para instalar LAMP en dicho contenedor.

```bash
#Nos meteemos en la máquina virtual y nos bajamos la imagen de ubuntu con el tag indicado
sudo docker pull ubuntu:22.04

#comprobamos que existe
sudo docker images

#Vamos a proceder a instalar LAMP, primero, creamos el contenedor y mapeamos el puerto
sudo docker container create -i -t  -p  8080:80 --name pw ubuntu:22.04

#arrancamos el contenedor
sudo docker container start --attach -i  pruebaWordp 
```

```bash
#si ha ido bien, habremos entrado en el terminal, procedemos a seguir la guía. Actualizamos los paquetes
sudo apt update

#instalamos apache2 utilizando este comando
sudo apt install -y apache2 apache2-utils

#instalamos el servidor de base de datos mariaDB
sudo apt install -y mariadb-server mariadb-client

#ejecutamos el siguiente comando para proteger la instalación mySQL
sudo mysql_secure_installation

#MUY IMPORTANTE, SI OS DA ERROR, HACED ESTO, YA QUE EN LA GUÍA NO ESTÁ
service mariadb start

#instalamos php
sudo apt install -y php php-mysql libapache2-mod-php

#Nos dirán de que región somos y más cosas, lo rellenamos y enter
```
![imagenPHP](https://github.com/user-attachments/assets/89242a89-c29f-4771-ac1f-18e5305c8479)


```bash
#Vamos a probar apache, ejecutamos este comando
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php

#Comprobamos que funciona, podemos hacerlo desde el navegador o a través de curl

#navegador
http://(ip maquina)/info.php
```
![wordpress](https://github.com/user-attachments/assets/bd53e650-22bd-44e2-bc1f-aed502b54a24)


```bash
#curl
apt-get update && apt-get install -y curl #instala curl
curl localhost/info.php #comprobamos que funciona el info.php, deberíamos de ver un html gigante
```
![curl](https://github.com/user-attachments/assets/21da007d-2425-465f-9fc3-012dfe11c3a6)


### 2. Utiliza esta guía para instalar wordpress en el contenedor.
```bash
#ejecutamos el siguiente comando que instalará muchos paquetes
sudo apt install ghostscript \
                 libapache2-mod-php \
                 mysql-server \
                 php \
                 php-bcmath \
                 php-curl \
                 php-imagick \
                 php-intl \
                 php-json \
                 php-mbstring \
                 php-mysql \
                 php-xml \
                 php-zip

#Creamos la instalación para el directorio
sudo mkdir -p /srv/www
sudo chown www-data: /srv/www
curl https://wordpress.org/latest.tar.gz | tar zx -C /srv/www
```

```bash
#Usa echo para crear el archivo /etc/apache2/sites-available/wordpress.conf
y redirigir la configuración a este archivo
cat <<EOF > /etc/apache2/sites-available/wordpress.conf
<VirtualHost *:80>
    DocumentRoot /srv/www/wordpress
    <Directory /srv/www/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Require all granted
    </Directory>
    <Directory /srv/www/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>
EOF

#habilitamos wordpress, pero antes reiniciamos apache2
service apache2 reload

#Una vez cargado apache2 de nuevo, ejecutamos este comando
a2ensite wordpress

#después este comando
sudo a2enmod rewrite

#Ahora tenemos que reiniciar de nuevo apache2
service apache2 restart

#luego ejecutamos este comando
sudo a2dissite 000-default

#por último, reiniciamos apache2 de nuevo
service apache2 reload
```

***Configuracion de la base de datos para wordpress***
```bash
#comandos a ejecutar
mysql -u root
CREATE DATABASE wordpress;
CREATE USER 'wordpress'@'localhost' IDENTIFIED BY '<your-password>';
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, ALTER ON wordpress.* TO 'wordpress'@'localhost';
FLUSH PRIVILEGES;
QUIT;

```

```bash
#comprobamos que accedemos a wordpress
http://192.168.1.46:8080/wp-admin/setup-config.php
```
**Debería de aparecer esto**
![img](https://github.com/user-attachments/assets/85ca5794-72b4-45fd-8896-22630dd31b9c)


## Configuración de wordpress
```bash
#establecemos el idioma, en mi caso español
```
![wordpressIdioma](https://github.com/user-attachments/assets/a64c18ea-ac10-45b9-9136-24283dc9ea6b)



```bash
#introducimos usuario, contraseña, alias y nombre de la web
```
![usercontraseñawordpress](https://github.com/user-attachments/assets/6bc1a60a-96f6-4eee-a580-be9b9c05b230)


```bash
#Si ha ido bien, nos saldrá un mensaje de exito y nos redigirá para introducir alias y contraseña
```
![accederWordpress](https://github.com/user-attachments/assets/42ee5342-519f-43ad-b681-72856bc34f43)


```bash
#si el alias y contraseña es correcto, nos saldrá algo así
```
![paginaIniciowordpress](https://github.com/user-attachments/assets/a69d12e1-4d5d-4ba7-aa33-1051b5368676)


### ENHORABUENA, SI TODO HA FUNCIONADO YA PUEDES INTERACTURAR CON WORDPRESS
:blush:

### Posibles problemas que os podéis encontrar en la instalación

```bash

#ESTE COMANDO ES SÚPER IMPORTANTE, MAPEAR LOS PUERTOS DEL CONTENEDOR CON LA MÁQUINA VIRTUAL
sudo docker container create -i -t  -p  8080:80 --name pw ubuntu:22.04

#iniciar mariadb, si no nos dará error al hacer (sudo mysql_secure_installation)
service mariadb start

```
