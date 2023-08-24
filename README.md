
# Creando un entorno local de Frappe con Docker



## Prerequisitos

Para empezar con el desarrollo se necesitan los siguientes requisitos:

+ Docker
+ docker-compose
+ Visual Studio Code
## Preparar contenedor para el desarrollo

+ Clonar el directorio frappe_docker
```
git clone https://github.com/frappe/frappe_docker.git
```
+ Entrar al directorio frappe_docker
```
cd frappe_docker
```
+ Copiar la configuración ejemplo del devcontainer de la carpeta devcontainer-example a .devcontainer
```
cp -R devcontainer-example .devcontainer
```
Para un desarrollo más sencillo en Frappe Framework recomiendo el uso de  [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

+ Primero instalar esta extensión antes de empezar.

![Logo](https://github.com/powerprogrammingacademy/install-frappe-docker/blob/main/images/01-install-vscode.png?raw=true)

Antes de abrir el contenedor, pueden cambiar la base de datos a usar en .devcontainer/docker-compose.yml, por defecto viene configurado con MariaDB pero puede usarse con PostgreSQL.

+ Una vez instalada la extensión, VSCode les preguntará si desea abrir el contenedor con la configuración dentro de .devcontainer. También puede hacerlo manualmente de la siguiente forma:

![Logo](https://github.com/powerprogrammingacademy/install-frappe-docker/blob/main/images/02-up-services.png?raw=true)


## Configurar Bench

+ Correr el siguiente comando dentro de la terminal del contenedor.

> Versión 13

```
bench init --skip-redis-config-generation --frappe-branch version-13 frappe-bench
```

> Versión 14
```
bench init --skip-redis-config-generation --frappe-branch version-14 frappe-bench
```

+ Apartir de acá, todos los comandos se ejecutan siempre dentro de la carpeta frappe-bench, sino no va a funcionar.

```
cd /workspace/development/frappe-bench
```
## Configurar hosts

+ Tenemos que decirle a bench que use los contenedores correctos en lugar de localhost. Correr el siguiente comando dentro del cotenedor.

```
bench set-mariadb-host mariadb
bench set-redis-cache-host redis-cache:6379
bench set-redis-queue-host redis-queue:6379
bench set-redis-socketio-host redis-socketio:6379
```
## Editar archivo Procfile
+ Abrir el archivo Procfile y quitar las tres lineas que contienen la configuración de Redis, ya sea manualmente o corriendo el siguiente comando:
```
sed -i '/redis/d' ./Procfile
```


## Crear un nuevo sitio con bench

+ Se puede crear un nuevo sitio con el siguiente comando: ```bench new-site sitename --no-mariadb-socket``` sitename DEBE terminar con .localhost para hacer deploy locales. Ejemplo:
```
bench new-site prueba.localhost --no-mariadb-socket
```
+ El comando pedirá la clave del usuario root de MariaDB. La clave por defecto es ```123```. También te pedirá que definas la clave del usuario ```Administrator``` que se usará para logearse en Frappe, Ejemplo: ```pleasechangeme```

+ Esto creará un nuevo sitio y un directorio ```prueba.localhost``` dentro de ```frappe-bench/sites```.
## Arrancar Frappe

+ Ejecutar el siguiente comando dentro del directorio frappe-bench.

```
bench start
```
