# Brief explanation about blockchain workflow

Conjunto de bloques que almacenan información que es difícil de cambiar. 

Cada bloque contiene: 

- Datos: Estos datos dependen del tipo de blockchain. Por ejemplo un Bitcoin block almacena datos sobre transacciones (origen, destino, cantidad de monedas)

- Hash del bloque: Identifica el bloque y todo su contenido, es su firma. Se calcula el hash nada mas crear el bloque y cada vez que hay un cambio.

- Hash del bloque anterior: Esto crea una cadena de bloques y lo hace tan seguro. Para prevenir el 'tampering' se utiliza esto, si un bloque cambia, cambia el hash de todos los demás 

**Bloque genesis**: Es el primer bloque de la cadena y no tiene un hash previo (primer bloque)
