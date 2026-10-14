# Instalación de Apache en Ubuntu Server
**Apartado 1**
1. Primero se realizará la preparación para la instalación comprobando que la MV tenga los paquetes actualizados
2. Se realizará con:
 ```bash
 sudo apt update
 sudo apt update -y
```
3. Después se comprobará la versión del sistema con:
 ```bash
 lsb_relase -a
```
<img width="351" height="104" alt="image" src="https://github.com/user-attachments/assets/8d32338d-1d1d-42d9-8e55-201ffe5bc033" />

**Apartado 2**

1. Ahora procederemos con la instalación de Apache. Para ello se tiene que usar el siguiente comando:
 ```bash
sudo apt install apache2 -y
```
Y para comprobar la versión instalada se usará
```bash
 apache2 -v
```
2. (Pregunta 1): Son: apache2-bin, apache2-data, apache2-utils, varías librerias (como libapr1 y demás) y dependencias de sistema y red como ssl-cert, iana-etc y netbase

**Apartado 3**

1. Ahora mediante el comando

 ```bash
 sudo systemctl status apache2
```
Comprobaremos que el servicio se encuentre activo.

2. Luego con 
```bash
 sudo ss -tulpn | grep apache 2
```
 se comprobaran los puertos que esten en escucha
 
3. Y por último probamos desde la terminal con 
```bash 
curl -I http://localhost
```
 y desde un navegador (introduciendo htpp://IP del servidor). Al entrar desde un navegador saldrá esto: <img width="890" height="916" alt="image" src="https://github.com/user-attachments/assets/2ef7a1a1-29df-4d2d-9b6a-2ed366200089" />
3. Comprobaremos si el firewall está activo con estos comandos:
```bash
sudo ufw status
sudo ufw allow 'Apache'
```
4. (Pregunta 2): La diferencia radica en los puertos que abren y su uso para webs (si quieres que transmita un trafico no cifrado, cifrado o ambos) 

**Apartado 4**

1. Ahora se hará una prueba sobre los comandos de administración de Apache

| Comando                        | Función                                    |
|--------------------------------|--------------------------------------------|
| sudo systemctl start apache2   | Inicia el servicio                         |
| sudo systemctl stop apache2    | Detiene el servicio                        |
| sudo systemctl restart apache2 | Reinicia el servicio                       |
| sudo systemctl reload apache2  | Recarga sin cortar la conexión             |
| sudo systemctl enable apache2  | Activa el arranque automatico de Apache    |
| sudo systemctl disable apache2 | Desactiva el arranque automatico           |
| apache2ctl configtest          | Comprueba la sintaxis                      |
| apache2ctl -S                  | Muestra los host cargados                  |
| apache2ctl -M                  | Muestra los modulos cargados               |
| a2enmod / a2dismod             | Activa o desactiva módulos                 |
| a2ensite / a2dissite           | Activa o desactiva sitios                  |
| a2enconf / a2disconf           | Activa o desactiva configuraciones         |

1. (Pregunta 3):Cuando no sea necesario reiniciar sin cortar conexiones

**Apartado 5**

1. Analizamos la estructura de los archivos de Apache con:
```bash
ls -l /etc/apache2
```
<img width="471" height="190" alt="imatge" src="https://github.com/user-attachments/assets/d253e05c-83af-4a7f-9636-b2822c7434b4" />

2. Luego observaremos las rutas de la tabla para saber que contienen

| Ruta                                         | Descripción                                         |
|----------------------------------------------|-----------------------------------------------------|
| /etc/apache2/apache2.conf                    | Contiene la configuración principal de Apache       |
| /etc/apache2/ports.conf                      | Indica los puertos por los que escucha              |
| /etc/apache2/sites-available/                | Enlace simbolico a los sitios disponibles definidos |
| /etc/apache2/sites-enabled/                  | Enlace a los sitios activos (desde sites-avaliable) |
| /etc/apache2/mods-available/ y mods-enabled/ | Enlaces a los modulos disponibles y activos         |
| /etc/apache2/conf-available/ y conf-enabled/ | Enlaces a las configuraciones disponibles y activas |
| /etc/apache2/envvars                         | Muestra las variables de entorno                    |
| /var/www/html/                               | Directorio raiz establecido por defecto             |
| /var/log/apache2/access.log                  | Muestra el registro de accesos                      |
| /var/log/apache2/error.log                   | Muestra el registro de errores                      |

3. Por último comprobamos que los ficheros de sites-enabled sean enlaces simbolicos con:
```bash
ls -l /etc/apache2/sites-enabled/
```

4. (Pregunta 4): Porque facilita la gestión y la alta disponibilidad de los sitos web

**Apartado 6** 

1. Antes de modificar archivos hacemos una copia de seguridad del fichero de configuracion con este comando:

```bash
sudo cp /etc/apache2/apache2.conf /etc/apache2/apache2.conf.bak
```

2. Procedemos a cambiar la pagina de inicio con el siguiente comando:

```bash
echo "<h1>Servidor de Nombre</h1>" | sudo tee /var/www/html/index.html
```

3. Después cambiamos los puertos de escucha modificando los 2 archivos con nano:

```bash
sudo nano /etc/apache2/ports.conf
sudo nano /etc/apache2/sites-available/000-default.conf
```

4. Tras cambiar los puertos de escucha introduciremos los siguientes comandos para verificar los cambios (luego en los anteriores archivos volveremos a dejar los puertos de escucha como estaban por defecto):

```bash
sudo apache2ctl configtest
sudo systemctl reload apache2
curl -I http://localhost:8080
```

5. Definimos el nombre del servidor con:
```bash
echo "ServerName localhost" | sudo tee /etc/apache2/conf-available/servername.conf
sudo a2enconf servername
sudo systemctl reload apache2
```
6. Y después de usar los comandos, personalizaremos la pagina de error y cambiaremos el correo de administrador modificando un archivo (en mi caso, sitio1.conf)

<img width="918" height="518" alt="image" src="https://github.com/user-attachments/assets/7e70d89d-4523-4eef-b968-a18becdb35f9" />



**Apartado 7**

1. Observamos los modulos que se cargan según la necesidad con lo siguiente:
```bash
apache2ctl -M
ls /etc/apache2/mods-available/
```
2. Activamos y comprobamos los módulos habituales con:

```bash
sudo a2enmod rewrite
sudo a2enmod headers
sudo a2enmod ssl
sudo systemctl restart apache2
apache2ctl -M | grep -E "rewrite|headers|ssl"
```
3. Analizamos los siguientes módulos:

| Módulo                                       | Función                                             |
|----------------------------------------------|-----------------------------------------------------|
| mod_rewrite                                  | Reescribe la URL                                    |
| mod_headers                                  | Manipula las cabezeras HTTP                         |
| mod_ssl                                      | Activa el soporte a HTTPS                           |
| mod_dir                                      | Gestiona los ficheros índice                        |
| mod_alias                                    | Alias de rutas                                      |
| mod_auth_basic                               | Activa la auteniticación                            |
| mod_status                                   | Muestra el estado del servidor                      |
| mod_php / PHP-FPM                            | Ejecuta PHP                                         |

4. Desactivamos un módulo (status en este caso) para ver que ocurre:

```bash
sudo a2dismod status
sudo systemctl restart apache2
```
5. (Pregunta 5): Fallará el inicio o reinicio del servidor


**Apartado 8**

1. Se va a proceder a crear dos sitios (sitio1.local y sitio2.local). Primero se crearán los directorios y las páginas
```bash
sudo mkdir -p /var/www/sitio1 /var/www/sitio2
echo "<h1>Bienvenido a SITIO 1</h1>" | sudo tee /var/www/sitio1/index.html
echo "<h1>Bienvenido a SITIO 2</h1>" | sudo tee /var/www/sitio2/index.html
```
2. Creamos los ficheros de configuración para sitio1 y sitio2 (para sitio2 se usan los mismos comandos que para sitio1)
```bash
sudo nano /etc/apache2/sites-available/sitio1.conf
<VirtualHost *:80>
    ServerName sitio1.local
    ServerAdmin admin@sitio1.local
    DocumentRoot /var/www/sitio1

    <Directory /var/www/sitio1>
        Options -Indexes +FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/sitio1_error.log
    CustomLog ${APACHE_LOG_DIR}/sitio1_access.log combined
</VirtualHost>
```
3. Activamos el sitio por defecto para activar los creados
```bash
sudo a2ensite sitio1.conf sitio2.conf
sudo a2dissite 000-default.conf
sudo apache2ctl configtest
sudo systemctl reload apache2
apache2ctl -S
```
4. Modificamos /etc/hosts añadiendo lo siguiente
```bash
127.0.0.1   sitio1.local sitio2.local
```
5. Comprobamos que se ha aplicado correctamente
```bash
curl http://sitio1.local
curl http://sitio2.local
```
<img width="748" height="386" alt="imatge" src="https://github.com/user-attachments/assets/08e76a73-6e5f-4e59-9cc3-df79f8cbceec" />

6. (Pregunta 6): En que las que se basan en IP solo se aplica una IP única para cada sitio mientras que las de nombre solo basta una IP (ya que se asigna a múltiples sitios web). La de nombre

**Apartado 9**

1. Comprobamos que Apache se ejecuta con el usuario www-data con lo siguiente:
```bash
ps aux | grep apache2
grep -E "APACHE_RUN_(USER|GROUP)" /etc/apache2/envvars
ls -ld /var/www/sitio1 /var/www/sitio1/index.html
```

2. Aplicamos permisios:
#En el primer chmod, el 755 se usa para dar permisos competos al usuario principal y de lectura y ejecucción al resto, el 644 después es para dar permisos de lectura y escritura para el usuario, y de lectura para el resto
#No ponemos 777 para evitar dar permisos a todos los del sistema (de la misma manera de que no puede pertenecer a root), para así evitar fallos críticos

```bash
sudo chown -R www-data:www-data /var/www/sitio1
sudo find /var/www/sitio1 -type d -exec chmod 755 {} \;
sudo find /var/www/sitio1 -type f -exec chmod 644 {} \;
```
#Ahora hay que cambiar los permisos con sudo chmod 000 /var/www/sitio1/index.html (luego restaurar con 644 los permisos) para causar el error 403. Luego hay que entrar en el log de sitio1 para comprobar que el error se ha registrado.

<img width="955" height="903" alt="image" src="https://github.com/user-attachments/assets/c58d292c-510f-4edd-a742-3e61e7aa0a07" />

<img width="1277" height="97" alt="image" src="https://github.com/user-attachments/assets/0e9c85f6-c215-4ba0-aef2-811fcc586c5d" />



