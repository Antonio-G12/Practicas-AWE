# Instalación de MariaDB
### Nombre: Antonio Velasco
### Curso: 2SMX
### Fecha: 21/10/2026

**Apartado 1**
1. Hay que deshabilitar los sitios virtuales realizados en la practica 2 con:
```bash
sudo a2ensite 000-default.conf
sudo systemctl reload apache2
```
2. Despues actualizamos y comprobamos que Apache este en funcionamiento
```bash
sudo apt update
sudo apt upgrade -y
systemctl is-active apache2
```
<img width="462" height="33" alt="imatge" src="https://github.com/user-attachments/assets/da943b8a-68bf-46a3-a39a-58ed53281fe2" />

**Apartado 2**
1. Instalamos MariaDB y comprobamos la version en la cual esta:
```bash
sudo apt install mariadb-server -y
mariadb --version
```
<img width="788" height="35" alt="imatge" src="https://github.com/user-attachments/assets/264df27a-1595-4ad1-ba27-51c76daaf72e" />

**Apartado 3**
1. Comprobamos que MariaDB funciona
```bash
sudo systemctl status mariadb
systemctl is-enabled mariadb
```
2. Entramos en su base de datos con:
```bash
sudo mariadb
```
3. Una vez que podamos acceder saldremos ya que por el momento no habra que usarlo

<img width="959" height="591" alt="imatge" src="https://github.com/user-attachments/assets/81e611b2-e512-423b-889d-9a0c7d57cd38" />

**Apartado 4**

| Comando                             | Funcion                                               |
|-------------------------------------|-------------------------------------------------------|
| sudo systemctl start mariadb        | Arranca el servicio                                   |
| sudo systemctl stop mariadb         | Detiene el servicio                                   | 
| sudo systemctl restart mariadb      | Reinicia MariaDB                                      |
| sudo systemctl status mariadb       | Muestra su estado actual                              |
| sudo systemctl enable mariadb       | Programa el arranque automatico al iniciar el sistema |

**Apartado 5**
1. Detendremos MariaDB para ver que ocurre (al intentar MariaDB fallará al 
```bash
sudo systemctl stop mariadb
systemctl is-active mariadb
sudo mariadb
```

<img width="959" height="591" alt="imatge" src="https://github.com/user-attachments/assets/a2a39e34-3f7a-4e25-af3d-4a5dc095babb" />

2. Volvemos a iniciarlo
```bash
sudo systemctl start mariadb
systemctl is-active mariadb
```
3.(Pregunta 1): Con sudo systemctl status mariadb y systemctl is-active mariadb

**Apartado 6**
1. Ahora instalamos PHP con lo siguiente (el php -v mostrara la version actual del PHP):
```bash
sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-zip php-intl -y
php -v
```
2. Reiniciamos Apache
```bash
sudo systemctl restart apache2
sudo apache2ctl -M | grep -E "mpm|php"
```
3. Comprobamos que PHP se pueda comunicar con MariaDB y que Apache pueda ejecutar PHP:
```bash
php -m | grep -i mysql
echo '<?php phpinfo();' | sudo tee /var/www/html/info.php
```

<img width="750" height="241" alt="imatge" src="https://github.com/user-attachments/assets/2904adc6-6406-49d4-881d-0e91df4c4f8b" />

<img width="750" height="241" alt="imatge" src="https://github.com/user-attachments/assets/b84b85b8-73f7-4417-b2fe-6041c98690c8" />

4. Borramos el info.php por seguridad y comprobamos que se haya eliminado
```bash
sudo rm /var/www/html/info.php
ls /var/www/html
```
5. (Pregunta 2): Porque muestra datos sensibles como la IP del servidor y la ruta de archivos del servidor. El paquete php-mysql sirve para conectar PHP con MySQL (con la base de datos)

**Apartado 7**
1. Comprobamos que tanto Apache como MariaDb y PHP estan activos para realizar la instalación de WordPress (importante hacer una instantanea en VirtualBox para que en caso de que falle la instalacion volver al punto de inicio)

```bash
systemctl is-active apache2 mariadb
php -v
```

