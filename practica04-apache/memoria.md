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


