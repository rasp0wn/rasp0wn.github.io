---
title: "Cómo instalar WordPress en una Raspberry"
date: 2021-08-03T20:33:30-04:05
categories:
  - blog
tags:
  - raspberry
  - wordpress
---
En esta ocasión vamos instalar <a href="https://es.wordpress.org/">WordPress</a> en una Raspberry Pi para crear nuestra web de forma sencilla sin necesidad de saber programación (ni si quiera en HTML, CSS, PHP ni Javascript).

Por si alguien no lo sabe, WordPress es lo que se conoce como un sistema de gestión de contenidos o CMS por sus siglas en inglés. Básicamente es una plataforma hecha en el lenguaje de programación PHP que nos permitirá crear páginas web sin necesidad de programar gracias a sus numerosos plugins (rasp0wn por ejemplo está hecha con esta plataforma) aunque si somos usuarios avanzados con conocimientos en estos lenguajes se le puede sacar más partido aún.

## Configuración e instalación de WordPress en Raspberry

Para instalar WordPress en la Raspberry (<a href="https://rasp0wn.github.io/blog/como-instalar-un-sistema-operativo-en-raspberry/">con Raspbian como sistema operativo</a>) vamos a necesitar primero un servidor HTTP (utilizaremos Apache aunque se puede usar Nginx por ejemplo). También necesitaremos instalar un gestor de bases de datos como MariaDB, PHP para ejecutar las páginas de la web y por último descargar e instalar la plataforma de WordPress propiamente dicha.

Ejecutaremos los siguientes comandos:

```
sudo apt-get install apache2 -y
sudo apt-get install php -y
sudo apt-get install mariadb-server php-mysql -y
sudo service apache2 restart
```

Con esto tendremos Apache, PHP y MariaDB instalados. A continuación descargamos WordPress en la ruta /var/www/html con los siguientes comandos:

```
cd /var/www/html/
sudo rm *
sudo wget http://wordpress.org/latest.tar.gz
sudo tar xzf latest.tar.gz
sudo mv wordpress/* .
sudo rm -rf wordpress latest.tar.gz
sudo chown -R www-data: .
```

Tras tener WordPress descomprimido en la ruta indicada solo nos quedaría terminar de configurar MariaDB para poder meternos en el panel de configuración y crear nuestra propia web.

## Configuración de MariaDB

WordPress utiliza una base de datos SQL para poder funcionar correctamente. Por lo tanto tendremos que configurar MariaDB y conectarla con nuestra página ejecutando el siguiente comando:

```
sudo mysql_secure_installation
```

Cuando lo introduzcamos se nos preguntará por una serie de cuestiones a la que tendremos que responder de esta forma:

* Primero nos pedirá configurar la contraseña de root (si ya teníamos una nos dirá si queremos dejarla o cambiarla).
* Después nos preguntará: «Remove anonymous users?» Presionamos ‘Y‘ para eliminar los usuarios anónimos.
* Continuamos con: «Disallow root login remotely?» Presionamos ‘Y‘ para desactivar el login como root de forma remota.
* A continuación: «Remove test database and access to it?» Presionamos ‘Y‘ para eliminar la base de datos de test.
* Y finalmente: «Reload privilege tables now?» Presionamos ‘Y‘ para refrescar los privilegios de las tablas.

![mariadb_config](/assets/images/posts/mariadb_config.png)

Cuando finalice la configuración nos aparecerá un mensaje del tipo «All done!» y «Thanks for using MariaDB!«. Ahora tenemos que entrar en MariaDB para crear la base de datos que usaremos en WordPress y concederle los permisos necesarios. Tras ejecutar el primer comando nos pedirá la contraseña que hemos configurado anteriormente para «root» y ya podremos ejecutar los demás (en el tecer comando tenemos que cambiar «CONTRASEÑA» por la password de root):

```
sudo mysql -uroot -p 
create database wordpress;
GRANT ALL PRIVILEGES ON wordpress.* TO 'root'@'localhost' IDENTIFIED BY 'CONTRASEÑA';
FLUSH PRIVILEGES;
exit
```

![mariadb_config2](/assets/images/posts/mariadb_config2.png)

## Accediendo a WordPress

Ya tenemos todo configurado así que lo único que falta es acceder a nuestra web y terminar el proceso de instalación de WordPress. Para ello desde un navegador nos vamos a «http://IP_RASPBERRY» o bien si estamos desde la propia Raspberry podemos ir a «localhost«.

![wordpress_config](/assets/images/posts/wordpress_config.png)

Seleccionamos el idioma en el que queramos usar WordPress, en mi caso seleccionaré español, en el mensaje que sale después le damos al botón de «¡Vamos a ello!«  y nos aparecerá la siguiente pantalla en la que tendremos que introducir los datos correspondientes:

![wordpress_config3](/assets/images/posts/wordpress_config3.png)

A continuación le damos a «Ejecutar la instalación« y nos aparecerá la pantalla donde debemos de introducir los datos principales de la web. Introduciremos el nombre de la web, del usuario administrador, etc. 

![wordpress_config5](/assets/images/posts/wordpress_config5.png)

Nos logeamos en la pantalla que aparece y se nos abrirá el panel de administración de WordPress donde podremos instalar plugins, nuevos temas y configurarlo absolutamente todo.

![wordpress_config6](/assets/images/posts/wordpress_config6.png)

Si queremos acceder a la página en sí podemos darle en la esquina superior izquierda donde aparece el nombre de la web y le damos a «Visitar sitio«. Tened en cuenta que si hemos cerrado el navegador o la sesión podemos volver al panel de administración a través de la dirección: «http://IP_RASPBERRY/wp-admin«

![wordpress_panel](/assets/images/posts/wordpress_panel.png)
![wordpress_pagina](/assets/images/posts/wordpress_pagina.png)
