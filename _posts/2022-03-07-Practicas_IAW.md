---
layout: post
title:  "Practicas"
date:   2022-03-07 05:26:46 -0600
categories: practicas
---


# Practica-16 de IAW
<div id='id6' />

### **Por Franco Sergio Pereyra 2º ASIR**
# **Índice**
* 1.- [Infractuctura ](#id1)
* 2.- [Instalación](#id2)
* 3.- [Comprobación](#id3)

<div id='id1' />

## 1.- Infractuctura
### Ponemos la AMI del ubuntu vacio y con el instance type .medium para que tenga almenos 2 GB de RAM y 10 GB de memoria 
![foto]({{ site.url }}/recursos/20-03-07/img/2.png)

### Le asignamos los puertos SSH, HTTP Y HTTPS
```
# Cramos una regla de accesso   SSH
    aws ec2 authorize-security-group-ingress \
    --group-name frontend-sg \
    --protocol tcp \
    --port 22 \
    --cidr 0.0.0.0/0

    # Cramos una regla de accesso  HTPP
    aws ec2 authorize-security-group-ingress \
    --group-name frontend-sg \
    --protocol tcp \
    --port 80 \
    --cidr 0.0.0.0/0

    # Cramos una regla de accesso   HTPPS
    aws ec2 authorize-security-group-ingress \
    --group-name frontend-sg \
    --protocol tcp \
    --port 443 \
    --cidr 0.0.0.0/0
```


<div id='id2' />

## 2.- Instalación bitnami prestashop con docker-compose
### Creamos el fichero  docker-compose.yml con las variables adecuadas para que prestashop se instale por su cuenta

```
version: '3.3'

services:
  prestashop:
    image: bitnami/prestashop:1.7
    environment:
      - PRESTASHOP_FIRST_NAME=${PRESTASHOP_FIRST_NAME}
      - PRESTASHOP_LAST_NAME=${PRESTASHOP_LAST_NAME}
      - PRESTASHOP_DATABASE_HOST=${PRESTASHOP_DATABASE_HOST}
      - PRESTASHOP_DATABASE_USER=${MYSQL_USER}
      - PRESTASHOP_DATABASE_PASSWORD=${MYSQL_PASSWORD}
      - PRESTASHOP_DATABASE_NAME=${MYSQL_DATABASE}
      - PRESTASHOP_HOST=${PRESTASHOP_HOST}
      - PRESTASHOP_ENABLE_HTTPS=${PRESTASHOP_ENABLE_HTTPS}
      - PRESTASHOP_COUNTRY=${PRESTASHOP_COUNTRY:-es}
      - PRESTASHOP_LANGUAGE=${PRESTASHOP_LANGUAGE:-es}
      - PRESTASHOP_EMAIL=${PRESTASHOP_EMAIL}
      - PRESTASHOP_PASSWORD=${PRESTASHOP_PASSWORD}
    restart: always
    volumes:
      - prestashop_data:/bitnami/prestashop
    depends_on:
      - mariadb
    networks:
      - frontend_network
      - backend_network
  
  mariadb:
    image: mariadb:10.4
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
    volumes:
      - mariadb_data:/var/lib/mariadb
    networks:
      - backend_network
    security_opt:
      - seccomp:unconfined

  phpmyadmin:
    image: phpmyadmin:5
    restart: always
    ports:
      - 8080:80
    environment:
      - PMA_HOST=mariadb
    depends_on:
      - mariadb
    networks:
      - frontend_network
      - backend_network

  https-portal:
    image: steveltn/https-portal:1
    ports:
      - 80:80
      - 443:443
    environment:
     # DOMAINS: 'localhost -> http://prestashop:8080 #local'
      DOMAINS: ' bitnamifranco.ddns.net -> http://prestashop:8080 #production'
    volumes:
      - ssl_certs_data:/var/lib/https-portal
    depends_on:
      - prestashop
    restart: always
    networks:
      - frontend_network

volumes:
  prestashop_data:
  mariadb_data:
  ssl_certs_data:

networks:
  frontend_network:
  backend_network:

```
### Creamos el fichero .env y le añadimos las variables
![foto]({{ site.url }}/recursos/20-03-07/img/3.png)


<div id='id3' />

## 3.- Comprobación
### Pagina principal
![foto]({{ site.url }}/recursos/20-03-07/img/1.png)
### Pagina administrativa de prestashop
![foto]({{ site.url }}/recursos/20-03-07/img/4.png)
![foto]({{ site.url }}/recursos/20-03-07/img/5.png)

<div id='id4' />


[Volver hacia arriba](#id6)
