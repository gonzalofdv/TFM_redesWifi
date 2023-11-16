# Análisis de herramientas de penetración de redes WiFi en Raspberry Pi

Trabajo de Fin de Máster para la Universidad Europea de Madrid en el Máster Universitario en Seguridad de las Tecnologías de la Información y las Comunicaciones. Curso 2022-2023

---------------------------------------

## Resumen del proyecto

Las redes de comunicaciones, en especial las redes inalámbricas, entre las que destacan las Wireless Local Area Network (WLAN), están presentes en prácticamente cualquier lugar donde se
encuentre una conexión a Internet por medio de un router. Así, es posible encontrar estas redes en
empresas, comercios, hogares e incluso en lugares públicos, siendo utilizados por usuarios sin conocimientos sobre ellas y pudiendo ser detectadas por cualquier dispositivo en el radio de alcance de la red, lo que brinda la oportunidad a ser atacadas por ciberdelincuentes. Es por esto que este trabajo arranca con la idea de analizar los protocolos de seguridad aplicables a estas redes y las distintas vulnerabilidades asociadas a ellos, encontrando herramientas dedicadas a explotarlas y comparándolas con el fin de encontrar cuál es la más completa y la cuál ofrece mejores resultados y rendimiento. Tras un análisis de los ataques que se pueden practicar contra redes inalámbricas y de las herramientas que los realizan, se elige Airgeddon como el framework para explotar sus capacidades en un entorno controlado formado por un Access Point (AP) víctima, una Raspberry Pi con Kali Linux como sistema operativo para realizar los ataques a la que se integra una antena Wireless Fidelity (WiFi) y una serie de dispositivos que se conectarán al punto de acceso, demostrando cómo es posible vulnerar la seguridad de estas redes con pocos recursos y en tiempos reducidos si no se aplican buenas prácticas de seguridad.

## Palabras clave

Redes inalámbricas, redes WiFi, WEP, WPA, WPA2, WPA3, Airgeddon, aircrack-ng, Raspberry
Pi, Kali Linux.

## Documentos

Entre los archivos incluidos en este repositorio se pueden encontrar la [memoria del TFM](TFM_Gonzalo_Figueroa_del_Val.pdf), la [presentación](Defensa_TFM.pdf) utilizada en la defensa, un [Manual de instalación de Kali Linux en Raspberry Pi](manual_instalacion.md) y un [resumen de los ataques realizados](ataques.md).