# Alpine linux es una imagen base muy ligera, ideal para aplicaciones simples como esta
FROM nginx:alpine

# Borramos el HTML por defecto que trae Nginx
RUN rm -rf /usr/share/nginx/html/*

# Copiamos nuestro index.html dentro de la ruta donde Nginx sirve los archivos
COPY index.html /usr/share/nginx/html/index.html

# Exponemos el puerto 80 (por donde escucha Nginx dentro del contenedor)
EXPOSE 80