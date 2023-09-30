# Instalación de LAMP con DOCKER.
![ads](https://i.gyazo.com/7601bc9497056eff44e6c9ab6ed149bd.png)

# Primeros pasos
* Creais una nueva carpeta
* Abris la terminal dentro del proyecto
* Escribis ``git clone`` + la URL

![](https://i.gyazo.com/777041711dc5dc4f0c566245ed197f56.png)

``NOTA:`` para los usuarios de Windows probablemente hay que descargarse el docker-desktop para que funcione correctamente.

# Instalación de contenedores
Una vez clonado, ejecutar el comando ```docker-compose up -d``` dentro de la carpeta. Es importante que esteis en la carpeta donde estén los archivos docker.

Con ese comando se os cargará todo lo necesario para hacer que funcione mySQL, apache, y el Xdebuger en docker. 

# Configuración de Xdebug

* Video ilustrativo de como hacer funcionar el Xdebugger: [video de 1min](https://youtu.be/61luX5kWwKo)

Para iniciar el debuger tenéis que ir a la pestaña propia que tiene VScode.

![asd](https://i.gyazo.com/f82931d7403070345b0ed4bdac6e75fc.png)

Después tenéis que establecer un punto de ruptura y seleccionar la siguiente opción: 

![ads](https://i.gyazo.com/ffe1276679a1619a5365f08b7f2ce0e0.png)


![asd](https://i.gyazo.com/118e4c92c0a2c5b8f3310ed9aa788af4.png)

Cuando lo hagáis por primera vez, os pedirá crear la configuración. Le dais a configurar y se os abrira un archivo ```LAUNCH.JSON```. Si no se os abre nada, podéis darle a la rueda de settings y acceder al JSON directamente.

### Copiar y pegar la siguiente configuración en el JSON del debugger:

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

### Explicacion del JSON:

Para que funcione correctamente el Debugger de PHP necesitais especificar correctamente la ruta de vuestra carpeta ``WWW`` o "pathMap". Es decir, si teneis un árbol de trabjo donde teneis Mi_proyecto/docker-lamp/www. Entonces la configuración funcionará.

Sin embargo, si tenéis sub-carpetas o un árbol más complicado debéis modificar la ruta de forma manual y añadirla al JSON. En mi caso por ejemplo mi "WORKSPACEFOLDER" es la carpeta que tengo en el escritorio que se llama 2DAW, pero dentro tengo más subcarpetas y tengo que especificarlo de forma manual tal que así: ``"${workspaceFolder}/Entornos_Servidor/Trimestre 1/docker-lamp/www"``

### Ultimos pasos

El último paso es tener el contenedor docker activo y encontrarse exactamente en la misma página.php donde se va realizar el test. No vale estar en la actividad2.php cuando estás haciendo un punto de ruptura en el index.php (por decir un ejemplo). 

Finalmente, para iniciar el Debugger sigue estos 3 pasos:
* Establece un punto de ruptura
* Dale al play
* Recarga la pagina.php de forma manual en Chrome

Si todo va bien, el VsCode parpadeará y el Debugger empezará a funcionar.

# Configuración de DockerFile

En nuestro caso, lo más relevante sería cambiar la versión PHP por una más antigua o nueva. En caso de que quieras cambiar la versión PHP tan solo debes cambiar la siguiente linea:

```PHP
FROM php:8.2.7-apache
```

a (por ejemplo):

```PHP
FROM php:9.0.1-apache
```

Puedes encontrar las diferentes versiones de PHP en: [dockerhub](https://github.com/docker-library/docs/blob/master/php/README.md#supported-tags-and-respective-dockerfile-links). Busca cualquiera que venga con ```-apache```.

# Configuración de Docker-Compose File

Este fichero nos permitirá cambiar los puertos de nuestro localhost, mysql, phpAdmin, contraseña de mysql entre otras cosas. Si os interesa cambiar los puertos mirar ```ports```.

# Configuracion de archivos

* ```WWW``` representa nuestro contenido. Es donde vamos a poner el index.php y todos nuestros archivos. 
* ```DUMP``` es el repositorio de nuestra base de datos. Cualquier archivo .sql que dejéis ahí se creará con el inicio del contenedor. Sino lo hace, lo podéis hacer de forma manual con phpAdmin o probar rehacer el contenedor con el comando de docker-compose up -d --build.
