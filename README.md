## Iniciar Contenedor
docker-compose up -d


## Crear directorio oculto en path www:
.well-known


## Crear fichero index.html en path: 


## Activar configuración de nginx.conf para el ssl
Insertar el siguiente codigo en el fichero de configuracion nginx.conf en el path "./nginx/conf/"

```
server {
    listen 80;
    listen [::]:80;

    server_name example.org www.example.org;
    server_tokens off;

    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }

    location / {
        return 301 https://example.org$request_uri;
    }
}
```


## Crear certificados
docker-compose run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ -d example.com -d www.example.com


```
server {
    listen 80;
    listen [::]:80;

    server_name example.org www.example.org;
    server_tokens off;

    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }

    location / {
        return 301 https://example.org$request_uri;
    }
}

server {
    listen 443 default_server ssl http2;
    listen [::]:443 ssl http2;

    server_name example.org;

    ssl_certificate /etc/nginx/ssl/live/example.org/fullchain.pem;
    ssl_certificate_key /etc/nginx/ssl/live/example.org/privkey.pem;
    
    location / {
    	# ...
    }
}
```


## Reiniciar Server para aplicar cambios de SSL

```
docker-compose restart
```


Fuente / Referencia: https://mindsers.blog/post/https-using-nginx-certbot-docker/
