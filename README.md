# Instalación de LAMP con DOCKER.
![ads](https://i.gyazo.com/7601bc9497056eff44e6c9ab6ed149bd.png)

# Primeros pasos
Lo primero que hay que hacer es descargarse el git y el docker desktop. Lo podéis hacer en las páginas oficiales [aqui git](https://git-scm.com/downloads) y [aqui docker](https://www.docker.com/products/docker-desktop/).

Una vez que instaléis los programas, creais una carpeta en el escritorio y la abris con Visual Studio Code. Después os toca abrir la terminal con ```F1``` y escribir git clone https://github.com/Vostenznuk/docker-lamp.git

# Instalación de contenedores
No hay que hacer nada más que ejecutar el comando ```docker-compose up -d``` dentro de la carpeta docker-lamp que acabáis de descargar.

Con ese comando se os cargará todo lo necesario para hacer que funcione mySQL, apache, y demás en docker. Ahora explicaré los *detalles* de algunos comandos para que sepáis como configurarlo a vuestro gusto.

# Configuración de Xdebug

```Podéis hacer SKIP a todo el texto mirando este``` [video de 1min](https://youtu.be/61luX5kWwKo)

Para iniciar el debuger tenéis que ir a la pestaña propia que tiene VScode.

![asd](https://i.gyazo.com/f82931d7403070345b0ed4bdac6e75fc.png)

Después tenéis que establecer un punto de ruptura y seleccionar la siguiente opción: 

![ads](https://i.gyazo.com/ffe1276679a1619a5365f08b7f2ce0e0.png)


![asd](https://i.gyazo.com/118e4c92c0a2c5b8f3310ed9aa788af4.png)

Cuando lo hagáis por primera vez, os pedirá crear la configuración. Le dais a configurar y se os abrira un archivo ```LAUNCH.JSON```.

Donde tiene que tener esta configuración:

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
                "/var/www/html": "${workspaceFolder}/docker-lamp/www"
              }
        }
    ]
}
```

Si la primera no funciona, probar esta y también probad cambiar la carpeta de .vscode de lugar.

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

El último paso es tener el contenedor corriendo y encontrarse exactamente en la misma página donde vas a correr el test. No vale estar en la actividad2.php cuando estás haciendo un punto de ruptura en la actividad1.php.

Ahora, hacer click en el boton verde tenéis que ir a la página.php que corresponda y recargarla (F5) de forma manual. Si todo va bien la ventana del VScode parpadeará anunciando el comienzo del debugger.

Si aun así no funciona...preguntad al chatGPT.

# Configuración de DockerFile

Es donde podemos instalar programas a nuestro Linux. En nuestro caso, lo más relevante sería cambiar la versión PHP por una más antigua o nueva. En caso de que quieras cambiar la versión PHP tan solo debes cambiar la siguiente linea:

```PHP
FROM php:8.2.7-apache
```
Puedes encontrar las diferentes versiones de PHP en: [dockerhub](https://github.com/docker-library/docs/blob/master/php/README.md#supported-tags-and-respective-dockerfile-links). Busca cualquiera que venga con```-apache```.

# Configuración de Docker-Compose File
Este fichero nos permitirá cambiar los puertos de nuestro localhost, mysql, phpAdmin, contraseña de mysql entre otras cosas. Si os interesa cambiar los puertos mirar ```ports```.

# Configuracion de archivos
La estructura de carpetas y su significado es el siguiente:
* ```WWW``` representa nuestro contenido. Es donde vamos a poner el index.php y todos nuestros archivos.
* ```DUMP``` es el repositorio de nuestra base de datos. Cualquier archivo .sql que dejéis ahí se creará con el inicio del contenedor. Sino lo hace, lo podéis hacer de forma manual con phpAdmin o probar rehacer el contenedor con el comando de docker-compose up -d --build.