---
title: "Cómo instalar Kali Linux en Raspberry"
date: 2021-08-03T18:33:30-04:05
categories:
  - blog
tags:
  - raspberry
  - hacking
  - kali
---
Uno de los sistemas operativos más conocidos en el mundo del hacking es Kali Linux. Este distribución está basada en Debian e incluye por defecto numerosas herramientas relacionadas con el mundo de la ciberseguridad. A lo largo de este artículo vamos a ver cómo instalar Kali Linux en nuestra Raspberry.

Para este sistema operativo en nuestra Raspberry Pi seguiremos los mismos pasos que para instalar cualquier otro sistema operativo. Podéis ver los pasos para instalar un sistema operativo en Raspberry Pi en el siguiente <a href="https://rasp0wn.github.io/blog/como-instalar-un-sistema-operativo-en-raspberry/">enlace</a>. 

## Descargade la imagen para instalar Kali Linux en Raspberry

Comenzamos descargando la imagen de Kali correspondiente a la versión de nuestra Raspberry Pi. Para este tutorial voy a instalarlo en una Raspberry Pi 3 B+ así que elegiré la siguiente versión:

![Kalipi_descarga](/assets/images/posts/Kalipi_descarga.png)

Una vez descargada la imagen del sistema, lo instalamos con <a href="https://www.balena.io/etcher/">Etcher</a>. Esta vez no es necesario que creemos el archivo «ssh» en la raíz de la SD para conectarnos de forma remota a la Raspberry ya que ya existe (todos estos pasos se explican en el primer tutorial sobre cómo instalar un sistema operativo, incluyendo algunos consejos y trucos que harán el proceso más cómodo). Hay que tener en cuenta que la primera vez tardará un cierto tiempo en iniciar la Raspberry Pi por lo que tendremos que ser pacientes.

## Conexión
Una vez haya iniciado completamente podemos conectarnos por SSH con las credenciales kali:kali (usuario:contraseña).

```
ssh kali@IP_Rasberry
```

Tras conectarnos, es recomendado actualizar los repositorios y el software que viene ya pre-instalado (esto va a tardar bastante) después de haber cambiado la contraseña. Para actualizar los repositorios y los programas instalados ejecutamos el siguiente comando: 

```
sudo apt-get upgrade -y && sudo apt-get update -y
```

Por otro lado, si queremos conectarnos por VNC a la Raspberry con Kali es un poco diferente al tutorial de <a href="https://rasp0wn.github.io/blog/como-conectarse-a-una-raspberry-pi-por-vnc/">cómo conectarse desde Raspbian</a>. Para conectarnos la primera vez y comprobar que todo funcione deberemos de introducir el siguiente comando desde nuestra conexión por SSH:

```
tighvncserver -geometry 1920x1080
```

Con esto ya podremos conectarnos remotamente y comprobar que todo funciona adecuadamente. Sin embargo, si queremos que VNC se ejecute con al iniciar la Raspberry tendremos que seguir los siguientes pasos.

## Configurar VNC en el inicio del sistema

Lo primero será crear un archivo llamado vncboot en la ruta /etc/init.d/ con el comando:

```
sudo touch /etc/init.d/vncboot
```

Y copiamos dentro lo siguiente:

```
#!/bin/sh
### BEGIN INIT INFO
# Provides: vncboot
# Required-Start: $remote_fs $syslog
# Required-Stop: $remote_fs $syslog
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: Start VNC Server at boot time
# Description: Start VNC Server at boot time.
### END INIT INFO

USER=root
HOME=/root

export USER HOME

case "$1" in
start)
echo "Starting VNC Server"
#Insert your favoured settings for a VNC session
/usr/bin/tightvncserver :1 -geometry 1920x1080 -depth 16 -pixelformat rgb565
;;

stop)
echo "Stopping VNC Server"
/usr/bin/tightvncserver -kill :1
;;

*)
echo "Usage: /etc/init.d/vncboot {start|stop}"
exit 1
;;
esac

exit 0
```

Guardamos con Control + O y salimos con Control + X. Una vez hecho esto le damos permisos al archivo:

```
sudo chmod 755 /etc/init.d/vncboot
```

Y ejecutamos el siguiente comando:

```
sudo update-rc.d vncboot defaults
```

Listo, ahora podemos reiniciar la Raspberry con el comando «sudo reboot» y cuando inicie de nuevo levantará automáticamente el servicio VNC sin necesidad de conectarnos previamente por SSH para lanzarlo. Para conectarnos desde el programa VNC viewer podemos hacer como expliqué en Raspbian (poniendo la siguente dirección IP_Kali_Pi:1)

![kali_vnc2](/assets/images/posts/kali_vnc2.png)