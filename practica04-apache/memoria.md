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





