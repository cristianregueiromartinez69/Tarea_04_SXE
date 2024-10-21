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

#Vamos a proceder a instalar LAMP, primero, creamos el contenedor
sudo docker container create -i -t --name pruebaWordp ubuntu:22.04

#arrancamos el contenedor
sudo docker container start --attach -i  pruebaWordp 

#si ha ido bien, habremos entrado en el terminal, procedemos a seguir la guía. Actualizamos los paquetes
sudo apt update

#instalamos apache2 utilizando este comando
sudo apt install -y apache2 apache2-utils

#instalamos el servidor de base de datos mariaDB
sudo apt install -y mariadb-server mariadb-client

#ejecutamos el siguiente comando para proteger la instalación mySQL





```
