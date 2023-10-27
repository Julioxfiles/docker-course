### Installing Doker in Windows 10.

In order to use docker in your Windows 10 you need to install a program called Windows Subsystem for Linux (WSL), so you can install a Linux distribution directly on Windows.

1.- Installing  WSL. https://learn.microsoft.com/en-us/windows/wsl/install
```
 $ wsl --install
```

2.- Now you have to download and install docker desktop program from: https://www.docker.com/products/docker-desktop/

3.- Open an Ubuntu terminal and run the following commands.
```
$ docker --version
$ docker-compose --version 
```

Pulling a docker hello image from https://hub.docker.com/
```
$ docker run hello-world
```

### The basic docker command lists.
```
$ docker images # List all the images.
$ docker ps -a # List all the containers.
$ docker pull node # Download node.
$ docker pull node:18 # Download node 18 version.
$ docker image rm node # Remove the node image.
$ docker create mongo # Creates a mongo container.
$ docker start containerId # Start a container.

$ docker create container --name monguito mongo 
# Creates a container called monguito from mongo image. 

$ docker run mongo # Download a mongo image, create a container and start it.

$ docker log mongo # Log the mongo container.

# Using docker real case:
$ docker pull mongo # Download mongo.
$ docker ps -a # List all containers.
$ docker create container -p27017:27017 --name monguito 
-e MONGO_INITDB_ROOT_USERNAME=nico
-e MONGO_INITDB_ROOT_PASSWORD=password
mongo

# Network
$ docker network ls # List all networks
$ docker network create my-network # Create a new network.
$ docker network rm # Delete a network.

```
### Que es un contenedor ?

Un contenedor es una forma de poder empaquetar nuestros programas, aplicaciones que estamos desarrollando y sus dependencias. Incluyendo sus archivos de configuracion. Y es portable o facil de compartir.


### Donde se almacenan las imagenes ?

En un repositorio de imagenes llamado https://hub.docker.com . Las imagenes pueden ser publicas o privadas. Una vez que descargas una imagen, dicha imagen se almacena en tu computadora.

Antes de los contenedores los programadores tenian que trabajar con diferentes versiones de un mismo programa que descargaban para diferentes sistemas operativos instalados en sus respectivas computadoras.

### Commandos

Descarga la imagen hello-world desde el repositorio de contenedores.
```
docker run hello-world 
```

Lista las imagenes actuales en tu computadora.
```
docker images
```

Instala la version de node de diversas formas.
```
docker pull node # Instala la version latest de node.
docker pull node:18 # Instala la version 18 de node
docker pull node:16
```

Descarga una imagen de docker hub.
```
$ docker pull mysql // Descarga la ultima.
```

Descarga mysql para cierta plataforma o arqutectura.

```
$ docker pull --platform linux/x86_64 mysql
$ docker image rm node:18 # Elimina la version 18
$ docker image rm node:16 # Elimina la version 16
$ docker image rm mysql # Elimina mysql
$ docker image rm node # Elimina node
$ docker images
```

Descargando la ultima imagen de mongo para crear un contenedor de mongo.
```
$ docker pull mongo
```

Variables de configuracion. Estas dependen de la imagen que estemos utilizando para crear nuestros contenedores.

This will create a container and will return a containerId
```
$ docker create mongo 
```

This will also create a mongo container. Here we are using the word container after the docker command.

```
$ docker container create mongo
# Will create a container called mongo and will return a containerId.
```

Running a docker container
```
$ docker start containerId 
$ docker ps # List the running containers.
$ docker stop containerId 
# Will stop the container using its containerId.
$ docker ps -a # List all the containers and their status. 
```

You can delete a container by its name also.
```
$ docker rm containerName 
```

### Setting a name to a container
```
$ docker create --name monguito mongo 
# Crea un contenedor llamado monguito a partir de la imagen de mongo.
docker start monguito # Arranca el contenedor
```

### Puertos

Un puerto publicado es un puerto abierto en nuestra computadora.

Los contenedores tienen un puerto y tu tienes que abrir un puerto en tu computadora que se conecte con ese puerto. A esta accion se le llama mapeo. Se dice que tenemos que mapear el puerto de nuestra maquina hacia el puerto del contenedor.

```
$ docker stop monguito
$ docker rm monguito
$ docker create -p 27017:17017 --name monguito mongo
```

El primero es el puerto de nuestra maquina, El segundo es el puerto del contenedor.

El attributo -p significa publish o puerto.

```
$ docker start monguito # Start monguito container.
$ docer ps -a # List all the containers 
```

La ip 0.0.0.0:27017 Esta es nuestra maquina y puerto
-> 27017/tcp # Este es el contenedor.

```
$ docker logs monguito # Muestra la salida del contenedor de mongo.
```
Tambien le podrias pasar el id del contenedor.

```
$ docker logs --follow monguito
$ docker stop monguito # Stop monguito.
$ docker rm monguito # Remove monguito.
$ docker image rm mongo # Remove mongo.
$ docker images # List all images.
```

### The docker run command
```
$ docker run mongo 
```

Docker run busca la imagen en local, si no la encuentra la descarga. Luego crea el contenedor y luego inicia el contenedor. Sin embargo esta opcion, utiliza docker log --follow al ejecutar el contenedor. De tal forma que no nos permite continuar escribiendo mas comandos en nuestra terminal actual. Es decir, no corre en segundo plano el comando, sino que tenemos que abrir otra terminal.

Use control + c to quit

```
$ docker run -d mongo 
# Esto utiliza la opcion detach para correr mongo en segundo plano.

$ docker ps -a # Nos mostrara que esta corriendo.
$ docker stop idContainer
$ docker rm idContainer
$ docker image rm mongo
$ docker run --name monguito -p27017:27017 -d mongo
$ docker ps - a
```

Debes tener en cuenta que cada vez que ejecutas docker run creas un nuevo contenedor y lo corres. Y podria ser que no fuera eso lo que quisieras.

```
$ docker ps -a # List all containers.
$ docker stop monguito # Stop monguito 
$ docker rm monguito # Remove monguito.
$ docker ps -a # List all containers.
```

### Caso de uso

```
$ docker pull mongo # Download mongo.
$ docker ps -a # List all containers.
$ docker create container -p27017:27017 --name monguito 
-e MONGO_INITDB_ROOT_USERNAME=nico
-e MONGO_INITDB_ROOT_PASSWORD=password
mongo
```

Nota: La -e significa variable de entorno.

```
$ docker ps -a
$ docker start monguito
$ docker ps -a
$ node index.js
```

Abre chrome https://localhost:3000
https://localhost:3000/crear # Crea un elemento 
https://localhost:3000/ # Listar

### Como podemos crear una aplicacion y meterla en una imagen y meterla en un container.

1.- Crear un archivo llamado Dockerfile.yml

```
FROM node:18 
# La imagen base que utilizara sera node version 18.

RUN mkdir -p /home/app 
# Creara una carpeta donde vamos a meter el codigo fuente de nuestra aplicacion. Esta ruta es una ruta dentro del mismo contenedor. Lo que vendria siendo una distribucion de linux. Esto no hace referencia a tu maquina fisica o a tu sistema de archivos de tu maquina. Sino mas bien hace referencia al contenedor que estamos creando ahora.

COPY  . /home/app 
# Esta es la carpeta en donde tienes el codigo fuente que estas creando en tu computadora anfitrion y que quieres pasar al contenedor. El comando COPY nos permite acceder al sistema operativo anfitrion de donde va a tomar el codigo fuente.

EXPOSE 3000
# Exponer un puerto para que otros contenedores o nosotros desde un sistema operativo anfitrion se pueda conectar a este contenedor.

CMD ["node","/home/app/index.js"]
# Indica el comando que tiene que ejecutar el contenedor para que la aplicacion corra.
Esto se hace con corchetes, como si fuera un array, en donde el primer elemento es el comando y el segundo la ruta completa del archivo que se ejecutara.
```


Si no ponemos la ruta completa la aplicacion dentro del contenedor no se va a ejecutar y cuando usemos la instruccion docker logs vermos que no corrio correctamente.

### Networks
Para poder conectar un contenedor con otro, por ejemplo un contenedor de php con uno de mysql y estos con otro en donde tenemos el codigo de nuestra app necesitamos crear una red o network.

Se requiere agrupar los contenedores a traves de una red interna de docker para que los contenedores se puedan comunicar entre si.

```
$ docker network ls # Lista las redes actuales.
```

### Ceando una red (network)

```
$ docker network create myNetwork 
# Creates a network called myNetwork

$ docker network ls # List all networks.
$ docker network rm myNetwork # Remove the network.
```

La forma que tienen los contenedores de comunicarse entre si cuando estos se encuentran en una misma red es a traves del nombre del contenedor. Esto significa que si en tu aplicacion haces referencia a localhost, entonces debes de sustituir localhost por el nombre del contenedor.

```
$ docker build -t image:version path-of-your-dockerfile
# Crea una imagen usando el archivo docker file.

$ docker build -t myApp:1 . 
# Here the dot represents the current path. In this case the dockerFile is in the current path.

$ docker images # Para listar la imagen creada.
```

Lo siguiente crea el contenedor para usar mongo. Es como si crearamos un contenedor para mysql y le pasaramos el usuario root y el password.

```
$ docker create -p27017:27017 --name monguito --network myNetwork 
-e MONGO_INITDB_ROOT_USERNAME=nico
-e MONGO_INITDB_ROOT_PASSWORD=password
mongo
```

Ahora creamos el contenedor en donde tenemos nuestra aplicacion. La cual se comunicara con el contenedor anterior, el contenedor monguito de mongo.

```
$ docker create -p3000:3000 --name chanchito --network myNetwork myApp:1
# Aqui chanchito es el contenedor en donde tenemos la imagen de nuestra aplicacion.
```

Ahora arrancamos los dos contenedores:
```
$ docker start monguito
$ docker start chanchito
$ docker logs chanchito 
# Esto ultimo mostrara los logs o la salida de la aplicacion que estamos corriendo. En este caso la aplicacion es un servidor node.

Output example:
listening on port 3000...
created...
listing..
```

RESUMEN:

1.- Descargar la imagen de mongo.

2.- Crear una red

3.- Crear el contenedor asignandole nombre, puertos, variables de entorno, especificar la red e indicar la imagen con su respectiva etiqueta o version de la imagen. Todo esto por cada contenedor.

### Usando Docker Compose

Agregar el archivo docker-compose.yml y agregar lo siguiente como su contenido.

```
version: "3.9"
services:
	chanchito:
		build: .
		ports: 
			- "3000:3000"
		links:
			- monguito
	monguito:
		image: mongo
		ports:
			- "27017:27017"
		environment:
			- MONGO_INITDB_ROOT_USERNAME=nico
			- MONGO_INITDB_ROOT_PASSWORD=password
		volumes:
			- mongo-data: /data/db
			#  - mysql: /var/lib/mysql
			#  - postgres: /var/lib/postgres/data
		

volumes:
	mongo-data:
    # No agregue nada despues de mongo-data:
	# Puede usar volumes para mysql o postgress.
	# mysql
	# postgress


```

### Persistencia de datos usando volumes

El primer mongo-data, le indica a el contenedor monguito cual es el volumen que va a utilizar. Aqui debes especificar la ruta en donde mongodb guarda los datos. 

El ultimo mongo-data al final del archivo define todos los volumenes que van a utilizar  nuestros contenedores. Si hubiera mas volumenes para mysql o postgress irian aqui.

```
$ docker compose up 
# Esto construira los contenedores a partir del archivo docker-compose.yml
```

Al usar este comando otra vez, no se crearan nuevos contenedores. Sino mas bien se utilizaran los contenedores creados la primera vez que se ejecuto este comando.

### Eliminando todo lo que docker compose up creo.

```
$ docker compose down 
# Esto eliminara los contenedores creados con docker-compose.yml al usar el comando docker compose up. Es como un roll back.

$ docker compose up # Ejecuta los contenedores.
$ docker compose down # Roll back
```

## The Dockerfile.dev

```
FROM node:18 
# La imagen base que utilizara sera node version 18.

RUN npm i -g nodemon
# Instala y ejecuta nodemon, para mantener node siempre escuchando nuevos cambios en el codigo y arrancarlo cada vez que se guardan los cambios.

RUN mkdir -p /home/app 
# Creara una carpeta donde vamos a meter el codigo fuente de nuestra aplicacion. Esta ruta es una ruta dentro del mismo contenedor. Lo que vendria siendo una distribucion de linux. Esto no hace referencia a tu maquina fisica o a tu sistema de archivos de tu maquina. Sino mas bien hace referencia al contenedor que estamos creando ahora.

WORKDIR /home/app
# Define que la ruta de directorio de trabajo.

# COPY  . /home/app 

# El comando  COPY nos permite acceder al sistema operativo anfitrion de donde va a tomar el codigo fuente.
# Sin embargo, el comando COPY en este archivo esta comentado porque usaremos volumes para crear un enlace simbolico en nuestra ruta /home/app hacia el codigo de nuestra aplicacion.

EXPOSE 3000
# Exponer un puerto para que otros contenedores o nosotros desde un sistema operativo anfitrion se pueda conectar a este contenedor.

# Al utilizar WORKDIR y especificar nuestro directorio de trabajo, ya no es necesario especificar nuestra ruta completa en el segundo elemento del arreglo. Solo sera necesario el nombre del archivo a ejecutar.
CMD ["nodemon","index.js"]

```

### Agregando el archivo docker-compose-dev.yml

```
version: "3.9"
services:
	chanchito:
		build: 
			context: .
			dockerfile: Dockerfile.dev
		ports: 
			- "3000:3000"
		links:
			- monguito
		volumes:
			- .:/home/app 
			# El punto es la ruta actual, la cual debe ser montada en 
			# :/home/app representa la ruta dentro del contenedor.

	monguito:
		image: mongo
		ports:
			- "27017:27017"
		environment:
			- MONGO_INITDB_ROOT_USERNAME=nico
			- MONGO_INITDB_ROOT_PASSWORD=password
		volumes:
			- mongo-data: /data/db
			#  - mysql: /var/lib/mysql
			#  - postgres: /var/lib/postgres/data
		

volumes:
	mongo-data:
	# mysql
	# postgress
```

Ejecutando el archivo:

```
$ docker compose -f docker-compose-dev.yml up
```

La -f es para indicarle el nombre del archivo que usaremos. De otra manera utilizaria el docker-compose.yml y eso no queremos. Lo que queremos que utilice el docker-compose-dev.yml

HOT RELOAD se puede utilizar en ambiente de desarrollo.

Al llegar un nuevo desarrollador el solo ejecuta docker compose up.

This docker summary was taken from:

Video: https://www.youtube.com/watch?v=4Dko5W96WHg&t=235s

Channel: https://www.youtube.com/@HolaMundoDev

Web Page: https://academia.holamundo.io/collections

Done by: Nicolás Schürmann













