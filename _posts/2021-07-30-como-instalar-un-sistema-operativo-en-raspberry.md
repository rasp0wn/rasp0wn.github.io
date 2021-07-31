---
title: "Cómo Cómo instalar un sistema operativo en Raspberry Pi"
date: 2021-07-30T16:34:30-04:05
categories:
  - blog
tags:
  - raspberry
  - primeros_pasos
  - sistema_operativo
---
Una vez sabemos <a href="https://rasp0wn.github.io/blog/como-iniciarse-en-raspberry-pi/"> cómo iniciarnos en Rapsberry Pi</a> <b>consiguiendo todos los elementos hardware</b>, llega la hora de saber <b>cómo instalar un sistema operativo en Raspberry Pi</b>, es decir, el software.

Como con cualquier PC, <b>la Raspberry necesitará un sistema operativo para funcionar</b>. Normalmente los sistemas operativos para la Raspberry son un archivo con extensión «.img». Estos archivos tenemos que escribirlos a la microSD con un programa (<i>por lo que necesitaremos un ordenador donde conectar la microSD mediante un adaptador para este proceso</i>). 

## ¿Qué sistema operativo instalar en Raspberry Pi?

A la hora de elegir nuestro sistema operativo tenemos que tener en cuenta que existen numerosas distribuciones Linux disponibles. Por tanto trataremos de elegir la que mejor se adapte a nuestras necesidades (dependiendo si queremos montar un centro multimedia, una nube personal, un servidor web, un sistema de juegos retro, etc.), pero sin duda, la más conocida es Raspbian aunque recientemente cambió su nombre a <a href="https://www.raspberrypi.org/software/operating-systems/">Raspberry Pi OS</a>. 

La principal ventaja de Raspbian es su soporte tanto por parte de la propia compañía como de los usuarios que continuamente suben tutoriales y responden dudas ante cualquier problema. Además, es un sistema operativo optimizado para consumir pocos recursos.

Por ello si te estás iniciando con una Raspberry Pi este es el sistema operativo más recomendado y el que enseñaré a instalar en este tutorial (podéis dejar en los comentarios si queréis que suba un tutorial de cómo instalar un sistema operativo en Raspberry Pi en concreto). 

## Cómo escribir un sistema operativo en una microSD

Para instalar un sistema operativo en Raspberry Pi, lo primero será descargar el sistema operativo que hayamos elegido.

En el caso de Raspbian es bastante sencillo. Solo tendremos que presionar sobre el botón «Download zip» de la versión que queramos (recomiendo la versión «with desktop and recommended software» ya que es la más completa aunque ocupe más espacio).

![Raspbian](/assets/images/posts/raspbian.png)

Mientras se descarga el archivo, aprovecharemos para descargar el programa que vamos a utilizar para grabarlo a la microSD.

Este programa se llama <a href="https://www.balena.io/etcher/">Etcher</a> y está disponible tanto para Windows como para Mac OS y Linux. Para descargarlo solo tenemos que elegir la versión correspondiente a nuestro PC: 

![Etcher](/assets/images/posts/etcher.png)

Una vez hemos descargado el programa, le damos click para instalarlo (solo tenemos que darle al botón de «Acepto») y se nos abrirá el programa:

![Etcher abierto](/assets/images/posts/etcher_abierto.png)

A continuación le daremos donde pone «Flash from file» y elegiremos el archivo de Raspbian que hemos descargado para escribirlo como sistema operativo a la microSD, la cual deberemos de conectarla al PC. Donde pone «Select target» debemos elegir nuestra tarjeta microSD (aseguraos de que es la microSD ya que si elegís un disco duro se formateará y perderéis la información): 

![Etcher SD](/assets/images/posts/etcher_SD.png)

En mi caso estoy utilizando una tarjeta microSD de 32GB. La seleccionamos y le damos al botón de «Flash!». El proceso tardará unos minutos (según la velocidad de la tarjeta microSD) en el cual flasheará el sistema operativo. Primero comprobará que se haya realizado correctamente y luego nos dará un mensaje de éxito o de error si ha habido algún problema:


![Etcher in process](/assets/images/posts/etcher_inprocess.png)

![Etcher exito](/assets/images/posts/etcher_exito.png)

Una vez completado el proceso, podemos poner la microSD en la Raspberry Pi y encenderla conectándola a un monitor, un teclado y un ratón para controlarla.

La primera vez que iniciemos la Raspberry nos pedirá un usuario y una contraseña. Por defecto en Raspbian el usuario es «pi» y la contraseña «raspberry«.

Si por el contrario os interesa controlar la Raspberry Pi de forma remota, sin necesidad de tener que conectarla a un monitor ni a un teclado ratón, visita nuestro artículo sobre cómo controlar la Raspberry Pi de forma remota.


## Cómo cambiar la contraseña en la Raspberry Pi

Como he comentado anteriormente, la Raspberry Pi tiene por defecto el usuario «pi» y la contraseña «raspberry». Por motivos de seguridad es de vital importancia cambiar la contraseña, y si queremos, también podemos crear otros usuarios.

Para cambiar la contraseña tendremos que abrir una terminal (se puede abrir con la combinación de teclas Control + Alt + T o haciendo click en el icono que aparece a continuación).

![Raspbian menu](/assets/images/posts/raspbian_menu.png)

Tras esto escribiremos el siguiente comando: 

```passwd```

Una vez introducido el comando nos pedirá que introduzcamos la contraseña actual (sería «raspberry» como hemos comentado anteriormente). Tras esto, podemos introducir nuestra nueva contraseña y se hará efectivo el cambio. La próxima vez que iniciemos sesión con el usuario «pi» tendremos que poner la nueva contraseña. 