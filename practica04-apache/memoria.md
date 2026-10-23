# Instalación de Apache
## Nombre: Antonio Velasco
## Curso: 2SMX
## Fecha: 23/10/26

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

4.(Pregunta 1): 
