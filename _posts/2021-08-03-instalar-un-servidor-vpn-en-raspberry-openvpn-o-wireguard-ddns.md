---
title: "Instalar un servidor VPN en Raspberry (OpenVPN o WireGuard + DDNS)"
date: 2021-08-03T21:33:30-04:05
categories:
  - blog
tags:
  - raspberry
  - vpn
  - ddns
  - wireguard
  - openvpn
---
Hoy vamos a ver cómo instalar un servidor VPN en Raspberry Pi. De esta forma podremos conectarnos a nuestra red local desde cualquier sitio y proteger nuestro tráfico.

Si no sabes lo que es una VPN te recomiendo que le eches un vistazo a este <a href="https://www.xataka.com/basics/que-es-una-conexion-vpn-para-que-sirve-y-que-ventajas-tiene">artículo</a>. Para los que ya conozcan esta tecnología, vamos a utilizar PiVPN para configurarlo de forma muy sencilla (pudiendo elegir entre WireGuard y OpenVPN). Además, utilizaremos un DNS Dinámico (o DDNS) para no tener que recordar nuestra IP y evitar problemas si esta cambia.

## Configuración del DDNS

Lo primero que vamos a hacer es crear un nombre de dominio para poder conectarnos a nuestra Raspberry por VPN sin necesidad de sabernos la IP externa. Además, es común que la IP externa controlada por nuestro proveedor de Internet cambie cada cierto tiempo. Por ello, gracias a los DDNS podremos tener un pequeño script ejecutándose en nuestra Raspberry Pi que actualice los registros DNS. De esta forma siempre tendremos conexión a nuestra VPN sin importar cuando cambie nuestra IP.

Para la creación de nuestro dominio con DDNS vamos a utilizar la página <a href="https://www.duckdns.org/">DuckDNS</a> ya que nos proporciona dominios gratuitos que no tendremos que renovar manualmente como es el caso de <a href="https://www.noip.com/">NoIP</a>. Lo primero será irnos a la página en cuestión:

![duckdns](/assets/images/posts/duckdns.png)

Iniciamos sesión, ponemos un nombre donde dice «sub domain» y le damos al botón de «Add Domain«.

![duckdns3](/assets/images/posts/duckdns3.png)

A continuación, vamos la pestaña superior donde pone «install» y seleccionamos «pi» en «Operating Systems«. Solo tenemos que seguir los pasos que nos describen y ya tendremos configurado un script que cada 5 minutos actualiza nuestra IP externa.

![duckns4](/assets/images/posts/duckns4.png)

Ya podemos proceder a configurar nuestro servidor VPN en la Raspberry Pi.

## Instalar servidor VPN en Raspberry Pi

La instalación del servidor VPN en nuestra Raspberry Pi es bastante sencilla ya que solo tendremos que ejecutar el siguiente comando:

```
curl -L https://install.pivpn.io | bash
```

Durante el proceso de instalación se nos preguntarán una serie de cuestiones que tenemos que configurar como se describe en las imágenes a continuación (el proceso ha sido realizado par WireGuard pero es prácticamente igual para OpenVPN). Para movernos por las opciones podemos usar las flechas del teclado o la tecla TAB y para aceptar le damos al espacio:


![install1](/assets/images/posts/install1.png)
![install2](/assets/images/posts/install2.png)
![install3](/assets/images/posts/install3.png)
![install4](/assets/images/posts/install4.png)
![install5](/assets/images/posts/install5.png)
![install6](/assets/images/posts/install6.png)

Finalmente tenemos que añadir el usuario con el que nos conectaremos con el siguiente comando:

```
pivpn add
```

Nos pedirá que introduzcamos un nombre de usuario (si es con OpenVPN, también una contraseña). Para WireGuard nos creará el archivo «nombre_usuario.conf» en /home/pi/configs (también como ~/configs/) mientras que para OpenVPN, se creará el archivo «nombre_usuario.ovpn» en /home/pi/openvpn. Tendremos que pasar estos archivos al dispositivo con el que queramos conectarnos (aunque todavía queda abrir los puertos del router para poder establecer la conexión). En WireGuard también podemos escanear el código QR que nos sale tras ejecutar el comando:

```
pivpn -qr
```

Como dato adicional si tenéis algún problema (por ejemplo que os conecta a la VPN pero no tenéis internet) podéis ejecutar el comando de debuggin para que haga un checkeo y aplique la configración restante con: 

```
pivpn -d
```

## Apertura de puertos en el router y conexión

Una vez ya hayamos realizado toda la configuración pertinente tendremos que abrir los puertos en nuestro router de forma que tengamos conexión directa al puerto que hemos configurado en el servidor VPN. Cada router es diferente así que aconsejo que si no sabéis cómo hacerlo vosotros mismos busquéis en Internet cómo es en vuestro modelo.

Finalmente, para conectarnos a nuestra VPN y comprobar que todo se ha realizado correctamente, tendréis que descargar un cliente para WireGuard (en la siguiente <a href="https://www.wireguard.com/install/">página</a> podéis encontrar los clientes tanto para iOS y Android, como para Windows, MacOS o Linux) o para <a href="https://openvpn.net/client-connect-vpn-for-windows/">OpenVPN</a>.

Si estamos con WireGuard, cuando tengamos el cliente importamos el fichero «.conf« que hemos comentado en el apartado anterior o escaneamos el código QR en cuestión. Si por el contrario hemos utilizado OpenVPN utilizaremos los archivos con extensión «.ovpn«. Una vez nos hayamos conectado a nuestra VPN podemos comprobar que nuestra IP ha cambiado en alguna página como ipinfo.io.