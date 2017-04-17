# Projecto django como plantilla para usarlo con docker

## Actualizar docker-compose ubuntu 16.04
```
$ curl -L https://github.com/docker/compose/releases/download/1.9.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/bin/docker-compose
```


## Comandos para docker-compose

Construir la imagen por nombre de servicio definido en el archivo compose
```
$ docker-compose build web
```

Levantar los contenedores como demonio (-d)
```
$ docker-compose up -d
```

Bajar los contenedores
```
$ docker-compose down
```

Ver los contenedores activos
```
$ docker-compose ps
```

Ver logs (en vivo con el flag '-f') por el nombre del servicio ejecutado
```
$ docker-compose logs -f servicename
```

## Comandos para docker

Conectarse a la terminal de un contenedor por el nombre o el id
```
$ docker exec -it namecontainer bash
$ docker exec -it 58083a8eb6ea bash
```

Listar contenedores
```
$ docker container ls
```

Listar imagenes
```
$ docker image ls
```

Listar las redes
```
$ docker network ls
```

Mirar todos los contenedores ejecutandose (view the running containers)
```
$ docker ps
```

Mirar todos los contenedores ejecutandose o no
```
$ docker ps -a
```

Inspeccionar elemento de docker(contenedor, imagen o red) por ID
```
$ docker inspect ccd39e77fd96
```

Eliminar un contenedor por ID
```
$ docker rm 0fb8397006b3
```

Eliminar una imagen por ID
```
$ docker rmi 18829894bab7
```

Eliminar todos los contenedores
```
$ docker rm $(docker ps -aq)
```

Eliminar todas las imagenes 1
```
$ docker rmi $(docker images -q)
```

Eliminar todas las imagenes 2
```
$ docker rmi $(docker images -qf "dangling=true")
```

Kill todos los contenedores y eliminarlos. Note: Replace 'kill' with 'stop' for graceful shutdown
```
$ docker rm $(docker kill $(docker ps -aq))
```

Mostrar las imagenes filtrando por las que contengan las palabras 'django' y 'python'
```
$ docker images | grep 'django\|python'
```

Mostrar las imagenes filtrando por las que NO contengan las palabras 'django' y 'python'
```
$ docker images | grep -v 'django\|python'
```

Mostrar las imagenes filtrando por las que NO contengan las palabras 'django' y 'python' y mostrar la primera columna
```
$ docker images | grep -v 'django\|python' | awk {'print $1'}
```

Mostrar las imagenes filtrando por las que NO contengan las palabras 'django' y 'python' y mostrar solo los ids
```
$ docker images | grep -v 'django\|python' | awk {'print $3'}
```

Ver los logs de un contenedor ejecutandose por ID o por su nombre
```
$ docker logs ce7c42761848
$ docker logs namecontainer
```

Ver logs en vivo (con el flag -f)
```
$ docker logs -f ce7c42761848
```




## Primer contenedor - https://github.com/docker/labs/blob/master/beginner/chapters/alpine.md

Descargar la imagen 'alpine' localmente, si no se especifica la version, el cliente docker tomara la ultima 'latest'
```
$ docker pull alpine
```
Es lo mismo que el siguiente comando
```
$ docker pull alpine:latest
```

Descargar la imagen especificando una version o tag
```
$ docker pull alpine:3.3
```

Ver la lista de todas las imagenes
```
$ docker images
```

Ejecucion de un contenedor
```
$ docker run alpine
```

Ejecucion de un contenedor con comandos por defecto para que los ejecute 'ls -l' o 'echo "hello from alpine"'
```
$ docker run alpine ls -l
$ docker run alpine echo "hello from alpine"
```

Ejecutar el contenedor y abrir el shell del contenedor en modo interactivo, para salir del shell ejecutar el comnando 'exit'
```
$ docker run -it alpine /bin/sh
```




## WebApp contenedores - https://github.com/docker/labs/blob/master/beginner/chapters/webapps.md

Ejecutar una imagen website static ('-d' lo ejecuta como demonio)
```
$ docker run -d dockersamples/static-site
```

Detener un contenedor y luego eliminarlo por ID (el id del contenedor se obtiene con el comando 'docker ps')
```
$ docker stop ce7c42761848
$ docker rm   ce7c42761848
```

Ejecutar una imagen con argumentos para levantar una website de la imagen 'dockersamples/static-site'
    * '-d' will create a container with the process detached from our terminal
    * '-P' will publish all the exposed container ports to random ports on the Docker host
    * '-e' is how you pass environment variables to the container
    * '--name' allows you to specify a container name
    * 'AUTHOR' is the environment variable name and 'Your Name' is the value that you can pass    
```
$ docker run --name static-site -e AUTHOR="Your Name" -d -P dockersamples/static-site
```

Ver los puertos asignados aleatoriamente al crear el contenedor 'static-site' o por ID
```
$ docker port static-site
$ docker port 3baf878e9092
```

Ejecutar un segundo contenedor webserver especificando un personalisable mapeo de puerto del host al contenedor
```
$ docker run --name static-site-2 -e AUTHOR="Your Name" -d -p 8888:80 dockersamples/static-site
```
En este caso el puerto del webserver (ngnix) del contenedor que es 80 se le asigna al puerto 8888 del host o la maquina local
Con esto se puede acceder a la web con la url: http://localhost:8888/

Otra forma de acceder a la web con la propia IP:PUERTO del contenedor es obtener el ID del contenedor con: 'docker ps'
Luego ejecutar para ver sus propiedades, buscar el valor de la llave 'IPAddress'
```
$ docker inspect 1e8d1e390817
```
Una vez obtenido la IP ingresar a la url, ejm: http://172.17.0.3/
No se necesita especificar en este caso el puerto 80, ya que es el puerto para el protocolo http por defecto en el contenedor

Ejecutar el contenedor usando la red del host usando '--net=host', es mas eficiente que el mapeo de puertos
```
$ docker run --name static-site -e AUTHOR="Your Name" -d --net=host dockersamples/static-site
```

Detener y eliminar el contenedor al mismo tiempo ('stop' and 'rm')
```
$ docker rm -f static-site-2
```

Buscar en Docker Hub una imagen por su nombre, ejm:
```
$ docker search redis
```




## Crear Imagen personalizada - https://github.com/docker/labs/blob/master/beginner/chapters/webapps.md#23-create-your-first-image
...


## Urls
https://docs.docker.com/samples/
https://docs.docker.com/compose/django/
https://docs.docker.com/engine/examples/running_redis_service/
https://docs.docker.com/engine/examples/postgresql_service/
https://docs.docker.com/compose/wordpress/

https://www.youtube.com/watch?v=cr0cbJ2PkJY
https://www.youtube.com/watch?v=_DJahG4FiOE
https://www.capside.com/labs/deploying-full-django-stack-with-docker-compose/
https://www.linux.com/learn/how-build-your-own-custom-docker-images
https://github.com/jcalazan/django-project-template
