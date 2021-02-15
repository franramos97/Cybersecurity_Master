# Análisis forense avanzado. P2

## 1. Extracción de archivos en capturas de paquetes de red

Los paquetes se guardan habitualmente en formato PCAP:

Global Header | Packet Header | Packet Data | Packet Header | Packet Data...

<br>

## 2. Foremost

Extraer ficheros de imágenes de disco. No es util para analizar capturas de paquetes de tráfico. **Soporta**: jpg gif png bmp avi exe mpg wav riff wmv mov pdf ole doc zip rar htm cpp.

- Si queremos escanear la particion "/dev/sdb1": 

        1. "sudo foremost -i /dev/sdb1" y crea un directorio llamado 'output'
        2. "sudo foremost -i /dev/sdb1 -o ~/Documents/output": Con -o le indicamos el output path

<br>

## 3. dd y fdisk. crear, eliminar, redimensionar, cambiar o copiar y mover particiones

El comando **dd** se usa para realizar copias de seguridad y recuperar archivos parcialmente dañados

Los comandos de **fdisk** son los siguientes:

        1. "fdisk -l": Listado de las particiones.
        2. "fdisk -l /dev/sdb": Ver las particiones de un disco específico.
        3. "fdisk -d /dev/sdb2": Borra esa partición (sbd2 que deberá ser la particion 2 del disco /dev/sdb)
        4. "fdisk -n /dev/sdb3": Crea una nueva partición
        5. "mkfs.ext4 /dev/sdb3": Le indicamos el formato de archivos para esa partición (ext4)
        6. "fdisk -s /dev/sdb2": Comprobar tamaño partición

<br>

## 4. Tcpflow. Útil para HTTP y FTP. Inutil par UDP y SMB

**Analiza capturas de paquetes de red**, manejando encabezados de protocolo, fragmentos y entrega de paquetes desordenados.

Se usa para exportar flujos de datos a archivos específicos.

- No procesa bien algunos protocolos por encima del N4 de la capa OSI (SMB por ejemplo)

Si usamos **Tcpflow con '-a'** es capaz de eliminar los encabezados del protocolo HTTP de los flujos reensamblados y descomprimir respuestas del servidor comprimidas con gzip.

<br>

## 5. Tcpxtract

Similar a foremost pero se centra en capturas de paquetes.

No muy util con formatos PE, ZIP y para UDP o TFTP.

<br>

## 6. Tcpextract

Herramienta de linea de comando.
No funciona con nombres de archivos largos.
Solo soporta protocolo HTTP.

<br>

## 7. Network Miner

Analizador gráfico de paquetes de capturas de tráfico.
Bueno para HTTP, TFTP, FTP...
No funciona con SMB.
Tarda más tiempo que otras herramientas del command line.

<br>

## 8. TShark Extractor

Herramienta del command line, es un agregado de Wireshark.
Obtiene acceso a los bytes sin procesar, quitarles la información del protocolo y escribir los datos restantes en un
archivo.

<br>

## 9. **VOLATILITY**

**Volcado de memoria**: Registro **no estructurado** del contenido de la memoria en un momento concreto. En Windows son archivos ".dmp". En Linux pueden aparecer como "vmcore" o "vmcore.incomplete".

**Realizar un volcado de memoria con Helix**: Con Helix ISO podemos hacer capturas de la RAM de nuestro sistema. Iniciamos Helix y vamos a 'Live Acquisition', establecemos el destino y el nombre de la imagen y le damos a 'Acquire. Y ya nos devuelve el volcado de nuestra memoria física

Para analizar RAM 32/64 bits en Linux, Windows, Mac y Android. Basado en Python.
Analiza volcados raw, volcados crash, volcados VMware (.vmem) y vbox.

1. **Perfil** (" --profile='perfilDeseado' "): Sirve para indicarle a Volatility el tipo de sistema operativo del que procede el volcado. Si no sabemos a qué SO procede el volcado utilizamos el plug-in "imageinfo" --> Comando: **"./vol.py imageinfo -f 'rutaDelVolcado'"**
2. **Ver los procesos existentes cuando se creó el volcado**: Comando: **" ./vol.py --profile=WinXPSP2x86 pslist -f 'rutaDelVolcado' "**
3. **Kdbgscan**: Plug-in para identificar el profile idóneo del sistema y la dirección KDBG (Kernel debugger block) correcta. Se puede usar cuando 'pslist' no muestra ningún proceso. Comando: **" ./vol.py --profile=WinXPSP2x86 kdbgscan -f 'rutaDelVolcado' "**
4. **Kpcrscan**: Plug-in para escanear estructuras KPCR (Kernel Processor Control Region). **KPCR**: Estructura de datos usada por el kernel para guardar datos específicos del procesador. Comando: **" ./vol.py --profile=WinXPSP2x86 kpcrscan -f 'rutaDelVolcado' "**
5. **Psscan**: Escanea procesos inactivos, ocultos y no enlazados por un rootkit/malware. Comando: **" ./vol.py --profile=WinXPSP2x86 psscan -f 'rutaDelVolcado' "**
6. **Dlllist**: Escanear los DLLs para los procesos corriendo. Para un process id especifico (1484) Comando: **" ./vol.py --profile=WinXPSP2x86 dlllist -p 1484 -f 'rutaDelVolcado' "**
7. **Dlldump**: Para volcar los DLLs para análisis más completo. Comando: **" ./vol.py –-profile=WinXPSP2x86 dlldump -D 'Destination Directory' -f 'rutaDelVolcado' "** 
8. **offset**: Con la opción **"--offset=0x000234"** indicamos que ejecute volatility a partir de esa dirección de memoria, lo que hace más rápido el proceso.
9. **pstree**: Como 'pslist'. Se usa para ver los procesos en forma de arbol, no muestra los procesos ocultos. Comando: **" ./vol.py -–profile=WinXPSP2x86 pstree -f 'rutaDelVolcado' "**
10. **consoles**: Para encontrar comandos usados en el 'cmd.exe' tanto en local como remoto vía backdoors. Comando: **" ./vol.py –-profile=WinXPSP2x86 consoles -f 'rutaDelVolcado' "** 
11. **verinfo**: Para ver la versión de la información para archivos PE (Portable Executable) como versión de archivo, versión de producto, OS. Comando: **" ./vol.py --plugins=contrib/plugins -f 'rutaDelVolcado' --profile=WinXPSP2x86 verinfo "** 
12. **connections**: Ver conexiones TCP que estában activas cuando se tomó el volcado. Comando: **" ./vol.py --profile=WinXPSP2x86 connections -f 'rutaDelVolcado' "**
13. **connscan**: Conexiones activas y terminadas. Comando: **" ./vol.py --profile=WinXPSP2x86 connscan -f 'rutaDelVolcado' "**
14. **sockets**: Ver conexiones de sockets a la escucha, incluye conexiones TCP y UDP. Comando: **" ./vol.py --profile=WinXPSP2x86 sockets -f 'rutaDelVolcado' "**
15. **hivescan**: Encontrar direcciones físicas de colmenas de registro en la memoria. Comando: **" ./vol.py --profile=WinXPSP2x86 hivescan -f 'rutaDelVolcado' "**
16. **hivelist**: Direcciones virtuales de colmenas de registro en la memoria. Comando: **" ./vol.py --profile=WinXPSP2x86 hivelist -f 'rutaDelVolcado' "**
17. **svcscan**: Lista de servicios corriendo en el sistema.

<br>

## 10. Uso de la herramienta Autopsy

Util para crear un timeline del sistema.

Link de descarga: http://www.sleuthkit.org/autopsy/

Guía sobre la herramienta: https://azeemnow.com/2015/06/04/autopsy-forensics-review/

Guía sobre cómo crear un caso y añadir evidencias en Autopsy: http://www.sleuthkit.org/autopsy/docs/quick/index.html


<br>

## 11. Strings

Recopila los strings tanto de ASCII como de Unicode a la vez que busca palabras clave.

- Para strings **ASCII**: Comando: **strings -td /dev/sdb > sdb.ascii** '-td' indica printar el offset en decimal.
- Para strings **Unicode**: Comando: **strings -td -eñ /dev/sdb > sdb.unicode** '-el' indica que maneja little-endian de 16 bits.

Una vez tenemos los ficheros de output (sacados como se muestra anteriormente, 'sdb.ascii' y 'sdb.unicode') podemos buscar palabras clave con 'grep':

        "grep -i 'palabraBuscada' sdb.ascii > sdb.ascii.keyword" con '-i' indicamos que no sea case-sensitiv.

        "grep -i -f ficheroPalabras.txt sdb.ascii > sdb.ascii.keyword". Con '-f' podemos pasarle un fichero con las palabras a buscar (ficheroPalabras.txt)

        "egrep -color -i -f ficheroPalabras.txt sdb.ascii". Con 'egrep -color' nos resalta los resultados que hagan match en color


<br>

## 12. Comprobar tamaño de un Cluster

**ntfsinf**: Para ver el tamaño de un sistema de archivos NTFS. 
- Comando: **"ntfsinfo -mft /dev/sda1"**, nos muestra el tamaño del sector y del cluster.

- Para Linux el tamaño de bloque se puede encontrar con el comando: **"tune2fs -l /dev/sda2 | grep Block"**

<br>

## 13. **Video 'Digital Forensic Memory Analysis - strings, grep and photorec'**

Enlace del video: https://www.youtube.com/watch?v=4XoidAheuJE

Partimos de una imagen llamada "exercise1.raw" que pesa 1GB.

1. Uso de **cat, strings y grep**

    - De primeras podemos ver todos los datos haciendo el comando **"cat exercise1.raw | strings""**, leer todos los datos y enviarlos al programa 'strings' que nos sacará todo como strings (los convertirá a datos legibles). 

    - **"cat exercise1.raw | strings > dictionary.txt"** ahí tendremos todos los strings que están disponibles en memoria, podría haber algún password. Útil para luego usar ese 'dictionary.txt' como diccionario para conseguir algunas credenciales si la pwd está en memoria (por lo tanto estará en dictionary.txt)

    - **"cat exercise1.raw | strings | grep "netsec" "** ahora grepeamos buscando entre los strings la palabra 'netsec'

2. Uso de **photorec**: Obtiene además archivos de nuestro volcado. 

    - Comando: **"photorec exercise1.raw"** --> Nos muestra el disco (exercise1.raw), lo seleccionamos y podemos ver las particiones, si vemos 'Unknown' es que no hay particiones. Podemos ir a la opcion [File Opt] y elegir el tipo de estructuras de datos a buscar dentro de 'exercise1.raw'. Una vez ehecho lo anterior le damos a [Search], como no tenemos una partición tampoco tenemos un sistema de ficheros, por lo que le damos a la opción [Other] y ya debemos darle un lugar donde almacenar los datos, cuando tengamos el directorio donde guardarlos pulsamos 'c' y ya tenemos los resultados donde le hayamos indicado.
