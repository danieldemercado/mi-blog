---
layout: post
title:  "Cómo ejecutar un nodo Bitcoin sobre Tor en Windows"
date:   2021-08-25 19:30:00 +0200
category: Bitcoin
permalink: /Bitcoin/:day-:month-:year/:title:output_ext
comments: true
---

Ejecutar tu propio nodo Bitcoin tiene un objetivo principal: apoyar a la red Bitcoin y ayudar a su descentralización. Sin embargo, la cosa no termina ahí.
Tu nodo puede convertirse en una valiosa herramienta para aumentar la privacidad en tus comunicaciones con el protocolo Bitcoin, incrementando así tu libertad y tu soberanía. Esto lo explica muy bien [@lunaticoin](https://twitter.com/lunaticoin) en el siguiente podcast:


<center> <iframe width="560" height="315" src="https://www.youtube.com/embed/C-razUWyUs0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>  

<br>
Para alcanzar esta mayor privacidad, es necesario ejecutar tu nodo Bitcoin sobre la red Tor. Existen muchas guías y tutoriales para hacer esto en Linux, pero no he encontrado ninguna guía clara para Windows. Tras varios días rompiéndome la cabeza, he conseguido montar satisfactoriamente mi nodo sobre Tor en Windows. Te dejo aquí abajo los pasos que he seguido, espero que te sean de ayuda si decides montar tu propio nodo. 



## 1. Instalación y configuración inicial de Bitcoin Core

1. Ve a [bitcoin.org/en/download](https://bitcoin.org/en/download) y descarga la última versión disponible de Bitcoin Core para Windows. En el momento en el que escribo esta guía, la última versión disponible es [Bitcoin Core 0.21.1](https://bitcoin.org/bin/bitcoin-core-0.21.1/bitcoin-0.21.1-win64-setup-unsigned.exe)

2. Ejecuta el instalador de Bitcoin Core que has descargado. Puedes instalarlo en la ruta que viene por defecto o elegir otra ruta que sea de tu preferencia.

3. Terminada la instalación, Bitcoin Core debería ejecutarse de forma automática, si no es así, ejecútalo manualmente. 

    Al ejecutarlo por primera vez, nos preguntará en qué directorio queremos que se descargue la blockchain. Aquí eres tú quien debe elegir una ruta particular (por ejemplo, si quieres que se descargue en otro disco duro, debes marcar la opción “Utilice un directorio de datos personalizado” e indicar un directorio del nuevo disco duro). Verifica que tengas suficiente espacio libre en el lugar donde vayas a descargar la blockchain. 

    Queremos  instalar un nodo completo, no uno “podado”. Asegúrate de NO tener marcada la opción “Descartar los bloques después de la verificación, excepto los 2 GB más recientes (prune)”. 

    Después de configurar los parámetros anteriores, Bitcoin Core debería iniciarse y comenzar la sincronización de la Blockchain. 

4. (Opcional pero recomendado) Si quieres usar tu nodo con otros programas como exploradores de bloques u otras wallets, necesitarás activar `txindex`. 

    En Bitcoin Core ve a `Configuración > Opciones… > Abrir archivo de configuración`. Se abrirá un archivo editable con el Bloc de notas o WordPad. En una nueva línea pega el siguiente texto: `txindex=1`.
    Guarda el archivo a través de `Archivo > Guardar` y ciérralo. 

    Para que este cambio surta efecto, debes cerrar Bitcoin Core y ejecutar un reindexado de la blockchain:

    Busca el directorio en el que has instalado Bitcoin Core (¡ojo!, NO el directorio que has indicado para descargar la blockchain), en mi caso es D:\Program Files\Bitcoin. Sabrás que estás en tu directorio de instalación correcto si dentro de este hay una aplicación denominada “bitcoin-qt”.
    Abre un terminal o símbolo de sistema (CMD) y ejecuta el comando: `cd “tu directorio”` (por ejemplo: `cd C:\Program Files\Bitcoin`).  
      
    Como yo he instalado Bitcoin Core en otro disco duro (el D: en lugar de C:) antes de ejecutar el comando anterior debo cambiar de disco duro con el comando: `D:` y posteriormente ejecutar: `cd D:\Program Files\Bitcoin`.

    Una vez dentro del directorio de instalación de Bitcoin Core en el terminal, ejecuta el comando:  
    `bitcoin-qt -reindex -txindex=1`  
    Bitcoin Core debería iniciarse y comenzar un reindexado de la blockchain. Si obtienes algún tipo de error, asegúrate de tener Bitcoin Core cerrado antes de ejecutar el comando y de estar en el directorio de instalación correcto.

5. (Opcional) Si quieres que Bitcoin Core se ejecute al iniciar Windows, en Bitcoin Core ve a `Configuración > Opciones… > Principal` y marca la casilla “Comience Bitcoin Core en el inicio de sesión del sistema”.  


## 2. Instalación y configuración de Tor

1. Ve a [torproject.org/download/](https://www.torproject.org/download/) y descarga la última versión de Tor Browser.

2. Ejecuta el instalador de Tor Browser que has descargado. Puedes instalarlo en la ruta que viene por defecto o elegir otra ruta que sea de tu preferencia.

3. Terminada la instalación, Tor Browser debería ejecutarse de forma automática (si no es así, ejecútalo manualmente). En la pestaña que se abre, marca la casilla “Always connect automatically” y presiona el botón “Connect”. Una vez conectado, comprueba que Tor Browser funciona entrando con él en la página [https://check.torproject.org/](https://check.torproject.org/) y, si todo funciona correctamente, ciérralo.

4.  Abre el archivo de configuración de Bitcoin Core de la misma forma que habíamos hecho en el punto 4 de la instalación y configuración inicial de Bitcoin Core. Si has instalado Bitcoin Core en el directorio predeterminado, también podrás encontrar este archivo en `%UserProfile%\AppData\Roaming\Bitcoin\bitcoin.conf` donde `%UserProfile%` es tu usuario de Windows. Debajo de la línea `txindex=1` que habíamos escrito anteriormente, añade los siguientes parámetros:

    ```
    listen=1 
    debug=1 
    logips=1 
    testnet=0
    listenonion=1 
    onlynet=onion 
    proxy=127.0.0.1:9150 
    torcontrol=127.0.0.1:9151 
    torpassword=YourPassword
    ```

    En la línea `torpasword=YourPassword` debes cambiar `YourPassword` por una contraseña de tu elección. Guarda el archivo a través de `Archivo > Guardar`  y ciérralo. 

    En Bitcoin Core, ve a `Configuración > Opciones… > Red`. Selecciona la casilla “Usar proxy SOCKS5 para alcanzar nodos vía servicios ocultos Tor”. Cambia el puerto 9050 a 9150 y presiona OK. Cierra Bitcoin Core.

5. Abre un terminal o símbolo de sistema (CMD). 
Si has instalado Tor Browser en el directorio predeterminado (tu escritorio) ejecuta el comando: `cd “Desktop\Tor Browser\Browser\TorBrowser\Tor"` . Si has elegido otra ruta de instalación deberás buscar tu nueva ruta hasta esta carpeta y ejecutar `cd "tu ruta"`.  Posteriormente ejecuta el comando: `tor --hash-password YourPassword >torhash.txt` (Donde de nuevo debes cambiar `YourPassword` por la contraseña que has puesto en el archivo de configuración de Bitcoin Core).

6. En el mismo directorio de trabajo (`“Desktop\Tor Browser\Browser\TorBrowser\Tor"`) se habrá creado un archivo llamado `torhash.txt` . Ábrelo y copia la última línea del mismo, debería ser un texto similar (NO igual) a `16:1CEDITEDOMERANDOMSHITCF8EBCD9A50CLOLCDACF`. 

7. Accede al directorio `Desktop\Tor Browser\Browser\TorBrowser\Data\Tor`. Allí deberías encontrar un archivo llamado `torrc`. Abrelo con el Bloc de Notas o WordPad y añade en una nueva línea el texto `HashedControlPassword` seguido del texto que has copiado del archivo `torhash.txt`. Como ejemplo, usando el texto del final del punto 6 quedaría:  
`HashedControlPassword 16:1CEDITEDOMERANDOMSHITCF8EBCD9A50CLOLCDACF`

8. Ejecuta Tor Browser y asegúrate de que esté conectado a la red Tor.

9. Ejecuta Bitcoin Core. La primera puede que tarde unos minutos hasta conectarse con los primeros nodos. Si tras esperar continúa sin conectarse, cierra Bitcoin Core y borra los archivos `peers.dat` y `onion_v3_private_key` del directorio `\Users\%UserProfile%\AppData\Roaming\Bitcoin` (el mismo directorio donde se guarda el archivo de configuración de Bitcoin Core) y vuelve a ejecutar Bitcoin Core.

10. (Opcional) Para que Tor Browser se ejecute al iniciar Windows, presiona las teclas `Windows + R` y se abrirá una pequeña pestaña denominada “Ejecutar” (si esta combinación de teclas no funciona puedes buscar en Windows la aplicación “Ejecutar”). En esta pestaña escribe el comando `shell:startup`, lo que abrirá una carpeta. Si pegas en esta carpeta un acceso directo a Tor Browser (o cualquier otro programa), este será ejecutado al iniciar Windows.


Si después de todo esto Bitcoin Core consigue conectarse a otros nodos, tu nodo debería estar correctamente configurado para funcionar sobre la red Tor.  Puedes consultar si esto es así con algunos comandos en la consola de Bitcoin Core (`Ventana > Consola`):

* `getnetworkinfo`

Este comando devuelve información sobre la configuración de red en tu nodo. Al final de la información que te da (en una sección denominada "localaddresses") deberías observar una dirección .onion (como por ejemplo 48dusp7d97ij7cziqcdkeupivntz3ygje2e75w2jchyemoejupinbmid.onion). Esta es la dirección asociada a tu nodo. Puedes comprobar si es alcanzable para otros nodos a través de la página [bitnodes.io](https://bitnodes.io/). En la sección “Join the network” introduce la dirección .onion de tu nodo y el puerto 8333. Si tu nodo es alcanzable, al presionar CHECK NODE debería aparecer un recuadro verde (con un tic verde), seguido de la dirección de tu nodo y la versión de Bitcoin Core que utilizas (por ejemplo: 48dusp7d97ij7cziqcdkeupivntz3ygje2e75w2jchyemoejupinbmid.onion:8333 /Satoshi:0.21.1/). Recuerda hacer esta comprobación desde Tor Browser, si la haces desde tu navegador usual la dirección .onion de tu nodo podría desanonimizarse.


* `getpeerinfo`

Devuelve información sobre los los nodos a los que está conectado tu nodo. Si Tor está correctamente configurado, la dirección de todos ellos (su parámetro "addr") debería ser una dirección .onion.

## 3. Problemas y soluciones

Siguiendo esta guía he conseguido montar nodos sobre Tor en dos ordenadores Windows independientes. Si tú te tropiezas con algún problema, encuentras su solución y quieres que la añada en esta sección (o simplemente quieres sugerirme alguna modificación en la guía) no dudes en escribirme con todo lujo de detalles al correo electrónico que encontrarás al final de esta web.

----------
<br>
Disfruta de tu nuevo nodo y recuerda:

<center>D O N' T &ensp; T R U S T, &ensp; V E R I F Y.</center>
<center>(N O &ensp; C O N F Í E S, &ensp; V E R I F I C A.)</center>
