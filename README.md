# Instalación de LAMP con DOCKER.
![ads](https://i.gyazo.com/7601bc9497056eff44e6c9ab6ed149bd.png)

# Primeros pasos
Lo primero que hay que hacer es descargaste el git y el docker desktop. Lo podéis hacer en las páginas oficiales [aqui git](https://git-scm.com/downloads) y [aqui docker](https://www.docker.com/products/docker-desktop/).

Una vez que instaléis los programas, creais una carpeta en el escritorio y la abris con Visual Studio Code. Después os toca abrir la terminal con ```F1``` y escribir git clone https://github.com/Vostenznuk/docker-lamp.git

# Instalación de contenedores
No hay que hacer nada más que ejecutar el comando ```docker-compose up -d``` dentro de la carpeta docker-lamp que acabáis de descargar.

Con ese comando se os cargará todo lo necesario para hacer que funcione mySQL, apache, y demás en docker. Ahora explicaré los *detalles* de algunos comandos para que sepáis como configurarlo a vuestro gusto.

# Configuración de Xdebug
Para empezar explicaré como funciona. Para iniciar el debuger tenéis que ir a la pestaña propia que tiene VScode.

![asd](https://i.gyazo.com/f82931d7403070345b0ed4bdac6e75fc.png)

Después tenéis que establecer un punto de ruptura y seleccionar la siguiente opción: 

![ads](https://i.gyazo.com/ffe1276679a1619a5365f08b7f2ce0e0.png)

El último paso es tener el contenedor corriendo y encontrarse exactamente en la misma página donde vas a correr el test. No vale estar en la actividad2.php cuando estás haciendo un punto de ruptura en la actividad1.php.

Si no se activa automáticamente, una ves seleccionado, tenéis que ir a la página.php que corresponda y recargarla (F5) de forma manual.

Si aún así no va bien, significa que la configuración del JSON no esta configurada correctamente. Aseguraros de que tenéis la carpeta .vscode dentro de docker-lamp y que el código sea o configurarlo de forma manual dentro de VScode copiando y pegando esta configuración:

```PHP
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for Xdebug",
            "type": "php",
            "request": "launch",
            "port": 9003, 
            "pathMappings": {
                "/var/www/html": "${workspaceFolder}/../docker-lamp/www"
              }
        }
    ]
}
```

Si aun así no funciona...preguntad al chatGPT.

# Configuración de DockerFile

Es donde podemos instalar programas a nuestro Linux. En nuestro caso, lo más relevante sería cambiar la versión PHP por una más antigua o nueva. En caso de que quieras cambiar la versión PHP tan solo debes cambiar la siguiente linea:

```PHP
FROM php:8.2.7-apache
```
Puedes encontrar las diferentes versiones de PHP en: [dockerhub](https://github.com/docker-library/docs/blob/master/php/README.md#supported-tags-and-respective-dockerfile-links). Busca cualquiera que venga con```-apache```.

# Configuración de Docker-Compose File
Este fichero nos permitirá cambiar los puertos de nuestro localhost, mysql, phpAdmin, contraseña de mysql entre otras cosas. Si os interesa cambiar los puertos mirar ```ports````.

# Configuracion de archivos
La estructura de carpetas y su significado es el siguiente:
* ```WWW``` representa nuestro contenido. Es donde vamos a poner el index.php y todos nuestros archivos.
* ```DUMP``` es el repositorio de nuestra base de datos. Cualquier archivo .sql que dejéis ahí se creará con el inicio del contenedor. Sino lo hace, lo podéis hacer de forma manual con phpAdmin o con el comando de docker-compose up -d --build.