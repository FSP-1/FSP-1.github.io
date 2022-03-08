---
layout: post
title:  "Bitnami Wordpress"
date:   2022-03-08 05:26:46 -0600
categories: practicas
---

# Practica-13 de IAW
<div id='id6' />

### **Por Franco Sergio Pereyra 2º ASIR**
# **Índice**
* 1.- [Infractuctura ](#id1)
* 2.- [Instalación](#id2)
* 3.- [Mejoras](#id3)
* 4.- [PhpMyadmin](#id4)

<div id='id1' />

## 1.- Infractuctura
### Ponemos la AMI del ubuntu vacio y con el instance type .small para que tenga almenos 2 GB de RAM
![foto]({{ site.url }}/recursos/20-03-08/img/1.png)

### Le asignamos los puertos SSH, HTTP Y HTTPS
![foto]({{ site.url }}/recursos/20-03-08/img/2.png)

### Añadimos un ddns al la ip publica de la maquina que creamos antes
![foto]({{ site.url }}/recursos/20-03-08/img/3.png)


<div id='id2' />

## 2.- Instalación
### Este seria el script que ultilizaremos para empezar a instalar Bitnami Wordpress

```bash

#!/bin/bash
set -x
#
apt update
#Instalamos las dependncias
apt install -y libncurses5

# Descargamos el instaladador
wget https://bitnami.com/redirect/to/1886150/bitnami-wordpress-5.8.3-0-linux-x64-installer.run?with_popup_skip_signin=1

# Le asignamos permisos de ejecución
chmod +x bitnami-wordpress-5.8.3-0-linux-x64-installer.run?with_popup_skip_signin=1

# Lo movemos
mv bitnami-wordpress-5.8.3-0-linux-x64-installer.run?with_popup_skip_signin=1 /tmp
 
# Lo ejecutamos

/tmp/bitnami-wordpress-5.8.3-0-linux-x64-installer.run?with_popup_skip_signin=1
```
### Nuestra instalación tiene que ser parecida a estas elecciones
![foto]({{ site.url }}/recursos/20-03-08/img/4.png)
### Añadimos cerbot a nuestro dominio con el comando sudo /opt/wordpress-5.8.3-0/bncert-tool
![foto]({{ site.url }}/recursos/20-03-08/img/5.png)
### Con esto tendriamos instalado el wordpress
![foto]({{ site.url }}/recursos/20-03-08/img/6.png)

<div id='id3' />

## 3.- Mejoras
### Al instalar el bitname wordpress, en la pagina principal nos saldra la pagina de instalacion completa de bitname. Para sacarlo usamos el siguiente comando 
> /opt/wordpress-5.8.3-0/apps/wordpress/bnconfig.disabled --appurl / 
### y reseteamos el servicio apache para que se apliquen los pasos con el comando 
>/opt/wordpress-5.8.3-0/ctlscript.sh restart apache 


```bash

#!/bin/bash
set -x

# Con el siguiente comando quitamos la pagina principal de Bitname
/opt/wordpress-5.8.3-0/apps/wordpress/bnconfig.disabled --appurl /

# restart apache
/opt/wordpress-5.8.3-0/ctlscript.sh restart apache
```
![foto]({{ site.url }}/recursos/20-03-08/img/7.png)
### Tambien saldra el banner de bitname al acabar de instalarlo
![foto]({{ site.url }}/recursos/20-03-08/img/8.png)
### Para eliminarlo, modificamos el archivo **httpp-app.conf** y ponemos un # al lado de la linea donde pone Include Banner.conf
![foto]({{ site.url }}/recursos/20-03-08/img/9.png)
![foto]({{ site.url }}/recursos/20-03-08/img/10.png)

<div id='id4' />

## 4.- PhpMyadmin
### Para poder conectar con phpMyAdmin tendremos que realizar un túnel SSH desde nuestra máquina con la máquina remota.
![foto]({{ site.url }}/recursos/20-03-08/img/11.png)
### Para ver que funciona ponemos en nuestro navejador web
>http://localhost:8888/phpmyadmin

![foto]({{ site.url }}/recursos/20-03-08/img/12.png)

[Volver hacia arriba](#id6)
