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

## 7. Comando `stop` y `start`

- `docker stop "nombre_o_ID"`: Paramos el contenedor, debemos pasarle o el nombre o el ID (verlo con `docker ps`)
- `docker start "nombre_o_ID"`: Para volver a arrancar el contenedor

## 8. Comando `rm`

- `docker rm "nombre_o_ID"`: Para quitarlo permanentemente. Ya no aparecerá al hacer `docker ps -a`
- `docker rm -f $(docker ps -aq)`: Esto los borra todos

## 9. Comando `images`: Ver **IMÁGENES**

- `docker images`: Nos muestra una lista de imágenes presentes en nuestro ordenador (y el tamaño)
- La nomenclatura de las imágenes es `image: docker.io/nginx/nginx` donde el primer apartado es el 'Regystry' que por defecto es dockerHub, el 2º es el "UserAccount" que es la cuenta del docker hub y el 3º es el "ImageRepository"

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

2) Una vez hecho lo anterior, `docker build Dockerfile -t franete/custom-app:2.0 .`: Especificamos el 'Dockerfile' del que se crea, el nombre es "franete/custom-app" y el tag '-t' es 2.0. Esto crea una imagen en local. Para hacerla pública en el registro docker hub --> `docker push franete/custom-app`

## 14. Docker labs: https://kodekloud.com/courses/enrolled/970256

## 15. **CMD & ENTRYPOINT**

- Si miramos la imagen por ejemplo de 'Ngingx' veremos unas instrucciones `CMD`, esto define el programa que correrá dentro del contenedor cuando se ejecute. Si vemos `CMD ["bash"]` esto es que tira una shell que escucha inputs del terminal. 
- Para sobrescribir el comando (cmd) que viene en la imagen podemos: `docker run ubuntu [COMMAND]` Por ejemplo `docker run ubuntu sleep 5`
- Con la instrucción `ENTRYPOINT` indicas el programa que correrá cuando se levante el contenedor 

- Con `CMD sleep 5` dentro del **Dockerfile** tendremos que levantarlo (si queremos cambiar el parámetro pasado a sleep) mediante `docker run ubuntu-sleeper sleep 10`
- Si tenemos en el **Dockerfile** `ENTRYPOINT ["sleep"]` cuando simplemente tendremos que levantarlo haciendo `docker run ubuntu-sleeper 10`: **Aquí no hace falta indicarle 'sleep' porque va en el ENTRYPOINT**
- Se puede combinar en el **Dockerfile** `ENTRYPOINT ["sleep"]` + `CMD ["5"]` para que si ejecutamos por defecto `docker run ubuntu-sleeper` no nos dé error (pq con entrypoint debemos indicarle un valor a sleep y con cmd debemos indicarle la instrucción). De este modo combina el ENTRYPOINT y el CMD para ejecutar `sleep 5`
- `docker run --entrypoint sleep2.0 ubuntu-sleeper 10`: Para cambiar el entrypoint directamente.

## 16. Networks. `Bridge`, `none`, `host`

- Al instalar docker crea tres redes automaticamente:


1) `Bridge`: Se crea con `docker run ubuntu` por defecto. Es una red privada creada por docker en el host, los contenedores se adjuntan ahí por defecto y obtienen una IP interna, los contenedores se comunican entre ellos mediante esta red
2) `Host`: Se crea con `docker run Ubuntu --network=host`. Permite que usuarios externos puedan conectarse a los contenedores (CUIDADO: ya no existirá aislamiento entre el host y el contenedor pero tampoco haría falta mapear los puertos entre los puertos del host y del contenedor, el contenedor usaría el puerto real del host)
3) `None`: Se crea con `docker run Ubuntu --network=none`. De esta forma los contenedores no están unidos a ninguna red y no tienen acceso a la red externa ni a otros contenedores (red aislada)

### 16.1. Definir redes

- `docker network create ---driver bridge --subnet 182.18.0.0/16 nombre-red-aislada`: Para crear nuestra propia red interna
- `docker network ls`: Lista de las redes docker.
- `docker network inspect bridge`: Inspecciona esa red en concreto (ver subred, gateway...)

## 17. Almacenamiento en Docker. File system

Al instalar docker en un sistema crea la estructura `/var/lib/docker` y bajo ese tenemos `aufs`, `containers`, `image`, `volumes`. Cada fichero contiene info sobre su nombre (containers: sobre contenedores...).

![container system](https://user-images.githubusercontent.com/78214153/109785081-0af1c280-7c0c-11eb-996b-17e24a545ca3.PNG)

### 17.1 Volume mounting

- Para crear un volumen persistente: `docker volume create data_volume` esto crea "/var/lib/docker/volumes/data_volume" y lanzo `docker run -v data_volume:/var/lib/mysql mysql` si por ejemplo 'data_volume' no lo hemos creado con el 1º comando de antes, docker lo genera automáticamente con el 2º comando que tenemos (run -v)

## 18. Docker `compose`

Escenario donde queremos lanzar 'voting-app', 'result-app', 'in-memory DB', 'db' y 'worker'. Debemos **enlazarlos entre sí** para que se puedan alcanzar entre ellos, para eso usamos el comando `link` que enlaza dos contenedores. Para utilizar link IMPORTANTE DARLES UN NOMBRE A LOS CONTENEDORES

- `docker run -d --name=redis redis`: Lanzamos el 'in-memory DB' que es una instancia de redis, '-d' para correrlo en background
- `docker run -d --name=db postgres:9.4 --link db:db result-app`: Lanzamos el 'db', instancia de postgres y lo enlazamos al db_db
- `docker run -d --name=vote -p 5000:80 --link redis:redis voting-app`: Esto es un servidor web y lo tenemos corriendo en el puerto 80 (5000 en nuestro host). Con `--link` le indicamos que enlace con la imagen redis:redis
- `docker run -d --name=result -p 5001:80`: Para ver los resultados en el servidor web en el puerto 5001
- `docker run -d --name=worker --link redis:redis --link db:db worker`: En programa .net para procesar los datos

Ahora procederemos a generar un `docker-compose.yml` a partir de lo anterior 

![dockerCompose](https://user-images.githubusercontent.com/78214153/109789663-d7fdfd80-7c10-11eb-8ccd-704afd33e78d.PNG)

- `environment:` 

Posteriormente podemos realizar un `docker-compose up` para levantar toda la arquitectura anterior.
Podemos tener varios 'docker-compose.yml' pero deben comenzar con `version: 2`... para poder diferenciarlos
Usando `depends_on:` dentro del .yml podemos establecer un orden de inicio para que una app arranque antes que otra









