---
title: "Cómo instalar LibreELEC en Raspberry (centro multimedia)"
date: 2021-08-03T22:33:30-04:05
categories:
  - blog
tags:
  - raspberry
  - kodi
  - libreelec
---

En esta ocasión vamos a ver cómo instalar LibreELEC en Raspberry Pi. Esta distribución nos permite convertir nuestra Raspberry en un centro multimedia para poder ver cualquier tipo de contenido. Veremos además como instalar Plex y configurar un cliente para ver la TV por Internet (útil si en alguna habitación no tenemos toma de antena).

Finalmente, enseñaré a instalar Kodi (el programa en el que se basa LibreELEC para convertir cualquier dispositivo en un centro multimedia) el cual está disponible para prácticamente cualquier sistema operativo (incluido Android).

##Instalar LibreELEC en Raspberry

Para instalar LibreELEC en Raspberry podemos descargar la imagen directamente desde <a href="https://libreelec.tv/downloads_new/">este enlace</a> pulsando sobre la versión de nuestra Raspberry. Una vez tengamos la imagen descargada vamos a utilizar Etcher y el mismo procedimiento que en el artículo sobre <a href="https://rasp0wn.github.io/blog/como-instalar-un-sistema-operativo-en-raspberry/">cómo instalar un sistema operativo</a>.

Tras tener la imagen instalada en la microSD la insertamos en la Raspberry y la encendemos teniendo en cuenta que la Raspberry deberá de estar conectada a un monitor para poder hacer toda la configuración. Cuando iniciemos la Raspberry veremos que aparece el logo de LibreELEC. Tendremos que esperar un poco a que termine de redimensionar las particiones correspondientes a nuestra microSD y cuando finalice aparecerá el logo de Kodi y la pantalla principal.

En esta pantalla elegimos el idioma del sistema y le damos a «Siguiente». Tras esto nos pedirá que le pongamos un nombre al equipo (podemos elegir el que queramos, por ejemplo «raspimedia«).

A continuación, nos pedirá que elijamos la red Wifi a la que conectarnos (si usamos conexión por Ethernet no hará falta seleccionar ninguna).

Finalmente, nos pregunta si queremos activar SSH (para conectarnos en remoto) y Samba (el servicio para compartir carpetas de Windows). Personalmente solo activaré el SSH ya que si necesito pasarme algún archivo puedo usar Filezilla de forma cómoda.

Al darle a «Siguiente» nos pedirá una contraseña para securizar la conexión por SSH y ya podremos acceder al menú principal.

screenshot000.png

## Configurando la interfaz de LibreELEC en Raspberry

Cuando se inicie la interfaz principal veremos que aunque lo hayamos puesto en español los menús salen en inglés. Por ello, lo primero será darle al icono de ajustes (la rueda dentada en la parte superior), luego a «Interface» > «Regional» y en «Language» seleccionamos «español«. Esperamos que se haga efectiva la configuración y veremos como ahora está todo el menú en español. Por último, nos queda establecer la distribución del teclado como «Español», para ello vamos al mismo menú anterior «Ajustes» > «Interfaz» > «Regional» > «Distribuciones del teclado» seleccionando «Español» y deseleccionando «English».

Ahora procederemos a instalar los programas, o como se conocen en el mundillo de Kodi/LibreELEC/OSMC, los Add-ons.

## Instalar Plex en LibreELEC

Para instalar Plex tenemos que irnos de nuevo a los ajustes (botón de rueda dentada) > «Addons» > «Instalar desde repositorio» > «Todos los repositorios» > «Add-ons de vídeo» y buscamos «Plex» (está ordenado alfabéticamente así que habrá que bajar bastante). Se nos abrirá una ventana con el logo de Plex y le damos a «Instalar» (se abre otra ventana y le damos a «Ok»). Esperamos que nos salgan varias notificaciones hasta que ponga que Plex ha sido instalado y ya podemos irnos al menú principal > Add-ons y ahí estará.

La ventaja de Plex en Kodi es que al ser Plex orientado a SmartTV no necesitamos hacer ningún pago (como pasa en las aplicaciones móviles) para visualizar sin límite nuestro contenido.

## Ver la TV por Internet

Otra de las opciones de LibreELEC es poder ver la televisión por Internet. Para poder ver la TV vamos a Ajustes > «Add-ons» (si después de haber instalado Plex se nos ha quedado en el menú de Add-ons de vídeo tenemos que subir del todo hasta ver los «..» para volver al menú anterior). > «Clientes PVR» y seleccionamos el que se llama «PVR IPTV Simple Client». Le damos a «Instalar» y seleccionamos la última versión que haya disponible en la ventana que se abre.

A continuación, tenemos que configurar la lista de canales por lo que volvemos a «Ajustes» > «Add-ons» > «Clientes PVR» > «PVR IPTV Simple Client» y ahora le damos a «Configurar». En la pestaña «General«, le damos a «URL a la lista M3U« y ponemos la siguiente URL:

```
https://www.tdtchannels.com/lists/tv.m3u8
```

Finalmente reiniciamos la Raspberry y cuando se inicie en la pestaña TV del menú principal tendremos todos los canales de TV. Para utilizar la TV en la Rasberry y poder movernos cómodamente por los menús yo recomiendo un mando inalámbrico de <a href="https://www.amazon.es/gp/product/B00WDSCTR4/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1">este estilo</a> (se puede utilizar como «Air Mouse» o con los botones).


