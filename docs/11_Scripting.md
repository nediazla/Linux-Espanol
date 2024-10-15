# 11. Scripting Básico 
## 11.1 Introducción
En este capítulo, analizaremos cómo las herramientas que ha aprendido hasta ahora se pueden convertir en scripts reutilizables.

![](img/20241009104420.png)

## 11.2 Scripts de shell en pocas palabras
Un _script_ de shell es un archivo de comandos ejecutables que se ha almacenado en un archivo de texto. Cuando se ejecuta el archivo, se ejecuta cada comando. Los scripts de shell tienen acceso a todos los comandos del shell, incluida la lógica. Por lo tanto, un script puede probar la presencia de un archivo o buscar una salida particular y cambiar su comportamiento en consecuencia. Puede crear scripts para automatizar partes repetitivas de su trabajo, lo que libera su tiempo y garantiza la coherencia cada vez que utiliza el script. Por ejemplo, si ejecuta los mismos cinco comandos todos los días, puede convertirlos en un script de shell que reduzca su trabajo a un comando.

Un script puede ser tan simple como un comando:

```
echo "Hello, World!"
```

El script, test.sh, consta de una sola línea que imprime la cadena Hello, World! a la consola.

> Este archivo no está disponible en el entorno de máquina virtual de este curso.

La ejecución de un script se puede hacer pasándolo como un argumento a su shell o ejecutándolo directamente:

```
sysadmin@localhost:~$ sh test.sh
Hello, World!
sysadmin@localhost:~$ ./test.sh
-bash: ./test.sh: Permission denied
sysadmin@localhost:~$ chmod +x ./test.sh
sysadmin@localhost:~$ ./test.sh
Hello, World!
```

En el ejemplo anterior, primero, el script se ejecuta como un argumento para el shell. A continuación, el script se ejecuta directamente desde el shell. Es raro tener el directorio actual en la ruta de búsqueda binaria $PATH por lo que el nombre tiene el prefijo ./ para indicar que debe ejecutarse fuera del directorio actual.

El error Permiso denegado significa que el script no se ha marcado como ejecutable. Un chmod rápido más tarde y el script funciona. chmod se utiliza para cambiar los permisos de un archivo, lo que se explicará en detalle en un capítulo posterior.

Hay varios shells con su propia sintaxis de lenguaje. Por lo tanto, los scripts más complicados indicarán un shell en particular especificando la ruta absoluta al intérprete como la primera línea, con el prefijo #! Como se muestra:

```
#!/bin/sh
echo "Hello, World!"
```
o
```
#!/bin/bash
echo "Hello, World!"
```

Los dos caracteres #! se llaman tradicionalmente hash y bang respectivamente, lo que lleva a la forma abreviada de _"shebang"_ cuando se usan al comienzo de un script.

Por cierto, el shebang (o crunchbang) se utiliza para scripts de shell tradicionales y otros lenguajes basados en texto como Perl, Ruby y Python. Cualquier archivo de texto marcado como ejecutable se ejecutará bajo el intérprete especificado en la primera línea siempre que el script se ejecute directamente. Si el script se invoca directamente como argumento para un intérprete, como sh script o bash script, el shell dado se usará sin importar lo que haya en la línea shebang.

> Es útil sentirse cómodo usando un editor de texto antes de escribir scripts de shell, ya que necesitará crear archivos en texto sin formato. Las herramientas ofimáticas tradicionales como LibreOffice, que generan formatos de archivo que contienen formato y otra información, no son apropiadas para esta tarea.
## 11.3 Edición de scripts de shell
UNIX tiene muchos editores de texto. Los méritos de uno sobre el otro son a menudo objeto de acalorados debates. Dos se mencionan específicamente en el programa de estudios de LPI Essentials: El GNU nano editor es un editor muy simple muy adecuado para editar archivos de texto pequeños. El Editor Visual, vi, o su versión más reciente, VI mejorado (vim), es un editor notablemente potente pero tiene una curva de aprendizaje empinada. Nos centraremos en lo nano.

Escribe nano test.sh y verás una pantalla similar a esta:

```
GNU nano 2.2.6              File: test.sh                         modified

#!/bin/sh

echo "Hello, World!"
echo -n "the time is "
date
                                    
                                                                                
                                                                                
                                                                                
                                                                                
                                                                                
                                                                                
^G Get Help  ^O WriteOut  ^R Read File ^Y Prev Page ^K Cut Text  ^C Cur Po 
^X Exit      ^J Justify   ^W Where Is  ^V Next Page ^U UnCut Text^T To Spell
```

El editor nano tiene pocas funciones para ponerte en marcha. Simplemente escribe con el teclado, usando las **teclas de flecha** para moverte y el  botón **de eliminar**/**retroceso** para eliminar texto. En la parte inferior de la pantalla puedes ver algunos comandos disponibles, que son sensibles al contexto y cambian dependiendo de lo que estés haciendo. Si está directamente en la máquina Linux, en lugar de conectarse a través de la red, también puede usar el mouse para mover el cursor y resaltar texto.

Para familiarizarse con el editor, comience a escribir un script de shell simple mientras está dentro de nano:

```
GNU nano 2.2.6              File: test.sh                         modified

#!/bin/sh

echo "Hello, World!"
echo -n "the time is "
date
                                    
                                                                                
                                                                                                                                                                
                                                                                                                                                            
                                                                                
^G Get Help  ^O WriteOut  ^R Read File ^Y Prev Page ^K Cut Text  ^C Cur Po 
^X Exit      ^J Justify   ^W Where Is  ^V Next Page ^U UnCut Text^T To Spell
```

Tenga en cuenta que la opción de la parte inferior izquierda es ^X Exit, que significa "presione **control** y **X** para salir". Presione **Ctrl** y **X** juntos y la parte inferior cambiará:

```
Save modified buffer (ANSWERING "No" WILL DESTROY CHANGES) ?                 
 Y Yes                                                                       
 N No           ^C Cancel
```

En este punto, puede salir del programa sin guardar presionando la tecla **N**, o guardar primero presionando **Y** para guardar. El valor predeterminado es guardar el archivo con el nombre de archivo actual. Puede presionar la tecla **Intro** para guardar y salir.

Volverá al indicador del shell después de guardar. Volver al editor. Esta vez presione **Ctrl** y **O** juntos para guardar su trabajo sin salir del editor. Las indicaciones son en gran medida las mismas, excepto que estás de vuelta en el editor.

Esta vez use las teclas de flecha para mover el cursor a la línea que tiene "La hora es". Presione **Control** y **K** dos veces para cortar las dos últimas líneas en el búfer de copia. Mueva el cursor a la línea restante y presione **Control** y **U** una vez para pegar el búfer de copia en la posición actual. Esto hace que el script haga eco de la hora actual antes de saludarlo y le ahorrará tener que volver a escribir las líneas.

Otros comandos útiles que podrías necesitar son:

|**Mandar**|**Descripción**|
|---|---|
|**Ctrl** + **W**|Buscar en el documento|
|**Ctrl** + **W** y, a continuación, **Control** + **R**|Buscar y reemplazar|
|**Ctrl** + **G**|Mostrar todos los comandos posibles|
|**Ctrl** + **Y**/**V**|Subir / Bajar página|
|**Ctrl** + **C**|Mostrar la posición actual en el archivo y el tamaño del archivo|
## 11.4 Conceptos básicos de scripting
Tuvo su primer contacto con el scripting anteriormente en este capítulo, donde presentamos un script muy básico que ejecutaba un solo comando. El script comenzaba con la  _línea shebang_ (o hashbang), diciéndole a Linux que /bin/bash (que es Bash) se usará para ejecutar el script.

Además de ejecutar comandos, hay 3 temas con los que debe familiarizarse:

- Variables, que contienen información temporal en el script
- Condicionales, que te permiten hacer diferentes cosas en función de las pruebas que escribas
- Bucles, que te permiten hacer lo mismo una y otra vez
### 11.4.1 Variables
Las variables son una parte clave de cualquier lenguaje de programación. A continuación se muestra un uso muy sencillo de las variables:

```
#!/bin/bash

ANIMAL="penguin"
echo "My favorite animal is a $ANIMAL"
```

Después de la línea shebang hay una directiva para asignar algo de texto a una variable. El nombre de la variable es `ANIMAL` y el signo igual asigna la cadena `pingüino`. Piensa en una variable como una caja en la que puedes almacenar cosas. Después de ejecutar esta línea, la caja llamada `ANIMAL` contiene la palabra `pingüino`.

Es importante que no haya espacios entre el nombre de la variable, el signo igual y el elemento que se va a asignar a la variable. Si tiene un espacio allí, obtendrá un error extraño como "`comando no encontrado`". No es necesario poner en mayúsculas el nombre de la variable, pero es una convención útil para separar las variables de los comandos que se van a ejecutar.

A continuación, el script envía una cadena a la consola. La cadena contiene el nombre de la variable precedido por un signo de dólar. Cuando el intérprete ve ese signo de dólar, reconoce que estará sustituyendo el contenido de la variable, lo que se llama interpolación. La salida del script es entonces `Mi animal favorito es un pingüino`.

Así que recuerde esto: para asignar a una variable, simplemente use el nombre de la variable. Para acceder al contenido de la variable, pregóngala con un signo de dólar. ¡Aquí, mostramos una variable a la que se le asigna el contenido de otra variable!

```
#!/bin/bash

ANIMAL=penguin
SOMETHING=$ANIMAL
echo "My favorite animal is a $SOMETHING"
```

`ANIMAL` contiene la cadena `penguin` (ya que no hay espacios; en este ejemplo, se muestra la sintaxis alternativa sin usar comillas). `A` SOMETHING se le asigna entonces el contenido de `ANIMAL` (porque `ANIMAL` tiene el signo de dólar delante).

Si quisiera, podría asignar una cadena interpolada a una variable. Esto es bastante común en scripts más grandes, ya que puede crear un comando más grande y ejecutarlo.

Otra forma de asignar a una variable es usar la salida de otro comando como el contenido de la variable encerrando el comando entre ticks invertidos:

```
#!/bin/bash
CURRENT_DIRECTORY=`pwd`
echo "You are in $CURRENT_DIRECTORY"
```

Este patrón se utiliza a menudo para procesar texto. Puede tomar texto de una variable o un archivo de entrada y pasarlo a través de otro comando como `sed` o `awk` para extraer ciertas partes y mantener el resultado en una variable. El  `comando sed` se utiliza para editar flujos (STDIN) y el  comando `awk` se utiliza para la creación de scripts. Estos comandos están más allá del alcance de este curso.

Es posible obtener la entrada del usuario de su script y asignarla a una variable a través del  comando `read`:

```
#!/bin/bash

echo -n "What is your name? "
read NAME
echo "Hello $NAME!"
```

El  comando `read` puede aceptar una cadena directamente desde el teclado o como parte de la redirección de comandos, como aprendió en el capítulo anterior.

Hay algunas variables especiales además de las que usted estableció. Puede pasar argumentos a su script:

```
#!/bin/bash
echo "Hello $1"
```

Un  signo `de dólar $` seguido de un número N corresponde al enésimo argumento pasado al script. Si llama al ejemplo anterior con `./test.sh World`, la salida será `Hello World`. La  variable `$0` contiene el nombre del propio script.

Después de que se ejecuta un programa, ya sea un binario o un script, devuelve un código de salida que es un número entero entre 0 y 255. Puedes probar esto a través de $`?` para ver si el comando anterior se completó correctamente.

```
sysadmin@localhost:~$ grep -q root /etc/passwd
sysadmin@localhost:~$ echo $?
0
sysadmin@localhost:~$ grep -q slartibartfast /etc/passwd
sysadmin@localhost:~$ echo $?
1
```

El  comando `grep` se usaba para buscar una cadena dentro de un archivo con el  indicador `–q`, que significa "silencio". El `grep`, mientras se ejecuta en modo silencioso, devuelve `0` si se encontró la cadena y `1` en caso contrario. Esta información se puede utilizar en un condicional para realizar una acción basada en la salida de otro comando.

Del mismo modo, puede establecer el código de salida de su propio script con el  `comando exit`:

```
#!/bin/bash
# Something bad happened!
exit 1
```

El ejemplo anterior muestra un comentario `#`. Cualquier cosa después de la marca de almohadilla se ignora, lo que se puede usar para ayudar al programador a dejar notas. La `salida 1` devuelve el código de salida `1` a la persona que llama. Esto funciona incluso en el shell, si ejecuta este script desde la línea de comandos y luego escribe `echo $?` Verás que devuelve `1`.

Por convención, un código de salida de `0` significa "todo está bien". Cualquier código de salida mayor que `0` significa que ocurrió algún tipo de error, que es específico del programa. Arriba viste que `grep` usa `1` para significar que no se encontró la cadena.
### 11.4.2 Condicionales
Ahora que puede ver y establecer variables, es hora de hacer que su script realice diferentes funciones basadas en pruebas, llamadas _ramificaciones_. La instrucción if es el operador básico para implementar la bifurcación.

Una instrucción if básica tiene el siguiente aspecto:

```
if somecommand; then
  # do this if somecommand has an exit code of 0
fi
```

El siguiente ejemplo ejecutará "somecommand" (en realidad, todo hasta el punto y coma) y si el código de salida es 0, se ejecutará el contenido hasta el fi de cierre. Usando lo que sabes sobre grep, ahora puedes escribir un script que haga diferentes cosas en función de la presencia de una cadena en el archivo de contraseñas:

```
#!/bin/bash

if grep -q root /etc/passwd; then
  echo root is in the password file
else
  echo root is missing from the password file
fi
```

De los ejemplos anteriores, es posible que recuerde que el código de salida de grep es 0 si se encuentra la cadena. El ejemplo anterior usa esto en una línea para imprimir un mensaje si root está en el archivo de contraseña o un mensaje diferente si no lo está. La diferencia aquí es que en lugar de un fi para cerrar el bloque if, hay un else. Esto le permite realizar una acción si la condición es verdadera y otra si la condición es falsa. El bloque else aún debe estar cerrado con la palabra clave fi.

Otras tareas comunes son buscar la presencia de un archivo o directorio y comparar cadenas y números. Puede inicializar un archivo de registro si no existe o comparar el número de líneas de un archivo con la última vez que lo ejecutó. El comando if es claramente el que ayuda aquí, pero ¿qué comando usa para hacer la comparación?

El comando test le proporciona un fácil acceso a los operadores de prueba de archivos y comparación. Por ejemplo:

|**Mandar**|**Descripción**|
|---|---|
|prueba –f /dev/ttyS0|0 si el archivo existe|
|¡prueba! –f /dev/ttyS0|0 si el archivo no existe|
|prueba –d /tmp|0 si el directorio existe|
|test –x 'que ls'|Sustituya la ubicación de ls y, a continuación, pruebe si el usuario puede ejecutar|
|Prueba 1 – Ecualizador 1|0 si la comparación numérica se realiza correctamente|
|¡prueba! 1 –Ecualizador 1|NOT – 0 si se produce un error en la comparación|
|Prueba 1 –NE 1|Más fácil, prueba de desigualdad numérica|
|Prueba "A" = "A"|0 si la comparación de cadenas se realiza correctamente|
|Prueba "A" != "A"|0 si las cadenas son diferentes|
|Prueba 1 –EQ 1 –O 2 –EQ 2|-o es O: cualquiera de los dos puede ser el mismo|
|Prueba 1 –EQ 1 –A 2 –EQ 2|-a es AND: ambos deben ser iguales|

Es importante tener en cuenta que test analiza las comparaciones de enteros y cadenas de manera diferente. 01 y 1 son iguales por comparación numérica, pero no por comparación de cadenas. Siempre debe tener cuidado de recordar qué tipo de información espera.

Hay muchas más pruebas, como –gt para mayor que, formas de probar si un archivo es más nuevo que el otro y muchas más. Consulte la página del manual de prueba para obtener más información.

test es bastante detallado para un comando que se usa con tanta frecuencia, por lo que hay un alias llamado \[ (corchete izquierdo). Si encierra sus condiciones entre corchetes, es lo mismo que ejecutar una prueba. Por lo tanto, estas declaraciones son idénticas.

```
if test –f /tmp/foo; then
if [ -f /tmp/foo]; then
```

Si bien la última forma se usa con mayor frecuencia, es importante comprender que el corchete es un comando por sí solo que funciona de manera similar a test, excepto que requiere el corchete de cierre.

La instrucción if tiene una forma final que le permite realizar varias comparaciones a la vez utilizando elif (abreviatura de else if).

```
#!/bin/bash

if [ "$1" = "hello" ]; then
  echo "hello yourself"
elif [ "$1" = "goodbye" ]; then
  echo "nice to have met you"
  echo "I hope to see you again"
else
  echo "I didn't understand that"
fi
```

El código anterior compara el primer argumento pasado al script. Si es hello, se ejecuta el primer bloque. De lo contrario, el script comprueba si es un adiós y, _si es_ así, emite un mensaje diferente. De lo contrario, se envía un tercer mensaje. Tenga en cuenta que se indica la variable $1 y se utiliza el operador de comparación de cadenas en lugar de la versión numérica (-eq).

Las pruebas if/elif/else pueden llegar a ser bastante detalladas y complicadas. La declaración case proporciona una forma diferente de facilitar la realización de múltiples pruebas.

```
#!/bin/bash

case "$1" in
hello|hi)
  echo "hello yourself"
  ;;
goodbye)
  echo "nice to have met you"
  echo "I hope to see you again"
  ;;
*)
  echo "I didn't understand that"
esac
```

La instrucción case comienza con una descripción de la expresión que se está probando: case _EXPRESSION_ in. La expresión aquí es el $1 cotizado.

A continuación, cada conjunto de pruebas se ejecuta como una coincidencia de patrón terminada por un paréntesis de cierre. En el ejemplo anterior primero se busca hello o hi; Las múltiples opciones están separadas por la barra vertical | que es un operador OR en muchos lenguajes de programación. A continuación se muestran los comandos que se ejecutarán si el patrón devuelve true, que terminan con dos punto y coma. El patrón se repite.

El patrón * es el mismo que otro porque coincide con cualquier cosa. El comportamiento de la instrucción case es similar a la instrucción if/elif/else en el sentido de que el procesamiento se detiene después de la primera coincidencia. Si ninguna de las otras opciones coincide, el * garantiza que la última coincidirá.

Con una sólida comprensión de los condicionales, puede hacer que sus scripts realicen acciones solo si es necesario.
### 11.4.3 Bucles
Los bucles permiten que el código se ejecute repetidamente. Pueden ser útiles en numerosas situaciones, como cuando se desea ejecutar los mismos comandos en cada archivo de un directorio o repetir alguna acción 100 veces. Hay dos bucles principales en los scripts de shell: el bucle for y el bucle while.

Los bucles for se utilizan cuando se tiene una colección finita sobre la que se desea iterar, como una lista de archivos o una lista de nombres de servidor:

```
#!/bin/bash

SERVERS="servera serverb serverc"
for S in $SERVERS; do
  echo "Doing something to $S"
done
```

El script primero establece una variable que contiene una lista de nombres de servidor separados por espacios. A continuación, la instrucción for recorre la lista de servidores, cada vez que establece la variable S en el nombre del servidor actual. La elección de la S fue arbitraria, pero tenga en cuenta que la S no tiene signo de dólar, pero la $SERVERS sí, lo que demuestra que $SERVERS se ampliará a la lista de servidores. La lista no tiene por qué ser una variable. En este ejemplo se muestran dos formas más de pasar una lista.

```
#!/bin/bash

for NAME in Sean Jon Isaac David; do
  echo "Hello $NAME"
done

for S in *; do
  echo "Doing something to $S"
done
```

El primer bucle es funcionalmente igual que el ejemplo anterior, excepto que la lista se pasa directamente al bucle for en lugar de usar una variable. El uso de una variable ayuda a la claridad del script, ya que alguien puede realizar cambios fácilmente en la variable en lugar de mirar un bucle.

El segundo bucle usa un * que es un _archivo glob_. Esto se expande por el shell a todos los archivos en el directorio actual.

El otro tipo de bucle, un bucle while, opera en una lista de tamaño desconocido. Su trabajo es seguir ejecutándose y en cada iteración realizar una prueba para ver si debe ejecutarse en otro momento. Puedes pensar en ello como "mientras alguna condición sea cierta, haz cosas".

```
#!/bin/bash

i=0
while [ $i -lt 10 ]; do
  echo $i
  i=$(( $i + 1))
done
echo “Done counting”
```

El ejemplo anterior muestra un bucle while que cuenta de 0 a 9. Una variable de contador, i, se inicializa en 0. A continuación, se ejecuta un bucle while en el que la prueba es "¿$i es inferior a 10?" Tenga en cuenta que el bucle while utiliza la misma notación que una instrucción if.

Dentro del bucle while, se repite el valor actual de i  y luego se le agrega 1 a través del comando $(( _aritmético_ )) y se asigna de nuevo a i. Una vez que i se convierte en 10, la instrucción while devuelve false y el procesamiento continúa después del bucle.