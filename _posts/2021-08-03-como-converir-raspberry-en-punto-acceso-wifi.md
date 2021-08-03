---
title: "Cómo convertir una Raspberry Pi en un punto de acceso Wifi"
date: 2021-08-03T17:33:30-04:05
categories:
  - blog
tags:
  - raspberry
  - wifi
  - raspap
---
En esta ocasión vamos a ver cómo convertir una Raspberry Pi en punto de acceso Wifi al que conectarnos desde cualquiera de nuestros dispositivos. Además, lo configuraremos para que enrute todo ese tráfico a través de una VPN. Todo es proceso se realizará de forma sencilla gracias a <a href="https://github.com/RaspAP/raspap-webgui">RaspAP</a>. Durante el desarrollo de este tutorial se ha utilizado como sistema operativo base Raspbian (podéis ver cómo instalarlo en el siguiente <a href="https://rasp0wn.github.io/blog/como-instalar-un-sistema-operativo-en-raspberry/">enlace</a>).

Para utilizar la función de enrutamiento por VPN necesitaremos tener contratado alguno de estos servicios (en mi caso utilizo NordVPN, pero servirá con cualquiera que nos proporcione un archivo ovpn de conexión). De esta forma podremos navegar de forma más segura estemos donde estemos ya que todo nuestro tráfico estará cifrado.

Es importante destacar que para tener conexión a Internet a través de la Raspberry deberemos de estar conectados a un router que nos de conexión con el exterior. Podremos conectarnos por cable o bien podemos aprovechar la función de «Cliente Wifi» que incorpora RaspAP. Esta última opción permite conectarnos por Wifi a otro punto de acceso desde la Raspberry de forma que podamos navegar por Internet a la vez que estamos conectados al AP de la Raspberry.

## Instalación de RaspAP para convertir nuestra Raspberry Pi en punto de acceso Wifi

La instalación de RaspAP, el programa que nos permitirá convertir nuestra Raspberry Pi en punto de acceso Wifi, es realmente sencilla. Solo hay que ejecutar el siguiente comando:

```
curl -sL https://install.raspap.com | bash
```
Tendremos que ir aceptando los diferentes mensajes que nos salgan (escribiendo una «Y» y dándole a Enter) hasta que la instalación finalice (en la último paso nos pregunta si deseamos reiniciar la Raspberry, aceptamos y esperamos a que se reinicie).

## Conexión con el punto de acceso Wifi creado en la Raspberry
Una vez se haya reiniciado la Raspberry, podremos comprobar con nuestro móvil u ordenador que aparece una red Wifi llamada «raspi-webgui». Para conectarnos a ella tendremos que introducir la contraseña:

```
ChangeMe
```

Tras habernos conectado, accederemos al panel de administración de RaspAP. Para ello nos conectamos a través de la dirección http://10.3.141.1 (la IP por defecto que configura RaspAP para la propia Raspberry). Nos pedirá credenciales para iniciar sesión (usuario:contraseña):

```
admin:secret
```

![raspAP_mainpage.png](/assets/images/posts/raspAP_mainpage.png)

## Configuración

Lo primero que deberemos de hacer por motivos de seguridad será cambiar tanto las credenciales de acceso al panel de administración como la contraseña (y también el nombre si se desea) del punto de acceso Wifi.

Para cambiar las credenciales de acceso al panel tendremos que ir a la opción «Authentication« donde pondremos la contraseña y el usuario que queramos.  Tras cambiarlas se nos saldrá del panel y tendremos que volver a acceder pero con las nuevas credenciales.

![raspAP_auth.png](/assets/images/posts/raspAP_auth.png)

Por otro lado, para cambiar la contraseña y el nombre del punto de acceso tendremos que ir a la opción «Hotspot«. En este apartado podremos elegir no solo el nombre (SSID) y la contraseña de la red Wifi (en «Security» > «PSK») si no también el protocolo de seguridad (se puede dejar por defecto en WPA2), el canal y la interfaz (por si conectamos alguna interfaz Wifi USB extra a la Raspberry) a utilizar.

![raspap_hotspot.png](/assets/images/posts/raspap_hotspot.png)

Como se ha comentado en la introducción, necesitaremos tener conectada la Raspberry por cable a un router o utilizar la función «Cliente Wifi». Esta opción se configura dentro de «Wifi client» donde podemos seleccionar la red Wifi a la que conectarnos para tener acceso a Internet.

![raspap_wificlient.png](/assets/images/posts/raspap_wificlient.png)

Con todo esto ya tendremos un punto de acceso funcional al que conectarnos con nuestros dispositivos (incluso podríamos llegar a utilizar la Raspberry como un extensor Wifi si lo necesitamos en algún momento). Sin embargo, aún se pueden seguir más cosas como veremos a continuación.

## Enrutamiento del tráfico por VPN

Finalmente, si queremos que todo el tráfico de los dispositivos conectados sea enrutado a través de una VPN utilizaremos la opción «OpenVPN» del panel de administración. Para ello tendremos que utilizar un archivo ovpn (en el caso de NordVPN se pueden encontrar en el siguiente enlace).

```
https://nordvpn.com/es/servers/tools/
```

Una vez puestos nuestro usuario y contraseña y seleccionado el archivo ovpn en cuestión, le damos a «Save changes». Cuando se hayan guardado ya podemos iniciar la VPN con el botón «Start OpenVPN». A partir de entonces todo nuestro tráfico se enrutará a través de la VPN. Podemos comprobar que nuestra dirección IP ha cambiado con cualquier página como la siguiente: https://ipinfo.io/

De esta forma podremos llevarnos nuestra Raspberry Pi con nosotros para conectarnos a redes poco seguras como las de los hoteles o las cafeterías y securizar nuestro tráfico.


