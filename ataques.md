# Ataques realizados en el desarrollo del proyecto

El menú principal de Airgeddon muestra el siguiente contenido:

![airgeddon_menu](airgeddon_menu.png)
## Denegación de servicio (DoS)

La primera opción del apartado de ataques del menú de airgeddon es la de ataques DoS, que buscan dejar sin acceso a la red a usuarios legítimos inutilizando la red con una inundación de paquetes contra el punto de acceso e incluso contra los mismos clientes conectados, tal y como muestra el siguiente diagrama:

![deauh_diagram](deauth_diag.png)

Así, seleccionando la opción correspondiente, se encuentra un nuevo menú donde se pueden diferenciar dos claros apartados, con ataques para los que es necesario utilizar el modo monitor de la interfaz de red, y ataques antiguos obsoletos o útiles. Estos últimos (basados en inundar al cliente con múltiples puntos de acceso con un mismo nombre, en generar falsas autenticaciones al AP o en un ataque contra WEP) no son muy eficaces contra los protocolos más recientes, por lo que el ataque será orientado a las tres primeras opciones.

Las tres mencionadas opciones o técnicas en uso, pueden utilizarse (como es este caso) para realizar un ataque DoS, aunque más adelante serán usadas junto con otros ataques para realizar
también DoS u otras opciones específicas (como una simple deautenticación momentánea de un cliente), siendo el más común, el ataque de deautenticación realizado por la herramienta *aireplay-ng* en la que se envían múltiples paquetes de deautenticación (tal y como se ve la próxima Figura) dejando totalmente inútil el punto de acceso, para que ni siquiera los clientes puedan reconocerlo a la hora de buscar conexión.

![deauth_attack](deauth_attack.png)
## WPA Pre-Shared Key Cracking



## WPA2 PMKID

## Evil Twin

## Ataque a WPS y Pixie Dust