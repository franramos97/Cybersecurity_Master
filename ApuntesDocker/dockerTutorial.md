# Apuntes curso docker
- Link curso: https://www.youtube.com/watch?v=fqMOX6JJhGo

Con docker podemos correr distintos componentes en contenedores separados, cada uno con sus propias dependencias y librerias. Todo sobre el mismo sistema operativo y la misma VM.

- **Contenedores**: Entornos completamente aislados, con sus propios procesos, interfaces ... pero todos **comparten el mismo kernel del Sistema Operativo**
- Docker utiliza contenedores **LXC**

Los sistemas operativos Ubuntu, fedora, Suse, CentOS, todos comparten el mismo Kernel OS (Linux) pero es el software existente por encima lo que los hace sistemas operativos distintos.

Si tenemos un sistema con Ubuntu (basado en Linux) como SO y docker instalado en él, docker puede correr cualquier SO que esté basado también en el mismo Kernel (Linux). `En este caso no podemos correr Windows si tenemos un SO basado en Linux ni viceversa`

`Si corremos en Windows un docker de Linux, realmente Windows está corriendo un contenedor Linux sobre una máquina virtual de Linux (en background)`

Existen múltiples aplicaciones dockerizadas (versiones metidas en contenedores) disponibles en docker hub o docker store. Una vez instalado Docker en tu host y descargada la imagen, simplemente `docker run` correrá la instancia en cuestión

- **Docker Image**: Paquete o plantilla utilizado para crear uno o más contenedores. Docker Image --> DockerInstance#1,  DockerInstance#2...
- **Contenedores**: Instancias de imágenes que están corriendo.

## Instalar Docker en **Linux**

- Enlace: https://docs.docker.com/engine/install/ubuntu/ . Nos vamos a la parte de hacerlo con el script
- `curl -fsSL https://get.docker.com -o get-docker.sh` + `sudo sh get-docker.sh`

## Uso de Docker

`sudo docker run docker/whalesay cowsay Hello-World!` Con este comando docker hace un pull+run de la imagen whalesay de dockerhub

## Comando `run` 

- Para correr un **contenedor** a partir de una **imagen**: `docker run nginx` correrá una instancia de la aplicación 'ngingx'. Si la imagen no está presente en el host, va a 'docker hub' y hace un pull de la imagen

## Comando `ps`

- `docker ps`: Lista todos los contenedores que están corriendo (ID, status, imagen, puertos, nombre...)
- `docker ps -a`: Para ver el historial de contenedores que están o se han lanzado
- `

## Comando `stop`

- `docker stop "nombre_o_ID"`: Paramos el contenedor, debemos pasarle o el nombre o el ID (verlo con `docker ps`)

## Comando `rm`

- `docker rm "nombre_o_ID"`: Para quitarlo permanentemente. Ya no aparecerá al hacer `docker ps -a`

## Comando `images`

- `docker images`: Nos muestra una lista de imágenes presentes en nuestro ordenador (y el tamaño)


![DockerStructure](https://user-images.githubusercontent.com/78214153/109523609-d4069a00-7aaf-11eb-86ee-a965f3b494cd.PNG)







