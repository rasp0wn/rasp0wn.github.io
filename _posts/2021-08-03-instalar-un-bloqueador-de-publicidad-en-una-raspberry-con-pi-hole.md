---
title: "Instalar un bloqueador de publicidad en una Raspberry con Pi-hole"
date: 2021-08-03T19:33:30-04:05
categories:
  - blog
tags:
  - raspberry
  - pihole
---
En el siguiente artículo vamos a ver cómo podemos instalar un bloqueador de publicidad en una Raspberry para toda la red de nuestra casa. Además, podremos configurar nombres de dominio para las IPs que deseemos dentro de nuestra red local y que sea más fácil gestionar nuestra propia LAN.

Para ello, simplemente vamos a necesitar una Raspberry Pi con Raspbian instalado (si no sabéis cómo instalar un sistema operativo en vuestra Raspberry, podéis echarle un ojo al <a href="https://rasp0wn.github.io/blog/como-instalar-un-sistema-operativo-en-raspberry/">siguiente artículo</a>).

## Configurar e instalar el bloqueador de publicidad en Raspberry

Para instalar un bloqueador de publicidad en Raspberry vamos a utilizar le programa conocido como <a href="https://pi-hole.net/">Pi-hole</a>. Simplemente tendremos que ejecutar el siguiente comando en una terminal:

```
curl -sSL https://install.pi-hole.net | sudo bash
```

Cuando lo ejecutemos se iniciará el proceso de instalación en el que iremos aceptando (o seleccionando a nuestro gusto) las opciones que nos van apareciendo en pantalla. Para movernos entre las opciones podemos utilizar las flechas del teclado o si queremos ir al botón de aceptar directamente podemos darle a la tecla TAB. Para aceptar la opción sobre la que estamos usaremos la tecla espacio. Cuando hayamos aceptado todas las opciones nos dará la contraseña de acceso a la interfaz web.

Para entrar a la interfaz web de administración tendremos que poner en un navegador la dirección http://IP_RASPBERRY/admin e iniciar con la contraseña que se nos ha proporcionado dándole al apartado «Login«.

![portal_web](/assets/images/posts/portal_web.png)

Si queremos cambiar la contraseña (bastante recomendado), tendremos que introducir el siguiente comando en la terminal:

```
pihole -a -p
```

Cuando hayamos completado la instalación nos quedará configurar los DNS en el router para que utilice a nuestra Raspberry Pi en lugar de los DNS por defecto (como por ejemplo los de Google) de forma que sea capaz de bloquear la publicidad de todos los dispositivos de dicha red. Adicionalmente, podemos configurar los DNS en cada uno de nuestros dispositivos para asegurarnos que se utilizan los de Pi-hole (tendremos que poner como DNS la IP de la Raspberry).

En el caso de querer desinstalar Pi-hole habría que introducir el siguiente comando:

```
pihole uninstall
```

## Configuración de nombres de dominio para nuestros dispositivos

Una de las opciones más interesantes que incluye Pi-hole, además de las de bloquear publicidad, es la de poder asignar nombres de dominio a nuestros dispositivos. De esta forma en lugar de tener que recordar las direcciones IPs de cada equipo de la red podemos recordar los nombres, por ejemplo, en lugar de poner «192.168.0.1» para acceder al router podríamos poner «router.local».

Para ello, lo primero sería configurar las direcciones IPs de los equipos como estáticas en lugar de dinámicas para que no cambien con el tiempo (normalmente los routers tienen esta opción para poder asignar la dirección que deseemos a nuestros equipos o bien podemos forzar que el equipo pida una IP en concreto).

![portal_web2](/assets/images/posts/portal_web2.png)

Una vez hecho esto deberemos de iniciar sesión en el panel de administración de Pi-hole e ir al apartado «Local DNS Records«. En este apartado pondremos en el apartado «Domain» el dominio con el que queramos acceder al dispositivo (por ejemplo raspberry.local o raspberry.casa) y en «IP Address» la dirección IP del dispositivo en cuestión (en el caso de mi Raspberry 192.168.31.127).

Con esto ya podremos acceder a nuestra Raspberry poniendo en el navegador raspberry.local en lugar de la dirección IP.

