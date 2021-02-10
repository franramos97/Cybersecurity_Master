# Clase 5: Seguridad Ofensiva

## 1. Team Viewer: Este es mi acceso principal al equipo (mediante credenciales lo he conseguido, raid forums)

Ahora podemos Crear un Canal --> Tener una herramienta para poder comunicarme con un C2 (Cobalt Strike,Metasploit). Para el C2 podríamos tener un VPS (Virtual Private Server, desde aquí tienes el listener)

## 2. Entrega por correo electrónico

- Construir un servidor SMTP legítimo y crearle una reputación, para que puedan confiar en ella. Por ello hay que hacerlo con tiempo.
  
- Los dominios caducan, estar atento de dominios que hayan caducado y los renuevas para así arrastrar la reputación que tenían.
  
- Comprometo la cuenta de un tercero y desde ahí ataco.

### 2.1 Herramientas:

- GoPhish

### 2.2 Adjuntos ofimáticos maliciosos:

 En excel es más tipico que tenga macros por lo que usarlas aquí en vez de en Word porque en Word no suelen estar.

- Office Macros

## 3. Entrega vía Web

Técnicas que obligen a que el usuario entre al sitio web y se descargue algo que ejecute un código.

### 3.1 Exploit kits

En muchas empresas no permiten que se actualicen automáticamente los navegadores.

Los navegadores son dificiles de hackear porque:

1. Entorno sandboxing: Requeriría dos vulnerabilidades.
2. Los navegadores se actualizan automáticamente.

## 4. Entrega vía dispositivos extraíbles (USB)

Relativamente sencillo

## 5. Explotación de vulnerabilidades

### 5.1 Fuentes para determinar vulnerabilidades:

cvedetails.com

vulmon.com

searchsploit

1. Pasiva: Recopilo versiones y cruzo con vulnerabilidades conocidas

2. Activa:

### 5.1 Escáneres de vulnerabilidades

Nexpose - Nessus - InsightVM - OpenVAS - Acunetix

## 6. METASPLOIT

Debemos huir de las herramientas automáticas

Es extensible, modular (podemos incluir elementos adicionales dentro de la estructura) y de codigo abierto.

Los módulos se distinguen en distintos bloques (Auxiliary, Exploit, Payloads (piezas de codigo q quiero ejecutar como resultado de una determinada epxlotacion), Post (modulos de post-explotacion), Encoders (relacionadas con la evasión) , Nops)

Estos módulos son directorios de la máquina: /usr/share/metasploit-framework/modules 

### 6.1 Búsqueda de Exploits con METASPLOIT

Con la orden 'search' podemos realizar busquedas sobre los exploits existentes

- Ejemplo: search arch:x86 type:exploit cve:2020.


Reverse shell: El servidor se conecta a nosotros. Previamente debe existir un puerto a la escucha
