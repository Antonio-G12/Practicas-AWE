# Instalación de Apache
### Nombre: Antonio Velasco
### Curso: 2SMX
### Fecha: 23/10/26

**Apartado 1**

1. Averiguamos la IP del servidor con:
```bash
hostname -I
```

2. Usamos el ssh desde la terminal anfitrion (a partir de aqui usaremos esta terminal para los ejercicios):

```bash
ssh user@IP server
```

3. Comprobamos que tanto Apache como MariaDB sigue funcionando:

```bash
systemctl is-active apache2 mariadb
php -v
```
<img width="827" height="750" alt="imatge" src="https://github.com/user-attachments/assets/31485734-8dd8-4ee5-b8e3-613c50175047" />

4.(Pregunta 1): SSH y Apache. SSH sirve para controlar el servidor incluso de forma remota, mientras que Apache sirve para mostrar la web y actuar como el sistema de este.

**Apartado 2**

1. Cambiamos los sitios que esten activos (si esta todo en orden configtest debe soltar lo siguiente: "Syntax OK"

```bash
sudo a2dissite sitio1.conf sitio2.conf
sudo a2ensite 000-default.conf
sudo apache2ctl configtest
sudo systemctl reload apache2
```

#En caso de que falle, realizaríamos lo siguiente para solucionarlo

```bash
echo "ServerName localhost" | sudo tee /etc/apache2/conf-available/servername.conf 
sudo a2enconf servername 
sudo apache2ctl configtest 
sudo systemctl reload apache2
```

2. Comprobamos que solo hay un sitio activo (en este caso 000-default.conf) y la carpeta que utiliza

```bash
ls /etc/apache2/sites-enabled/
grep DocumentRoot /etc/apache2/sites-enabled/000-default.conf
```

3. Entramos en la pagina web desde el sistema (debe de aparecer la pagina por defecto, apareciendo o "Apache2 Ubuntu Default Page" o "Servidor de NOMBRE" (en este caso es el segundo por haber modificado el archivo sitio1.local en practicas anteriores. Ahora no es necesario este archivo)

```bash
http://IP Servidor
```


<img width="700" height="64" alt="imatge" src="https://github.com/user-attachments/assets/7eba6e1e-9462-485b-b66f-44e4a971a7d7" />


<img width="954" height="968" alt="imatge" src="https://github.com/user-attachments/assets/6fb886e9-5e31-485d-8d14-29e2ce08da77" />


4.(Pregunta 2): En que a2dissite lo desactiva, pero el fichero permanece. Por el 000-default.conf

**Apartado 3**

1. Crearemos la base de datos de Wordpress y la guardaremos dentro de MariaDB. Para ello entraremos en MariaDB

```bash
sudo mariadb
```

2. Con los siguientes comandos se crearan la base de datos y el usuario para acceder (al igual que una contraseña)
#El primero lo que hará es crear la base de datos, luego el segundo la llave de acceso por así decirlo y el tercero le otorga permisos a esa misma llave

```bash
CREATE DATABASE wordpress;
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'Ejemplo_contraseña';
GRANT ALL ON wordpress.* TO 'wp_user'@'localhost';
```

#En la primera foto salta error pero porque he puesto el segundo comando otra vez, pero al ejecutarlo por primera vez no ha dado error. El resto solo los he ejecutado una única vez 

<img width="659" height="366" alt="image" src="https://github.com/user-attachments/assets/e9739f24-983e-4bd3-abfb-9cdfa47a2567" />

3. Entraremos para ver que funciona

```bash
mariadb -u wp_user -p wordpress
```

<img width="688" height="242" alt="image" src="https://github.com/user-attachments/assets/34ef1b9d-3fda-429c-ba51-6432a38257f5" />

4. (Pregunta 3): Por evitar ataques y problemas de seguridad

**Apartado 4**

1. Instalamos WordPress dentro de la carpeta /tmp.

```bash
sudo apt install wget -y
cd /tmp
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
ls wordpress
```

2. Luego movemos el contenido a la carpeta del sitio web.

```bash
sudo rm /var/www/html/index.html
sudo cp -a /tmp/wordpress/. /var/www/html/
rm -r /tmp/wordpress /tmp/latest.tar.gz
ls /var/www/html
```

3. (Pregunta 4): Los primeros comandos sirven para, en el caso del primero de estos, instalar wget (ya instalado en mi caso), el siguiente para moverse a /tmp, el otro para conseguir el archivo comprimido de la pagina de WordPress y los ultimos de estos para descomprimir y ver el contenido de la carpeta. Los siguientes tanto para borrar como para copiar

4. Le damos los siguientes permisos

```bash
sudo chown -R www-data:www-data /var/www/html
sudo find /var/www/html -type d -exec chmod 755 {} \;
sudo find /var/www/html -type f -exec chmod 644 {} \;
ls -ld /var/www/html
```

<img width="669" height="404" alt="image" src="https://github.com/user-attachments/assets/cc2c0383-56e0-4085-b014-207993cf21c3" />

5. (Pregunta 5): Estos comandos lo que hacen es dar permisos (en el caso del chown -R, el find lo hace igual solo que busca en el directorio los archivos concretos para darle permiso (el -exec es para ejecutar el comando sin confirmacion). Porque WordPress no podrá ni crear ni modificar archivos, únicamente podrá leerlos. Puede fallar las actualizaciones y causar errores al subir archivos

**Apartado 5**

1. Ahora procedemos a la instalación de WordPress en el sitio web (que se accede con http://IP_Servidor) accediendo a su carpeta desde el indice de la web (las siguientes capturas muestran los pasos mas importantes)

<img width="800" height="800" alt="image" src="https://github.com/user-attachments/assets/bc51f72f-2b07-4807-8619-9ba641e531bd" />

<img width="764" height="801" alt="image" src="https://github.com/user-attachments/assets/744c0272-8637-4eae-bbf1-a85d727b44ab" />

<img width="840" height="791" alt="image" src="https://github.com/user-attachments/assets/0c7aa158-2e46-4b78-9975-2394dcb81b9a" />

2. (Pregunta 6): La parte de instalación en terminal mediante los comandos del anterior apartado y en el navegador ya la parte más grafica (configuración de idioma, del sitio web y del usuario). Porque en el servidor no hace falta ejecutar un programa como tal ya que va por comandos (aparte de que sin interfaz gráfica consume menos recursos)

**Apartado 6**

1. Abrimos el sitio web otra vez para ver el titulo y para entrar en Wordpress al panel de admin con: "http://IP_Servidor/wp-admin/"

2. Publicaremos una entrada y comprobaremos el resultado. Luego para ver la entrada guardada en terminal se hara con lo siguiente:

```bash
sudo mariadb wordpress -e "SHOW TABLES;"
```

<img width="859" height="613" alt="image" src="https://github.com/user-attachments/assets/22775f31-0c4f-4ed2-b5c8-b46260913409" />

<img width="555" height="345" alt="image" src="https://github.com/user-attachments/assets/8c80755f-9be3-4f79-bc5f-dcc1c3dc93c3" />

3. (Pregunta 7): En base de datos (en especifico la de WordPress de MariaDB). Con el comando anterior

**Apartado 7** 

1. Editaremos la configuración con lo siguiente

```bash
sudo nano /var/www/html/wp-config.php
```

Y modificaremos estas lineas del archivo: 

```bash
define( 'WP_HOME',    'http://' . $_SERVER['HTTP_HOST'] );
define( 'WP_SITEURL', 'http://' . $_SERVER['HTTP_HOST'] );
```

2. Lo guardaremos y comprobaremos con:

```bash
php -l /var/www/html/wordpress/wp-config.php
```

para ver que no hayan errores de sintaxis. Ahora la pagina causara un error404 al entrar. Esto se soluciona en el siguiente apartado

<img width="678" height="150" alt="image" src="https://github.com/user-attachments/assets/bd0c01ba-2de8-4fc1-a5c4-4ca5f6b56e9a" />

<img width="590" height="47" alt="image" src="https://github.com/user-attachments/assets/b9507efd-3460-40c5-a671-ca3b7f158ab8" />


**Apartado 8**

1.


