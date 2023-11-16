# Entorno de pruebas para la realización del proyecto

Para la realización de las pruebas se han utilizado ciertos elementos para construir un entorno
adecuado con las herramientas necesarias. Estos son una Raspberry Pi, el sistema operativo Kali
Linux y una antena o adaptador WiFi externo.

## Raspberry Pi

El hardware principal que se utilizará será una Raspberry Pi. Este dispositivo se conoce como Single Board Computer (SBC), un pequeño ordenador de tamaño algo mayor a una tarjeta de crédito cuyo coste es relativamente reducido para las prestaciones que es capaz de ofrecer. El dispositivo Raspberry Pi tiene múltiples utilidades y admite muchos tipos de períféricos, siendo algunos de los más comunes los necesarios para trabajar con ella de manera interactiva como son un monitor o pantalla, un teclado y un ratón que son conectados gracias a sus puertos USB y micro High-Dfinition Multimedia Interface (HDMI).

Uno de los aspectos que diferencia la Raspberry Pi de otros SBC es que cuenta con pines General Purpose Input-Output (GPIO), los cuales permiten conectar distintos elementos hardware como pueden ser sensores, lo que la hace muy versátil y llamativa para experimentar con ella. Además, puede trabajar con diferentes sistemas operativos, contando incluso con el suyo propio, aunque para cumplir los objetivos de este trabajo se hará uso de Kali Linux.

El modelo concreto utilizado es Raspberry Pi 4 model B, el cual, en este caso, cuenta con las
siguientes características:

- Procesador Broadcom BCM2711, quad-core Cortex-A72 (ARM v8) 64-bit SoC @ 1.5GHz
- Memoria Random Accesss Memory (RAM) de 8GB LPDDR4
- Capacidad de disco de 64GB
- Puerto micro HDMI
- Alimentación meidante conector USB-C
## Kali Linux

Kali Linux es una distribución de Linux desarrollada por Offensive Security basada en Debian y
de fuente abierta preparada para realizar auditorias de seguridad y pentesting.

La compañia Offensive Security inicia el proyecto Kali Linux en 2012 buscando reemplazar a BackTrack Linux, una primera distribución GNU/Linux construida sobre Ubuntu y enfocada en seguridad informática creada por esta misma empresa (la cual contaba con otros predecesores). Así, Kali Linux se desarrolla sobre Debian por motivos de calidad y estabilidad. Tras múltiples actualizaciones, la última versión, publicada en el año 2023, es Kali Purple, ofreciendo mejoras notorias respecto a versiones anteriores.

Esta distribución está dirigida a profesionales de seguridad y administradores de Tecnologías de la Información (TI) de manera que pueden realizar auditorías de seguridad, análisis forenses, pentesting, ingeniería inversa, etc. Cuenta con más de 600 herramientas preinstaladas formando un
entorno perfectamente preparado para realizar las mencionadas actividades. Además, es posible construir una imagen de Kali totalmente personalizada (incluso a nivel de kernel) para utilizarla en lugar de la preconfigurada ofertada por Offensive Security. También destacar que existe una gama de imágenes apta para su instalación en entornos cloud, en máquinas virtuales, en USBs, Windows Subsystem for Linux (WSL) y hasta imágenes Advanced RISC Machine (ARM), de las que se hará uso para instalar en Raspberry Pi.

Con esto, Kali Linux se convierte en la distribución más extendida en el ámbito de la seguridad informática, seguida de cerca por otras, como Parrot Security OS, una alternativa conocida también basada en Debian.

## Antena

Las tarjetas de red de las que disponen por defecto dispositivos como smartphones, ordenadores portátiles, o en este caso, la Raspberry Pi no suelen ser por lo general adecuadas para realizar acciones de penetración ya que en su mayoría no permiten la inyección de paquetes o el modo monitor, por lo que es recomendable hacerse con una antena externa que sea apta para estas tareas. Además, estos adaptadores externos suelen ofrecer mayor rango de alcance, mejores señales a la hora de establecer un AP y velocidades de transmisión más rápidas.

Por ello, se adquiere y se hace uso de una antena externa USB WiFi Atheros AR9271, la cuál soporta los estándares IEEE 802.11 b/g/n, ofreciendo una velocidad de transmisión de hasta 150 mbps en 802.11n y que cuenta con 3 antenas dBi de 2,4 GHz, pudiendo operar en el rango de canales 1-13 con el rango de frecuencia de 2,4 GHz. Se trata de una antena básica, compatible con Kali Linux, de coste reducido y fácil adquisición muy común para este tipo de prácticas.

Para el correcto funcionamiento de la antena, es necesario también instalar los drivers adecuados para el sistema operativo a utilizar. Estos, se pueden obtener de manera sencilla en la página del frabricante.

## Punto de acceso objetivo

Se hará uso de un AP Comtrend AR5387un para implementar una red local controlada y segura en la que realizar los ataques. Este AP utiliza el estándar IEEE 802.11n, soporta cifrado WPA2 y es posible activar WPS vía PIN. Además, se ha modificado el SSID (que será AP_GFDV_01) y la contraseña para favorecer la brevedad del desarrollo de ataques, ya que, por ejemplo, una fuerza bruta generando un diccionario puede llevar incluso días y generar cantidades ingentes de datos para las capacidades utilizadas.
## Airgeddon

Este script bash multiusos para Linux que se puede obtener a través de su repositorio de GitHub es utilizado para auditar redes inalámbricas. Este proyecto, de libre acceso, cuenta con soporte y continuas actualizaciones. El script, cuenta con múltiples funciones, posibilitando realizar ataques DoS, cracking de handshakes mediante fuerza bruta por diccionario, ataques en línea por diccionario a WPA3, Evil Twin, Pixie Dust a WPS y ataques a WEP, entre otros. Para ejecutar dichas
actividades, hace uso de herramientas como ettercap, aireplay-ng, aircrack-ng, Bully, Reaver, Hashcat o crunch.

Además, cuenta con características que hacen de Airgeddon un framework muy llamativo como son su fácil manejo, compatibilidad con prácticamente todas las distribuciones de Linux, actualizaciones automáticas, despliegue en un contenedor de Docker y la posibilidad de crear plugins. Así, se considera uno de los frameworks de este tipo más completos.