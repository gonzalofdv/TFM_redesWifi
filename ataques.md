# Ataques realizados en el desarrollo del proyecto

El menú principal de Airgeddon muestra el siguiente contenido:

![airgeddon_menu](images/airgeddon_menu.png)
## Denegación de servicio (DoS)

La primera opción del apartado de ataques del menú de airgeddon es la de ataques DoS, que buscan dejar sin acceso a la red a usuarios legítimos inutilizando la red con una inundación de paquetes contra el punto de acceso e incluso contra los mismos clientes conectados, tal y como muestra el siguiente diagrama:

![deauh_diagram](images/deauth_diag.png)

Así, seleccionando la opción correspondiente, se encuentra un nuevo menú donde se pueden diferenciar dos claros apartados, con ataques para los que es necesario utilizar el modo monitor de la interfaz de red, y ataques antiguos obsoletos o útiles. Estos últimos (basados en inundar al cliente con múltiples puntos de acceso con un mismo nombre, en generar falsas autenticaciones al AP o en un ataque contra WEP) no son muy eficaces contra los protocolos más recientes, por lo que el ataque será orientado a las tres primeras opciones.

Las tres mencionadas opciones o técnicas en uso, pueden utilizarse (como es este caso) para realizar un ataque DoS, aunque más adelante serán usadas junto con otros ataques para realizar
también DoS u otras opciones específicas (como una simple deautenticación momentánea de un cliente), siendo el más común, el ataque de deautenticación realizado por la herramienta *aireplay-ng* en la que se envían múltiples paquetes de deautenticación (tal y como se ve la próxima Figura) dejando totalmente inútil el punto de acceso, para que ni siquiera los clientes puedan reconocerlo a la hora de buscar conexión.

![deauth_attack](images/deauth_attack.png)
## WPA Pre-Shared Key Cracking

Se trata del clásico ataque a WPA cuyo proceso es posible dividir en varias fases: identificación y escaneo de redes, captura del handshake y descifrado del handshake. En Airgeddon, se utilizarán las opciones 5 y 6 del menú principal. Para completar todo este proceso, se utilizan diferentes técnicas y ataques que permiten vulnerar la seguridad de una red de este tipo.

Además, la configuración del AP se ha modificado para utilizar el protocolo WPA con TKIP, además de haber establecido su contraseña a *neverhacked33*.

![apconfig](images/apconfig.png)
### Identificación y escaneo de redes

Desde el menú principal de Airgeddon, se accede a la opción número 5 "*Handshake/PMKID tools
menu*", la cuál mostrará un nuevo menú  donde se eleccionará la opción "*Explore for targets (monitor mode needed)*" que iniciará un escaneo de las redes a las que tiene alcance la tarjeta de red en uso. Así, una ventana emergente aparecerá donde van apareciendo redes e información sobre ellas, entre las que se encuentran el BSSID, el ESSID, la calidad o potencia de señal, el cifrado que utilizan y el canal en el que operan. Una vez identificado el punto de acceso objetivo, (en este caso AP_GFDV_01) se interrumpe la ejecución presionando *Ctrl+C*, dando lugar a una lista de los puntos de acceso encontrados y una entrada a consola para seleccionar el objetivo deseado. Además, esta lista informa de aquellos AP que cuentan con clientes conectados.

![aplist](images/aplist.png)
### Captura del handshake

Tras identificar el objetivo del ataque, será necesario obtener un handshake válido para proceder a la ruptura del mismo. Esto es posible conseguirlo de dos formas distintas, una lenta, que no asegura ningún tipo de éxito, la cuál consigue en esperar a que un cliente nuevo se conecte a la red o que uno ya conectado se desconecte y se vuelva a conectar, y una forma más eficaz, que consiste en realizar un ataque de deautenticación de manera que se fuerce el handshake. Esta última es la que ejecuta Airgeddon y cuyo proceso se ve reflejado en la siguiente Figura:

![wpadiag](images/wpadiag.png)

Así, seleccionar la opción "*Capture Handshake*" mostrará un nuevo menú con las opciones de deautenticación disponibles. En este caso, se hará uso de la opción "*Deauth aireplay attack*", que utiliza la herramienta *aireplay-ng* de la suite *aircrack-ng*.

Una vez seleccionada la opción de captura permite establecer la duración del ataque en un rango de 10 a 100 segundos, lo cuál debe ser suficiente para realizar la deautenticación de los  ispositivos conectados al AP Iniciado el ataque, aparecen dos ventanas emergentes, una con el proceso de captura de paquetes de la red objetivo y otra con el envío de paquetes de deautenticación.

![wpaattack](images/wpaattack.png)

En la imagen anterior es posible identificar cómo se ha conseguido capturar un handshake
tanto por el mensaje en la primera línea ’*WPA handshake: 38:72:C0:9F:14:00*’, como por la identificación del protocolo Extensible Authentication Protocol over LAN (EAPOL) en los paquetes capturados.

Este proceso, en una ejecución sin este tipo de suites requeriría el uso de comandos algo complejos en los que incluir información del AP objetivo y el uso de varios terminales simultáneos, por lo que se puede observar una gran ventaja en el uso de Airgeddon.

### Descifrado del handshake

El último paso para completar este ataque es el descifrado del handshake capturado, para lo que será necesario seleccionar la opción ’*Offline WPA/WPA2 decrypt menu*’ en el menú principal, lo que da lugar a una pantalla con las diferentes formas de realizar el ataque, entre las que se encuentran distintos variantes de uso de aircrack y hashcat, tanto para handshake como PMKID.

![wpacrack](images/wpacrack.png)

Las posibilidades para romper un handshake en este caso son cinco diferentes, pudiendo atacar
mediante diccionario o por fuerza bruta basada en reglas o generando un diccionario con crunch. Esta última opción únicamente será consultada por la gran cantidad de recursos que supone ejecutarla. Así, eligiendo la opción ’*aircrack Dictionary attack against Handshake/PMKID capture file*’, será necesario especificar la ruta del fichero que contiene el handshake así como del diccionario a utilizar, que para este caso es uno generado a partir de 100.000 líneas del conocido diccionario *rockyou.txt* incluido en Kali Linux que contiene más de 14 millones de contraseñas reales y de donde se ha sacado la contraseña configurada en el AP.

Una vez iniciado el ataque será posible visualizar el proceso de ataque por diccionario, que, en este caso, tarda un tiempo total de 4 minutos y 15 segundos en procesar obtener la contraseña y exportarla a un fichero de texto.

![aircrack](images/aircrack.png)

Como alternativa, se ha tratado de romper el mismo handshake por medio del mismo diccionario
pero esta vez utilizando la opción de *hashcat*, la cual es exitosa. Además, cabe mencionar que el tiempo de ejecución ha sido muy similar, aunque *hashcat* permite el uso de GPUs, lo que proporcionaría una mayor rapidez en caso de poder realizar el ataque mediante este hardware.

Por último, como se ha mencionado previamente, eligiendo el ataque de fuerza bruta con *aircrack* y *crunch* se pide especificar un tamaño mínimo de clave y uno máximo (entre 8 y 63), para luego especificar un conjunto de caracteres a utilizar. Eligiendo, por ejemplo, un tamaño de clave de 20 caracteres y un set de mayúsculas y números, se generarían, tal y como muestra a continuación, 2821109907456 combinaciones diferentes y un total de 23 TB de información, lo que supone unos recursos y tiempo mucho mayores de los disponibles.

![crunch](images/crunch.png)
## WPA2 PMKID

Este ataque, es el más actual contra WPA2 y permite romper la clave sin capturar un handshake completo (tan solo con el primer paquete que incluye el PMKID), y mucho más importante, sin necesidad de una tercer dispositivo cliente al que deautenticar o esperar que se conecte, puesto que será el atacante el que solicite el emparejamiento, lo que supone una gran ventaja frente a otros ataques.

![pmkiddiag](images/pmkiddiag.png)

Este ataque cuenta con las mismas fases que la ruptura de handshake previamente realizada, con la diferencia de que esta vez no se capturará un handshake completo, no se realizará un ataque de deautenticación y se hará uso de la herramienta hashcat. Además, para realizarlo, se ha configurado el AP objetivo indicando el uso del protocolo WPA2 con cifrado AES.

### Captura del PMKID

Las acciones previas a la captura del PMKID son idénticas al ataque anterior, por lo que no se verán reflejadas. A partir de ahí, y una vez situados en el menú de captura, se selecciona la opción ’*Capture PMKID*’, que solicitará un valor de timeout de entre 10 y 100 segundos antes de iniciar la captura. Esto arrancará un nuevo terminal que realiza las acciones necesarias para capturar el primer paquete del handshake que será suficiente para intentar averiguar la clave del punto de acceso.

![pmkid2](images/pmkid2.png)

Una vez finalizada la captura, aparece un mensaje informando del éxito o fracaso de la acción, además de facilitar una conversión del fichero de captura para su compatibilidad con aircrack, ya que hashcat utiliza hashes para realizar el ataque y aircrack utiliza un formato de captura de red clásico (’cap' o 'pcap’), pudiendo así evitar la estricta necesidad de utilizar hashcat para la siguiente fase.

### Ataque de diccionario contra PMKID

Situados en el menú principal y seleccionando la opción ’*Offline WPA/WPA2 decrypt menu*’ para avanzar al siguiente menú será posible seleccionar la opción asociada al ataque por diccionario a un fichero con captura de PMKID (aunque existen otras opciones ya comentadas) para posteriormente especificar la ruta del fichero a atacar y el diccionario a utilizar (en este caso el preparado para estas pruebas) y comenzar el ataque de manera automática sin necesidad de conocer los comandos específicos de hashcat.

![pmkid3](images/pmkid3.png)

Así la Figura anterior refleja cómo en algo menos de dos minutos se han procesado las 100.000 líneas del diccionario elegido y se ha hallado la contraseña del AP, que se exporta a un fichero de texto con el resultado. 

De igual manera, si se dispone de la captura de tráfico con el paquete PMKID capturado o se ha utilizado la conversión ofrecida durante el proceso de captura anterior, será posible realizar el ataque mediante aircrack, que en este caso ha ofrecido el resultado en un tiempo mayor al transcurrido con el uso de hashcat.

## Evil Twin

Este ataque tiene como principal finalidad falsificar un punto de acceso legítimo que permita
la conexión de víctimas para así obtener información sensible o confidencial, ya sea del propio cliente o de la red.
Accediendo a la opción ’*Evil Twin attacks menu*’, se mostrará un menú. Con las opciones que aparecen, se plantean dos ataques diferentes, un Evil Twin con portal cautivo y otro aplicando el sniffing de tráfico y la captura de credenciales mediante bettercap.

![eviltwin](images/eviltwin.png)

### Evil Twin con portal cautivo

Muchos puntos de acceso situados en cafeterías, centros comerciales, u otros lugares públicos cuentan con un portal cautivo en el que iniciar sesión con una contraseña o unos credenciales, por
lo que la finalidad de este ataque es simular un punto de acceso idéntico a otro legítimo en el que
aplicar un portal cautivo, para que un cliente se autentique, introduzca la clave y, dado un handshake previamente capturado, verificar si esta clave es correcta. Una diagrama del funcionamiento de este ataque se puede ver a continuación:

![evil2](images/evil2.png)

Para realizar este ataque, una vez elegida la opción ’Evil Twin AP attack with captive portal’, se ofrece elegir entre 3 métodos de deautenticación: amok mdk4, aireplay o WIDS/WIPS/WDS Confusion. Esto se debe a que para forzar la conexión de los clientes ya conectados al AP legítimo y evitar que a clientes nuevos les aparezcan dos redes iguales es recomendable (y necesario) realizar un ataque de deautenticación contra el AP legítimo provocando una denegación de servicio.

Seleccionado el método de autenticación, será necesario, si no se cuenta con uno ya, disponer de un handshake de la red objetivo, de forma que la contraseña introducida por la víctima pueda ser comprobada y evitar errores humanos. En este caso, aunque es posible especificar uno de los handshakes capturados en ataques previos, se plantea realizar todo un proceso completo, por lo que, estableciendo un tiempo de espera máximo, se realiza un ataque de deautenticación y captura de handshake, para posteriormente, especificar un fichero de salida para la captura de la contraseña e iniciar el ataque. Todo este proceso descrito se puede ver reflejado en la siguiente Figura:

![evil3](images/evil3.png)

Handshake capturado y preparado, airgeddon permite establecer el portal cautivo en 13 diferentes
idiomas, entre los que se encuentra el español. Además, permite establecer un portal cautivo avanzado detectando el fabricante del AP e implantando la misma apariencia en el portal cautivo
para una mayor fiabilidad por parte de la víctima. Como se puede observar, la  herramienta ha detectado el AP víctima como ’Comtrend’ para mostrar el portal cautivo avanzado.
Aún así, airgeddon también permite incluir portales cautivos personalizados por el atacante para adaptarse a las necesidades del ataque.

![evil4](images/evil4.png)

Iniciado el ataque, se abren 6 ventanas o componentes diferentes. Con apoyo en la Figura que se puede ver más abajo, la ventana ’AP’ configura el punto de acceso y muestra el estado de la interfaz de red y los procesos de autenticación. El terminal ’DHCP’ se encarga de asignar una IP a los nuevos dispositivos conectados. Siguiendo en la misma línea, el terminal ’Deauth’ se encarga de mantener el ataque que deniega el servicio del AP legítimo. El ’webserver’ levanta el portal cautivo. La ventana ’DNS’ se encarga de resolver las direcciones requeridas ya que estas pueden ser modificadas para una redirección maliciosa. Por último, la ventana ’Control’ informa del AP que se está simulando, de los clientes asociados, del tiempo levantado y de si se ha capturado la contraseña, entre otras cosas.

![evil5](images/evil5.png)

Así, utilizando un segundo dispositivo, en este caso un teléfono móvil, que se encuentra conectado a la red, sufre una desconexión momentánea y en unos segundos salta el portal cautivo donde la víctima introduce la contraseña de su punto de acceso y que, introduciéndola correctamente puede hacerle no sospechar de nada ya que sigue contando con conexión a Internet que proporciona el ataque Evil Twin.

![evil6](images/evil6.png)

Además, en la ventana de control, aparecerá la contraseña capturada por el portal cautivo, consiguiendo con esto acceso a la red objetivo para realizar acciones de post explotación.

### Evil Twin con sniffer de tráfico

Ejecutada la primera variante de Evil Twin, es posible afinar más este ataque aplicando un sniffer de tráfico acompañado de bettercap para intentar capturar credenciales en las interacciones de la víctima conectada al punto de acceso.

![evil7](images/evil7.png)

Así, los pasos previos a iniciar el ataque, serán idénticos al caso anterior, por lo que, mediante interacción con airgeddon, se selecciona la red objetivo, las interfaces de red a utilizar (una para denegación de servicio y otra para hacer de punto de acceso) y el fichero de salida donde se almacenarán las credenciales en caso de conseguir capturar alguna.

Al iniciar el ataque, aparecen las mismas ventanas que en el caso previo, a excepción de la ventana ’DNS’ y la ’webserver’, pero en su lugar, se muestra una nueva ventana dond se muestra el sniffer. Con la deautenticación en curso, el usuario consigue conexión, sin darse cuenta al nuevo AP. Esto se debe a que el nuevo punto de acceso no cuenta con contraseña, pero al ser idéntico y el legítimo dejar de estar disponible, el dispositivo móvil víctima se conecta de manera automática sin hacer saltar las alarmas.

Con la víctima conectada al punto de acceso malicioso, solo queda esperar a que se realice alguna acción que cuente con un acceso por credenciales para que bettercap pueda capturar estos. La Figura que aparece más abajo muestra cómo el sniffer monitorea las peticiones web, mostrando en colores destacados los credenciales capturado durante la escucha, que además, son exportados al finalizar el ataque a un fichero de texto.

![evil8](images/evil8.png)
## Ataque a WPS y Pixie Dust

WPS supone más inconvenientes que ventajas cuando se trata de seguridad y la mejor opción es mantener desactivada esta característica en el AP. Aún así, es común encontrarlo activado por defecto con un PIN de serie, lo que deriva en un fácil objetivo a atacantes y cómo no, airgeddon cuenta con diferentes opciones para atacar esta característica que se pueden consultar en el menú del ataque al que se accede seleccionando la opción ’WPS attacks menu’ en el menú principal de la herramienta.

![wps1](images/wps1.png)

Entre las opciones ofrecidas, se encuentran distintas variantes para las herramientas reaver y bully. Entre estas variantes se encuentran el seleccionar un PIN concreto (porque sea previamente conocido, para realizar pruebas o simplemente por creer conocerlo), realizar el ataque Pixie Dust, realizar fuerza bruta o atacar dados una serie de PINs obtenidos de bases de datos.

Esta última opción suele realizarse antes de intentar una enumeración completa de todas las combinaciones posibles mediante fuerza bruta ya que permite realizar intentos dados unos PINs probables de ser correctos ya que se han obtenido a partir de bases de datos de PINs WPS conocidos de fabricantes específicos y por algoritmos de generación de PINs (como ComputePIN,
EasyBox o Arcadyan, todos incluidos en airgeddon). Estos podrán ser consultados en la opcion ’*Offline PIN generation using algorithms and database*’, cuyo menú se muestra a continuación, dónde es posible observar cómo entre los PINs ofrecidos por la base de datos, se encuentra el configurado en el AP, por lo que atacar mediante este método debería ofrecer un resultado satisfactorio.

![wps2](images/wps2.png)

Tras realizar diversas pruebas, la herramienta bully no ha sido capaz de finalizar el ataque con exito, ni por fuerza bruta ni el ataque Pixie Dust, seguramente por incapacidad de la herramienta. Por el contrario, los resultados con reaver han sido satisfactorios a excepción del ataque Pixie Dust, con lo que se podría concluir que el AP objetivo no es vulnerable a este último ataque, que consiste en capturar la comunicación y descifrar el PIN en lugar de realizar una fuerza bruta. Esto se debe principalmente a que el punto de acceso presenta medidas contra este ataque.

En cuanto al resto de pruebas, en primer lugar, se selecciona la opción "*(reaver) Known PINs database based attack*", que realizará un ataque por diccionario con los posibles PINs que se observan en la Figura anterior así como los generados por los algoritmos presentes en este framework. Así se inicia el ataque, que se hará con un total de 15 PINs y con un tiempo de espera de 30 segundos para controlar posibles retrasos en la comunicación con el AP, realizando 8 intentos fallidos y consiguiendo con el PIN ’18836486’ obtener la contraseña del punto de acceso (establecida como ’neverhacked33’). La Figura a continuación muestra dichos resultados.

![wps3](images/wps3.png)

Adicionalmente, se guarda el resultado del ataque en un fichero de texto cuya ruta es determinada
previa al ataque.