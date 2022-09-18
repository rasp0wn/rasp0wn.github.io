---
title: "Flipper Zero - primeros pasos"
date: 2021-08-03T23:33:30-04:05
categories:
  - blog
  - flipperzero
tags:
  - hardware
  - hacking
  - flipper
---
<h1>Introducción</h1>
Para quien no lo sepa, el Flipper Zero es un dispositivo orientado a hardware hacking que consiste en multitud de herramientas recopiladas en un único sitio. Entre estas herramientas se encuentran: lector NFC/RFID, sensor infrarrojos (IR), tiene puertos GPIO (comunicación UART), radio sub-1GHz, etc...

![flipper000](/assets/images/posts/flipper000.png)

Tras haber estado unos días cacharreando con el <a href="https://flipperzero.one/">Flipper Zero</a> y viendo las posibilidades que tiene he decidido hacer una pequeña recopilación de enlaces a repositorios y herramientas útiles. 

# 1. Firmwares: Original vs Custom
El primer paso que hay que realizar con el Flipper Zero es actualizarlo a la última versión del firmware oficial con el que viene. Esto se hace de forma muy sencilla a través de la herramienta que la propia empresa porporciona: https://flipperzero.one/update

![flipper001](/assets/images/posts/flipper001.png)

Lo recomendado desde la empresa es quedarse con el firmware original ya que los customs firmwares pueden habilitar opciones ilegales como desbloquear el rango de frecuencias en el que el  Flipper puede emitir (por defecto el dispositivo está limitado al lugar donde se ha enviado tras su compra) y no tienen soporte oficial si algo sale mal. 

Dicho esto, los custom firmwares parten del original y son modificados por la comunidad para añadir utilidades (algunas de ellas llegarán de forma oficial en el futuro, otras no). Los dos custom firmware más famosos son: 

- **Unleashed Firmware**: https://github.com/Eng1n33r/flipperzero-firmware

- **RogueMaster Firmware**: https://github.com/RogueMaster/flipperzero-firmware-wPlugins

Ambos firmware son bastante parecidos e incluyen multitud de nuevas funcionalidades que no vienen con el firmware original. Personalmente, prefiero el Unleashed por su facilidad de instalación y porque no viene con tantos gráficos cambiados (no salen Pokémon ni cosas de Dragon Ball por las pantallas).

## Instalación
Para la instalación de cada uno de los firmwares: 

- **Unleashed Firmware**: Se hace a través de una página web clickando en un simple botón. De todas formas siempre es recmoendable seguir las instrucciones que recomienden para la última versión: https://github.com/Eng1n33r/flipperzero-firmware/blob/dev/documentation/HowToInstall.md

- **RogueMaster Firmware**: Dejo un enlace a un blog donde explican cómo instalarlo, aunque igualmente en el repositorio detallan los pasos a seguir: https://interestingsoup.com/n00b-guide-flashing-flipper-zero-to-rougemaster/


# 2. Bases de datos de dispositivos para infrarrojos: IRDBs

Con los custom firmwares se dispone de mandos IR "universales" para TVs, ACs, ventiladores, proyectores, etc que básicamente hacen una especie de "fuerza bruta" probando muchos códigos. Sin embargo, si buscas algo más concreto o estás con el firmware oficial puedes descargarte bases de datos de multitud de dispositivos con sus códigos infrarrojos. De esta forma puedes apagar o encender una tele/ventilador/proyector de una marca en concreta.

Con buscar en Google "flipper IRDB" encontraréis un montón de enlaces a repositorios de github (casi todos son forks unos de otros). Los que he visto que se mantienen más actualizados y admiten aportacioens de la comunidad son: 

- https://github.com/UberGuidoZ/Flipper-IRDB
- https://github.com/Lucaslhm/Flipper-IRDB

# 3. Payloads para el BadUSB 

El Flipper Zero es compatible con scripts hechos para rubber ducky y similares por lo que podéis encontar un montón por internet. Personalmente he utilizado solo scripts de broma pero podéis descargar bastantes desde aquí (los que he probado han funcionado a la primera sin necesidad de cambiar nada): 

- https://github.com/FalsePhilosopher/badusb

Tip: Si no sabes lo que es un BadUSB, básicamente puedes conectar el Flipper Zero por USB a cualquier dispositivo y que se comporte como un teclado introduciendo comandos de forma automática.

# 4. Más utilidades

A parte de todo lo comentado anteriormente, recomiendo mucho echarle un vistazo al siguiente repositorio: 

- https://github.com/UberGuidoZ/awesome-flipperzero

Básicamente es un compendio de enlaces a los custom firmwares, bases de datos IR, canciones que se pueden agregar a la función de reproductor de música que tiene el Flipper Zero y un montón de utilidades más (algunas de ellas solo funcionan con custom firmwares).

Otro enlace de interés si tenéis una consola de Nintendo. Son un montón de Amiibos ya escaneados para emular desde el Flipper Zero: 

- https://github.com/Gioman101/FlipperAmiibo

Nota: Todas las aplicaciones y demás temas referente a Wifi no sirven por defecto. El Flipper Zero no tiene módulo Wifi incorporado pero si se puede comprar un módulo externo desde la página oficial.
