

### Preguntas cortas sobre Bash

**1.-** *(0.4 puntos)* Desde tu directorio personal, crea un fichero en `/tmp` llamado `ev3ex1.txt` que contenga tu nombre utilizando una única orden (por supuesto sin utilizar ningún editor).

```

```


**2.-** *(0.4 puntos)* Muestra por pantalla el número de ficheros que tiene el directorio `/bin` (es decir, la salida tiene que ser únicamente un número, por ejemplo `99`).

```

```


**3.-** *(0.4 puntos)* Muestra por pantalla todos los archivos del directorio `/bin` que tengan por lo menos dos vocales en su nombre, suponiendo que te encuentras en tu directorio personal.

```

```


**4.-** *(0.4 puntos)* Estando en tu directorio personal, muestra por pantalla la quinta línea del fichero `/etc/passwd`.

```

```


**5.-** *(0.4 puntos)* Sabemos que el fichero `/usr/share/dict/spanish` contiene un listado de palabras en castellano. Calcula el número de palabras de este fichero que tienen un número impar de letras. La salida de este comando tiene que ser simplemente un número.

```

```


### Expresiones regulares

**6.-** *(2 puntos)* Indica qué expresiones regulares utilizarías para identificar los siguientes patrones, utiliza la sintaxis del motor ERE y supón  que la cadena buscada es el único contenido de cada línea.

**a)** Un NIF. Ejemplos: `2345678f`, `71.555.333N`, `10.333.222-T`, …

```

```


**b)** Una matrícula de coche (formato nuevo y antiguo). Ejemplos: `2288FSF`, `4441KSD`, `LE3308G`, `A1234AJ`, …

```

```


**c)** Una fecha de la forma `04/05/2021`

```

```

**d)** Una dirección MAC. Ejemplos: `00-09-0F-FE-00-01`, `3C:A0:67:43:D3:92`, …

```

```

**e)** Una dirección IP. Ejemplos: `192.168.1.31`, `10.0.0.1`, `172.255.255.1`, …

```

```

**7.-** (1 punto) Partiendo del fichero CSV (**Hay que añadir el enlace**), crea un fichero XML con la siguiente estructura.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<clientes>
    <cliente>
        <nombre>AcerSA</nombre>
        <cif>5664332</cif>
    </cliente>
    <cliente>
        <nombre>Mer SL</nombre>
        <cif>5111444</cif>
    </cliente>
</clientes>
```


