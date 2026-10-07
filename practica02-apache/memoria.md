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

1. 
