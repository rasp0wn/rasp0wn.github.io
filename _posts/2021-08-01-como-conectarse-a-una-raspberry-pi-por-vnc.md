---
title: "Cómo conectarte a una Raspberry Pi utilizando VNC"
date: 2021-08-01T16:33:30-04:05
categories:
  - blog
tags:
  - raspberry
  - primeros_pasos
  - vnc
---
Ya hemos visto cómo podemos conectarnos a la Raspberry Pi por SSH. Sin embargo, a través del protocolo SSH solo podemos ejecutar comandos sin interfaz gráfica.

Si lo que queremos es conectarnos teniendo un escritorio como si estuviéramos delante de un monitor, lo que necesitamos es un escritorio remoto. Para este caso vamos a ver como conectarte a una Raspberry Pi utilizando VNC, que ya viene preinstalado en Raspbian.

## Preparando la Raspberry para conectarte por VNC

El primer paso será conectarnos por SSH a nuestra Raspberry Pi. Una vez hemos iniciado sesión ejecutamos el siguiente comando: 

```vncserver```

Puede que nos pida que establezcamos una contraseña para la conexión, la ponemos y cuando termine de ejecutarse el programa tendremos la siguiente información:

![vncserver](/assets/images/posts/vncserver_1.png)

Como vemos, aparece la dirección IP de nuestra Raspberry con un «:1». Eso significa que se ha creado un escritorio remoto al que podemos conectarnos. Si ejecutásemos otra vez el comando anterior veríamos que pondría «:2», es decir, se habría creado un segundo escritorio remoto. 

## Conectándonos al escritorio remoto

Para conectarnos al escritorio remoto existen muchos programas diferentes. Personalmente utilizo <a href="https://www.realvnc.com/es/connect/download/viewer/">Real VNC Viewer</a> ya que está disponible prácticamente para cualquier plataforma (Windows, Linux, Mac OS, Android, etc) y si nos hacemos una cuenta sincroniza las sesiones guardadas de forma que podemos conectarnos desde nuestro PC o nuestro móvil sin tener que configurar las cosas varias veces. Una vez lo hemos descargado e instalado se nos abrirá el programa: 

vncviewer.png

Para conectarnos a la Raspberry Pi tendremos que darle en «Archivo» > «Nueva conexión» o click derecho > «Nueva conexión» y se abrirá la siguiente ventana: 

![vnc viewer](/assets/images/posts/vncviewer_1.png)

En el campo «VNC Server» pondremos la dirección IP seguida del «:1» del escritorio remoto, es decir, quedaría IP_RASPBERRY:1 (en mi caso «192.168.31.194:1») y en «Nombre» pondremos un alias descriptivo, por ejemplo «Mi Raspberry 3». Le damos a aceptar y se nos habrá creado un icono como este: 

![vnc viewer](/assets/images/posts/vncviewer_2.png)

Le damos doble click y comenzará el intento de conexión. Nos aparecerá un mensaje como el siguiente: 

![vnc viewer](/assets/images/posts/vnviewer_3.png)

Le damos a «Continuar» y nos pedirá el usuario y la contraseña para iniciar sesión y se nos iniciará el escritorio remoto: 

![vnc viewer](/assets/images/posts/vncviewer_4.png)
![vnc viewer](/assets/images/posts/vncviewer_5.png)

Si queremos podemos pasar por el proceso inicial de configuración y actualización siguiendo los pasos que se nos indican (entre los que está cambiar el idioma si queremos ponerlo en español). Con esto ya puedes conectarte a una Raspberry Pi utilizando VNC sin necesidad de tener conectado un monitor, teclado o ratón y desde cualquier dispositivo. 

## Ejecutar VNC en el inicio y cambio de resolución

Tras seguir los pasos anteriores podemos conectarnos a la Raspberry Pi de forma remota sin problema. Sin embargo, si la Raspberry se reinicia o la apagamos y la volvemos a encender de forma manual tendremos que conectarnos por SSH para volver a ejecutar el comando «vncserver«.

Para evitar esto podemos hacer que cuando la Raspberry se encienda automáticamente inicie el servidor VNC, con lo que podremos conectarnos directamente sin pasar por el paso intermedio del SSH. Para ello vamos a utilizar el programa conocido como Crontab, el cual permite ejecutar comandos de forma periódica (por ejemplo cada 5 minutos ejecuta un programa determinado) o bien ejecutar comandos en el inicio del sistema (esta es la parte que nos interesa). 

Comenzamos escribiendo el siguiente comando en una terminal: 

```crontab -e```

La primera vez que lo ejecutemos nos aparecerá un diálogo que nos pregunta con qué editor queremos abrir el fichero de configuración. Recomiendo utilizar «nano» si no estamos habituados a otros:


![crontab](/assets/images/posts/crontab_1.png)

Tenemos que pulsar la tecla 1 en el caso de la captura y Enter. Tras esto se nos abrirá un archivo de configuración como el siguiente: 

![crontab](/assets/images/posts/crontab-2.png)

Tenemos que bajar hasta el final del todo con las flechas y escribir lo siguiente: 

``` @reboot vncserver```


O si queremos que el escritorio remoto de VNC tenga una resolución determinada, por ejemplo, 1920×1080 (Full HD) podemos poner lo siguiente:

```@reboot vncserver -randr 1920x1080```

Para guardar los cambios utilizamos la combinación de teclas control + O (y pulsamos Enter) y para salir control + X. Podemos comprobar que funciona si reiniciamos la Raspberry Pi con el siguiente comando: 

```sudo reboot````

Cuando hayan pasado unos segundos y la Raspberry se haya reiniciado correctamente, volvemos al VNC Viewer y nos conectamos. Ahora la pantalla será mucho más grande ya que hemos cambiado la resolución.

## Arreglar la terminal tras haber configurado VNC

La única pega que nos podemos encontrar cuando ejecutamos VNC en el inicio por Crontab, es que cuando abrimos una terminal en vez de abrirse con bash (cuando aparece en la terminal el texto verde «pi@raspberrypi:~ $») se abre con sh (solo aparece «$» y es menos cómoda de utilizar). Por ello, para cambiar de sh a bash podemos simplemente ejecutar el comando: 

```/bin/bash````

Para arreglarlo y que cada vez que se reinicie abramos una terminal con bash en vez de con sh tendremos que ejecutar un pequeño script en el inicio en vez de VNC directamente. Comenzaremos creando una carpeta que se llame «scripts» en la ruta «/home/pi«:

![fodler scripts](/assets/images/posts/folder_scripts.png)

Dentro de esa carpeta crearemos un archivo llamado «scriptVNC.sh« y copiaremos el siguiente contenido dentro de él.

```
#!/bin/bash
export SHELL=/bin/bash
vncserver -randr 1920x1080
```

Y le damos permisos de ejecución escribiendo el siguiente comando en una terminal abierta en el directorio donde tenemos el script:

```chmod +x scriptVNC.sh```

Tras esto, tenemos que volver a abrir el fichero de configuración de crontab con:

```crontab -e````

Borramos la línea que habíamos escrito anteriormente para ejecutar VNC en el inicio y escribimos lo siguiente:

```@reboot /home/pi/scripts/scriptVNC.sh````

Como antes, pulsamos control + O (y Enter) para guardar y para salir control + X. Reiniciamos la Raspberry Pi y ahora podremos conectarnos por VNC como antes pero cuando abramos una terminal tendremos bash en lugar de sh.


