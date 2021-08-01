---
title: "Cómo conectarte a una Raspberry Pi por SSH"
date: 2021-08-01T16:34:30-04:05
categories:
  - blog
tags:
  - raspberry
  - primeros_pasos
  - ssh
---
Si queremos utilizar la Raspberry sin necesidad de conectarnos a un monitor ni tener que conectar un teclado y un ratón, existen diferentes métodos:

Puedes conectarte a una Raspberry Pi por SSH con lo que tendremos una Shell (o interfaz de línea de comandos).
O conectarte mediante VNC con lo que tendremos un escritorio remoto.

En este artículo explicaremos como conectarnos a una Raspberry Pi mediante el protocolo SSH.

## Cómo conectarnos por SSH a una Raspberry Pi

Un pequeño truco para conectarte a una Raspberry Pi por SSH cuando instalamos el sistema operativo es el de habilitar directamente la conexión por SSH de forma que no será necesario conectarse a un monitor ni si quiera la primera vez que iniciamos la Raspberry.

Para ello, una vez hemos escrito el sistema operativo en la tarjeta SD la volvemos a conectar al PC y veremos que hay una partición que se llama «boot»: 

![boot partition](/assets/images/posts/boot.png)

Si nos metemos veremos varios archivos, pero lo que nos interesa es crear uno que se llame «ssh». De este modo cuando la Raspberry inicie directamente va a habilitar el servicio SSH. Con ello podremos conectarnos siempre y cuando estemos en la misma red. Debemos de tener en cuenta que es importante que el archivo no tenga ninguna extensión (ni si quiera txt).

Desde Windows podemos crear el documento haciendo click derecho > Nuevo > Documento de texto y lo renombramos a «ssh» quitando la extensión «.txt» (seguramente tendremos que habilitar el ver las extensiones de los archivos para poder borrar la extensión cuando cambiemos el nombre).


![windows extensiones archivos](/assets/images/posts/windows_extensiones.png)

![archivo ssh creado](/assets/images/posts/archivo_ssh.png)

Si vamos a utilizar la Raspberry Pi conectada a nuestra red por cable ya podemos introducir la microSD y conectarla a la alimentación.

Sin embargo, si la queremos utilizar por Wifi (algo menos fiable debido a los posible fallos de conexión/interferencias de la conexión inalámbrica) tendremos que crear otro fichero llamado «wp_supplicant.conf» en la misma ruta donde creamos el fichero «ssh».

En este nuevo archivo tendremos que escribir lo siguiente: 

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=ES

network={
 ssid="<Nombre de punto de acceso Wifi>"
 psk="<Contraseña del punto de acceso>"
}
```
Hay que tener en cuenta que, en la línea «country=ES», las letras ES vienen de España. Si queremos utilizar el código de otro país podemos consultar cuáles son sus siglas en el siguiente repositorio <a href="https://github.com/recalbox/recalbox-os/wiki/Wifi-country-code-(EN)">«Wifi country code»</a>. También es importante que tanto el nombre de nuestra red Wifi como la contraseña se encuentren entre comillas.

Finalmente, una vez tenemos todos los archivos de configuración preparados metemos la microSD en la Raspberry Pi y la encendemos. Tras esto, nos queda conocer la dirección IP de nuestra Raspberry Pi para poder establecer la conexión por SSH. Para conocer la IP podemos meternos en la configuración de nuestro router y ver los dispositivos conectados (personalmente uso esta para no tener que instalar más programas en mi PC) o podemos descargarnos alguna utilidad que escanee nuestra red como podría ser NMAP o <a href="https://www.advanced-ip-scanner.com/es/">Advanced IP Scanner</a> (utilizaremos este último por facilidad de uso en Windows). 

## Cómo saber la dirección IP de nuestra Raspberry Pi con Advanced IP Scanner

Una vez nos hemos descargado e instalado el Advanced IP Scanner (para instalarlo es solo hacer click y darle a siguiente/acepto cuando nos lo pida) se nos abrirá la siguiente ventana: 

![ip scanner](/assets/images/posts/advancedIPscanner.png)

Pulsamos sobre el botón de «Explorar» y esperamos a que nos salgan todos los dispositivos de nuestra red. 

![ip scanner](/assets/images/posts/dispositivos_escaneados.png)

En la imagen se han ocultado algunas partes que dan información sobre otros dispositivos de mi red, pero se puede ver claramente que en mi caso tengo 4 Raspberry Pi conectadas (1 Raspberry Pi 4 versión 4GB de RAM y 3 Raspberry Pi 3 B+) y sus respectivas direcciones IP. Ya solo tendremos que conectarnos por SSH mediante algún software si estamos en Windows o mediante la terminal si estamos en MAC o Linux. Para conectarnos desde la terminal usaremos el siguiente comando (cambiando «IP_Raspberry_Pi» por la dirección que nos ha aparecido en Advanced IP Scanner): 

```ssh pi@IP_Raspberry_Pi```

## Establecimiento de la conexión SSH desde Windows

Como he comentado anteriormente, si estamos en Windows necesitaremos algún software extra. Uno de los más comunes es el conocido como <a href="https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html">PuTTy</a>, sin embargo, personalmente prefiero <a href="https://termius.com/">Termius</a> ya que estéticamente es más bonito y fácil de gestionar. Nos podemos descargar Termius desde este <a href="https://www.termius.com/windows">enlace</a>. Una vez descargado lo instalamos y se nos abrirá el programa: 

![termius](/assets/images/posts/termius.png)

Con Termius podemos conectarnos de varias formas: 

* Desde la parte superior con el mismo comando que hemos comentado anteriormente para Linux o Mac OS (ssh pi@direcciónIP).
* Guardando la información de nuestro dispositivo para conectarnos haciendo un solo click (como se ve en la captura mi Raspberry Pi 4). Para ello, le damos al botón «NEW HOST» e introducimos la información necesaria: 

![termius](/assets/images/posts/termius_addhost.png)
![termius](/assets/images/posts/termius_addhost_2.png)


El usuario será «pi» y la contraseña «raspberry» o si ya la hemos cambiado pues la que hayamos puesto nosotros. Le damos al botón verde «SAVE» para guardar los cambios y nos aparecerá en la ventana principal un icono con el nombre que hayamos puesto. Le damos y se nos abrirá la Shell (si nos sale algún mensaje le damos a «REPLACE» y listo): 


![shell establecida](/assets/images/posts/termius_shell_establecida.png)

Siguiendo todos estos pasos conseguirás conectarte a una Raspberry Pi por SSH exitosamente. Sin embargo, como he comentado al principio, también es posible conectarte remotamente utilizando VNC.

En el siguiente artículo podrás ver paso a paso cómo conectarte a una Raspberry Pi utilizando VNC para establecer un escritorio remoto y usarla como si estuvieras conectado a un monitor.

