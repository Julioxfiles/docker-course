## Installing Doker in Windows 10.

In order to use docker in your Windows 10 need to install a program called Windows Subsystem for Linux (WSL), so you can install a Linux distribution directly on Windows.

1.- Installing  WSL. https://learn.microsoft.com/en-us/windows/wsl/install
```
 $ wsl --install
```

2.- Now you have to download and install docker desktop from

3.- Open an Ubuntu terminal and run the following commands.
```
$ docker --version
$ docker-compose --version 
```

Pulling a Docker Hello Image from DockerHub
```
$ docker run hello-world
```

Now in your terminal, you can use the following docker command to have a look at all the containers that are currently running or have run in the past.
```
$ ps -a  
$ docker ps -a
```

The following docker summary was taken from:

Video: https://www.youtube.com/watch?v=4Dko5W96WHg&t=235s

Channel: https://www.youtube.com/@HolaMundoDev

Web Page: https://academia.holamundo.io/collections
Done by: Nicolás Schürmann

### Que es un contenedor ?

Un contenedor es una forma de poder empaquetar nuestros programas, aplicaciones y sus dependencias. Incluyendo sus archivos de configuracion.

En un contenedor podemos tener todos los archivos que conforman nuestra aplicacion y sus dependencias y ese contenedor es portable o facils de compartir entre los desarrolladores.

### Donde se almacenan las imagenes ?
En un repositorio de contenedores llamado https://hub.docker.com . Pueden ser publicos o privados.

Antes de los contenedores los programadores tenian que trabajar con diferentes versiones un mismo programa que descargaban y en diferentes sistemas operativos instalados en sus computadoras.

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

docker stop monguito
docker rm monguito

$ docker create -p 27017:17017 --name monguito mongo

El primero es el puerto de nuestra maquina, El segundo es el puerto del contenedor.
-p =  publish o puerto si gustas.

$ docker start monguito
$ docer ps -a

La ip 0.0.0.0:27017 Esta es nuestra maquina y puerto
-> 27017/tcp # Este es el contenedor.

$ docker logs monguito # Muestra la salida de mongo o lo que esta pasando.
Tambien le podrias pasar el id del contenedor.

$ docker logs --follow monguito

docker stop monguito
docker rm monguito
docker image rm mongo
docker images

$ docker run mongo 

Docker run busca la imagen en local, si no la encuentra la descarga. Luego crea el contenedor y luego inicia el contenedor. Sin embargo esta opcion, utiliza docker log --follow al ejecutar el contenedor. De tal forma que no nos permite continuar en nuestra terminal.  Es decir, no corre en segundo plano la instruccion, sino que tendriamos que abrir otra terminal.

Use control + c to quit

$ docker run -d mongo # Esto utiliza la opcion detach para correr mongo en segundo plano.

$ docker ps -a # Nos mostrara que esta corriendo.

$ docker stop idContainer
$ docker rm idContainer
$ docker image rm mongo

$ docker run --name monguito -p27017:27017 -d mongo
$ docker ps - a

Debes de tener en cuenta que cada vez que ejecutas docker run creas un nuevo contenedor y lo corres. Y podria ser que no fuera eso lo que quisieras.

$ docker stop monguito # o el idContainer
$ docker rm monguito # o el idContainer
$ docker ps -a

Caso de uso:
$ docker pull mongo
$ docker ps -a 
$ docker create container -p27017:27017 --name monguito 
-e MONGO_INITDB_ROOT_USERNAME=nico
-e MONGO_INITDB_ROOT_PASSWORD=password
mongo

Nota: La -e significa variable de entorno.

$ docker ps -a
$ docker start monguito
$ docker ps -a

$ node index.js

Abre chrome https://localhost:3000
https://localhost:3000/crear # Crea un elemento 
https://localhost:3000/ # Listar

## Como podemos crear una aplicacion y meterla en una imagen y meterla en un container.

1.- Crear un archivo llamado Dockerfile
Ejemplo de contenido del archivo Dockerfile

FROM node:18 # La imagen base que utilizara sera node version 18.
RUN mkdir -p /home/app 
# Creara una carpeta donde vamos a meter el codigo fuente de nuestra aplicacion. Esta ruta es una ruta dentro del mismo contenedor. Lo que vendria siendo una distribucion de linux. Esto no hace referencia a tu maquina fisica o a tu sistema de archivos de tu maquina. Sino mas bien hace referencia al contenedor que estamos creando ahora.

COPY  . /home/app # Esta es la carpeta en donde tienes el codigo fuente que estas creando en tu computadora anfitrion y que quieres pasar al contenedor. El comando COPY nos permite acceder al sistema operativo anfitrion de donde va a tomar el codigo fuente.

EXPOSE 3000
Exponer un puerto para que otros contenedores o nosotros desde un sistema operativo anfitrion se pueda conectar a este contenedor.

CMD ["node","/home/app/index.js"]
Indicar el comando que tiene que ejecutar para que la aplicacion corra.
Esto se hace con corchetes, como si fuera un array, en donde el primer elemento es el comando y el segundo el nombre del programa que se ejecutara.

Si no ponemos la ruta completa la aplicacion dentro del contenedor no se va a ejecutar y cuando usemos la instruccion docker logs vermos que no corrio correctamente.

REDES
Para poder conectar un contenedor con otro, por ejemplo un contenedor de php con uno de mysql con otro en donde tenemos el codigo de nuestra app necesitamos crear una red o network.

Se requiere agrupar los contenedores a traves de una red interna de docker para que los contenedores se puedan comunicar entre si.


$ docker network ls # Lista las redes actuales.

Cear una red:

$ docker network create myNetwork # Creates a network called myNetwork
$ docker network ls
$ docker network rm myNetwork # Elimina o borra la red.

La forma que tienen los contenedores de comunicarse entre si cuando estos se encuentran en una misma red es a traves del nombre del contenedor. Esto significa que si en tu aplicacion haces referencia a localhost, entonces debes de sustituir localhost por el nombre del contenedor.

$ docker build -t newImage:version pathOfYourDockerFile # Crea una imagen usando el archivo docker file.

$ docker build -t myApp:1 .  // Here the dot represents the current path. In this case the dockerFile is in the current path.

$ docker images # Para listar la imagen creada.

// Lo siguiente crea el contenedor para usar mongo. Es como si crearamos un contenedor para mysql y le pasaramos el usuario root y el password.

$ docker create -p27017:27017 --name monguito --network myNetwork 
-e MONGO_INITDB_ROOT_USERNAME=nico
-e MONGO_INITDB_ROOT_PASSWORD=password
mongo

// Ahora creamos el contenedor en donde tenemos nuestra aplicacion. La cual se comunicara con el contenedor anterior, el contenedor monguito de mongo.

$ docker create -p3000:3000 --name chanchito --network myNetwork myApp:1
// Aqui chanchito es el contenedor en donde tenemos la imagen de nuestra aplicacion.

Ahora arrancamos los dos contenedores:

$ docker start monguito
$ docker start chanchito

$ docker logs chanchito # Esto mostrara los logs o la salida de la aplicacion que estamos corriendo. En este caso la aplicacion es un servidor node.

Output example:
listening on port 3000...
created...
listing..

RESUMEN:

1.- Descargar la imagen de mongo.
2.- Crear una red
3.- Crear el contenedor asignandole nombre, puertos, variables de entorno, especificar la red e indicar la imagen con su respectiva etiqueta o version de la imagen. Todo esto por cada contenedor.

Docker Compose

Agregar el archivo docker-compose.yml y agregar lo siguiente como su contenido.

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
	# mysql
	# postgress


No se necesita agregar nada despues de mongo-data:

Persistencia de datos usando volumes

El primer mongo-data, le indica a el contenedor monguito cual es el volumen que va a utilizar. Aqui debes especificar la ruta en donde mongodb guarda los datos. 

El ultimo mongo-data al final del archivo define todos los volumenes que van a utilizar  nuestros contenedores.

$ docker compose up ## Esto construira los contenedores a partir del archivo docker-compose.yml

Al usar este comando otra vez, no se crearan nuevos contenedores. Sino mas bien se utilizaran los contenedores creados la primera vez que se ejecuto este comando.

Eliminando todo lo que docker compose up creo.

$ docker compose down # Esto eliminara los contenedores creados con docker-compose.yml usando el comando docker compose up.

$ docker compose up
$ docker compose down

## The Dockerfile.dev

FROM node:18 # La imagen base que utilizara sera node version 18.

RUN npm i -g nodemon

RUN mkdir -p /home/app 
# Creara una carpeta donde vamos a meter el codigo fuente de nuestra aplicacion. Esta ruta es una ruta dentro del mismo contenedor. Lo que vendria siendo una distribucion de linux. Esto no hace referencia a tu maquina fisica o a tu sistema de archivos de tu maquina. Sino mas bien hace referencia al contenedor que estamos creando ahora.

WORKDIR /home/app

# COPY  . /home/app 

# El comando  COPY nos permite acceder al sistema operativo anfitrion de donde va a tomar el codigo fuente.
# Sin embargo, el comando COPY en este archivo esta comentado porque usaremos volumes para crear un enlace simbolico en nuestra ruta /home/app hacia el codigo de nuestra aplicacion.

EXPOSE 3000
Exponer un puerto para que otros contenedores o nosotros desde un sistema operativo anfitrion se pueda conectar a este contenedor.

# Al utilizar WORKDIR y especificar nuestro directorio de trabajo, ya no es necesario especificar nuestra ruta completa en el segundo elemento del arreglo. Solo sera necesario el nombre del archivo a ejecutar.
CMD ["nodemon","index.js"]


Agregando el archivo docker-compose-dev.yml

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

$ docker compose -f docker-compose-dev.yml up
# La -f es para indicarle el nombre del archivo que usaremos. De otra manera utilizaria el docker-compose.yml y eso no queremos. Queremos que utilice el docker-compose-dev.yml

HOT RELOAD se puede utilizar en ambiente de desarrollo.

Al llegar un nuevo desarrollador el solo ejecuta docker compose up


















