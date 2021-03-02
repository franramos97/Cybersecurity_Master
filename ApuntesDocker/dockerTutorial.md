# Apuntes curso docker
- Link curso: https://www.youtube.com/watch?v=fqMOX6JJhGo


![DockerStructure](https://user-images.githubusercontent.com/78214153/109523609-d4069a00-7aaf-11eb-86ee-a965f3b494cd.PNG)

Con docker podemos correr distintos componentes en contenedores separados, cada uno con sus propias dependencias y librerias. Todo sobre el mismo sistema operativo y la misma VM.

- **Contenedores**: Entornos completamente aislados, con sus propios procesos, interfaces ... pero todos **comparten el mismo kernel del Sistema Operativo**
- Docker utiliza contenedores **LXC**

Los sistemas operativos Ubuntu, fedora, Suse, CentOS, todos comparten el mismo Kernel OS (Linux) pero es el software existente por encima lo que los hace sistemas operativos distintos.

Si tenemos un sistema con Ubuntu (basado en Linux) como SO y docker instalado en él, docker puede correr cualquier SO que esté basado también en el mismo Kernel (Linux). En este caso no podemos correr Windows si tenemos un SO basado en Linux ni viceversa

Si corremos en Windows un docker de Linux, realmente Windows está corriendo un contenedor Linux sobre una máquina virtual de Linux (en background)

Existen múltiples aplicaciones dockerizadas (versiones metidas en contenedores) disponibles en docker hub o docker store. Una vez instalado Docker en tu host y descargada la imagen, simplemente `docker run` correrá la instancia en cuestión

- **Docker Image**: Paquete o plantilla utilizado para crear uno o más contenedores. Docker Image --> DockerInstance#1,  DockerInstance#2...
- **Contenedores**: Instancias de imágenes que están corriendo.

## 1. Instalar Docker en **Linux**

- Enlace: https://docs.docker.com/engine/install/ubuntu/ . Nos vamos a la parte de hacerlo con el script
- `curl -fsSL https://get.docker.com -o get-docker.sh` + `sudo sh get-docker.sh`

## 2. Uso de Docker

`sudo docker run docker/whalesay cowsay Hello-World!` Con este comando docker hace un pull+run de la imagen whalesay de dockerhub

## 3. Comando `run` 

- Para correr un **contenedor** a partir de una **imagen**: `docker run nginx` correrá una instancia de la aplicación 'ngingx'. Si la imagen no está presente en el host, va a 'docker hub' y hace un pull de la imagen.
- Por defecto (si no ponemos `-it` de interactivo para que nos abra la shell) al hacer un run abre y cierra el contenedor (la instancia) **si no tiene ningún servicio dentro ejecutándose** como es el caso de los sistemas operativos (`docker run ubuntu`). Por lo que sólo haciendo `docker ps -a` lo veremos (y lo veremos en estado 'Exited') 
- `docker run kodekloud/simple-webapp`: Se ejecuta en **'attached mode'** que lo que hace es mostrarte la consola del container
- `docker run -d kodekloud/simple-webapp`: Se ejecuta en **'dettached mode'**, en modo background. Después podemos hacer `docker attach "nombre_o_ID"` para volver al modo attached

### 3.1 **Tags** (en Dockerhub podemos ver los 'tags' soportados)

- `docker run redis:4.0`: Nos lanza la versión 4.0 (tag = redis:4.0)
- `docker run redis`: Si no especifico 'tag' lanza la última versión (tag = 'latests')

### 3.2 Modo **interactivo** `-it`

- `docker run -it kodekloud/simple-prompt-docker`: Mapeamos el standard input de nuestro host al contenedor usando '-i' y utilizando '-t' adjuntamos la terminal al contenedor (sudo terminal)

### 3.3 PORT mapping `-p`. Ejemplo `docker run kodekloud/webapp` lanza una aplicación web. Para acceder:

- Supongamos que el comando anterior devuelve: *Running on http://0.0.0.0:5000* Por lo que se ejecuta en local, en el puerto 5000

- Internamente (desde mi host) usamos la IP del contenedor. A cada contenedor se le asigna una IP por defecto (es una IP interna que sólo se accede desde el host donde está el docker)
- Para que un usuario acceda desde fuera debe usar la IP del **docker host (nuestro host)**. Para que esto funcione debemos mapear el puerto dentro del contenedor docker a un puerto libre en el host. Si queremos que acceda un usuario externo a nuestra aplicación debemos mapear el puerto 80 de nuestro host con el puerto 5000 del contenedor (en el que corre el contenedor) --> `docker run -p 80:5000 kodekloud/simple-webapp`: Mapeamos el puerto 80-5000. Ahora un usuario externo accederá con `http://IP_mi_host:80` y el tráfico al puerto 80 se redirigirá al 5000. **NO PUEDE MAPEARSE UN MISMO PUERTO DEL HOST A VARIOS PUERTOS DE CONTENEDORES**

### 3.4 Volume mapping `-v`

- Cada contenedor docker tiene su propio sistema de archivos aislado.
- Si queremos **datos persistentes** --> Mapear un directorio externo (en el host) a uno interno en el contenedor: `docker run -v /opt/datadir:/var/lib/mysql mysql`: **directorio_host:directorio_internoDocker** 

## 4. Comando `pull`

- `docker pull nginx`: No ejecuta el run, sólo carga la imagen y la guarda en nuestro host.

## 5. Comando `ps`: Ver **CONTENEDORES**

- `docker ps`: Lista todos los contenedores que están corriendo (ID, status, imagen, puertos, nombre...)
- `docker ps -a`: Para ver el historial de contenedores que están o se han lanzado

## 6. Comando `inspect`

- `docker inspect "nombre_o_ID"`: Muestra info adicional de un contenedor (path, config data, networkSettings..)

### 6.1. Variables de entorno y `docker inspect`

- En un código python podemos poner por ejemplo una línea con `color = os.environ.get('APP_COLOR')`. Después podemos ejecutar `docker run -e APP_COLOR=blue simple-webapp-color` y estaremos ya pasándole el valor 'blue' directamente a la instancia que creamos.
- Cuando hacemos `docker inspect "nombre_o_ID"` en el apartado `"Env": [` veremos una lista de sus variables de entorno

## 7. Comando `stop`

- `docker stop "nombre_o_ID"`: Paramos el contenedor, debemos pasarle o el nombre o el ID (verlo con `docker ps`)

## 8. Comando `rm`

- `docker rm "nombre_o_ID"`: Para quitarlo permanentemente. Ya no aparecerá al hacer `docker ps -a`

## 9. Comando `images`: Ver **IMÁGENES**

- `docker images`: Nos muestra una lista de imágenes presentes en nuestro ordenador (y el tamaño)

## 10. Comando `rmi`: Borrar **IMÁGENES** (Comprobar si algún contenedor está corriendo desde esa imagen)

- `docker rmi nginx`: Nos borra la imagen 'nginx'
- `docker rmi $(docker images -a -q)`: Borra TODAS las imagenes

## 11. Comando `exec`: Para ejecutar un comando dentro de un contenedor

- `docker exec "nombre_o_ID" cat /etc/hosts`: Ejecuto cat de /etc/hosts dentro del docker

## 12. Comando `logs`

- `docker logs "nombre_o_ID"`: Para ver los contenidos escritos al standard output de ese contenedor

## 13. Crear imágenes propias. Uso de `Dockerfile`

- El `Dockerfile` está escrito con lenguaje específico para docker `FROM`, `RUN`, `COPY`, `ENTRYPOINT`... El primer comando siempre debe ser un 'FROM' del sistema operativo en el que se basa u otra imagen creada anteriormente basada en un OS.

<img width="1270" alt="DockerfileStructure" src="https://user-images.githubusercontent.com/78214153/109571023-4b0d5400-7aeb-11eb-8d2e-324b90c29909.PNG">


1) Crear un docker file llamado `Dockerfile` donde escribamos las instrucciones para instalar nuestra aplicación (instalación de dependencias, dónde copiar el código source, cuál es el ENTRYPOINT de la aplicación... en ese orden)

### 13.1 Comando `build` + `push`

2) Una vez hecho lo anterior, `docker build Dockerfile -t franete/custom-app`: Especificamos el 'Dockerfile' del que se crea y el tag '-t' para la imagen. Esto crea una imagen en local. Para hacerla pública en el registro docker hub --> `docker push franete/custom-app`

## 14 Docker labs: https://kodekloud.com/courses/enrolled/970256

## **CMD & ENTRYPOINT**

- Si miramos la imagen por ejemplo de 'Ngingx' veremos unas instrucciones `CMD` que define el programa que correrá dentro del contenedor cuando se ejecute. Si vemos `CMD ["bash"]` esto es que tira una shell que escucha inputs del terminal. 
- Para sobrescribir el comando (cmd) que viene en la imagen podemos: `docker run ubuntu [COMMAND]` Por ejemplo `docker run ubuntu sleep 5`




