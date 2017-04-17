# Se construye la imagen a partir de la imagen base, en este caso la base es python:3.5
FROM python:3.5
# Para esperar el valor de la variable de entorno pasado desde la ejecucion del contenedor, esta variable se pasa en el archivo docker-compose.yml
ARG DJANGO_ENV
# Indica que la salida estandar y la salida de errores lo muestre en la consola
ENV PYTHONUNBUFFERED=1
# Se crea la variable de entorno para el directorio principal dentro del contenedor donde va a estar todo el codigo fuente de django
ENV WEBAPP_DIR=/webapp
# Comando para crear la carpeta dentro de la raiz del contenedor
RUN mkdir $WEBAPP_DIR
# Se indica el directorio principal al ejecutarse el contenedor, es donde se ubica cuando se levanta el contenedor
WORKDIR $WEBAPP_DIR
# Se copia el archivo requirements.txt al directorio principal
ADD requirements/base.txt $WEBAPP_DIR/
ADD requirements/$DJANGO_ENV.txt $WEBAPP_DIR/
# Se ejecuta para instalar las dependencias necesario para el desarrollo
RUN pip install -r $DJANGO_ENV.txt
# Para que copie todo el contenido del directorio donde esta el archivo Dockerfile a la carpeta principal del contenedor, todo el codigo fuente
ADD . $WEBAPP_DIR/
