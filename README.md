# Entorno API Lumen dockerizado

Este es un entorno que utiliza NGINX, MySQL y php-fpm para tener un entorno de trabajo para Lumen.

# Dependencias
- Las mismas dependencias de Lumen 7.0.
- Docker y docker-compose.

# Configuración del entorno

## Primeros pasos
- Clonamos el repositorio en el lugar donde vayamos a instalar el proyecto.
```bash

  $ git clone https://github.com/XavYan/lumen-env-dockerized

```
- Creamos el fichero `.env` a partir del `.env.example`.
```bash

  $ cp .env.example .env

```
- Configuramos el fichero `.env` con los datos deseados.

## Configuración de Lumen
Para configurar Lumen, hacemos lo siguiente.

- Nos metemos en la carpeta `src`. Ahí es donde se encuentra Lumen instalado.
- Creamos el fichero `.env` a partir del `.env.example` igual que hicimos con el fichero de entorno de docker-compose.
- Rellenamos los datos necesarios en el `.env`, teniendo en cuenta que los datos proporcionados de la BBDD deben coincidir
con los proporcionados en el `.env` del docker-compose.
  - Como Lumen no tiene por defecto el comando para generar la key de la aplicación (APP_KEY), se puede optar por incluir
  en el fichero `routes/web.php` un endpoint que te devuelva un valor válido para la key.
  ```php
    $router->get('/key', function() {
        return 'APP_KEY=base64:'. base64_encode(\Illuminate\Support\Str::random(32));
    });
  ```
  Con ese código simplemente es acceder a la ruta `/key` de tu aplicación y con ello obtener la APP_KEY.
  Una vez obtenida y puesta en el fichero `.env`, es recomendable eliminar dicha ruta (o protegerla para
  que solo sea accesible en modo desarrollo).
- Una vez realizado estos pasos ya se puede proceder a iniciar los contenedores. Para ello, simplemente ejecutar:
```bash
  $ docker-compose up -d
```
Y si todo ha ido bien acceder a la ruta `localhost:8888` nos devolverá la versión de Lumen.

# Migraciones
Para hacer las migraciones, hay que ejecutar el comando desde el propio contenedor de la siguiente manera:
```bash
  $ docker-compose exec php php /var/www/html/artisan migrate
```