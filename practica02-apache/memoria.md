# Instalación de Apache en Ubuntu Server
**Apartado 1**
1. Primero se realizará la preparación para la instalación comprobando que la MV tenga los paquetes actualizados
2. Se realizará con (```bash sudo apt update```) y (```bash sudo apt update -y```)
3. Después se comprobará la versión del sistema con (```bash lsb_relase -a) <img width="351" height="104" alt="image" src="https://github.com/user-attachments/assets/8d32338d-1d1d-42d9-8e55-201ffe5bc033" />

**Apartado 2**

1. Ahora procederemos con la instalación de Apache. Para ello se tiene que usar el siguiente comando: (```bash sudo apt install apache2 -y) y para comprobar la versión instalada se usará (```bash apache2 -v)
2. (Pregunta 1): Son: apache2-bin, apache2-data, apache2-utils, varías librerias (como libapr1 y demás) y dependencias de sistema y red como ssl-cert, iana-etc y netbase

**Apartado 3**

1. Ahora mediante el comando (```bash sudo systemctl status apache2) comprobaremos que el servicio se encuentre activo.
2. Luego con (```bash sudo ss -tulpn | grep apache 2) se comprobaran los puertos que esten en escucha
3. Y por último probamos desde la terminal con (```bash curl -I http://localhost) y desde un navegador (introduciendo htpp://IP del servidor). Al entrar desde un navegador saldrá esto: <img width="890" height="916" alt="image" src="https://github.com/user-attachments/assets/2ef7a1a1-29df-4d2d-9b6a-2ed366200089" />
