# Instalación de MariaDB
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

