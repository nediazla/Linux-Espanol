# 9. Archivando y Compresion
## 9.1 Introducción
En este capítulo, discutimos cómo administrar archivos de almacenamiento en la línea de comandos. El archivado de archivos se utiliza cuando uno o más archivos deben transmitirse o almacenarse de la manera más eficiente posible. Hay dos aspectos fundamentales que se exploran en este capítulo:
	•	Archivado: Combina varios archivos en uno, lo que elimina la sobrecarga en archivos individuales y facilita la transmisión de los archivos.
	•	Compresión: Hace que los archivos sean más pequeños al eliminar la información redundante.
	
> Los archivos se pueden comprimir individualmente, o se pueden combinar varios archivos en un solo archivo y luego comprimirlos. Esto último todavía se conoce como archivo.

Cuando se descomprime un archivo y se extraen uno o más archivos, esto se denomina desarchivado.

A pesar de que el espacio en disco es relativamente barato, el archivado y la compresión siguen teniendo valor:
	•	Al poner a disposición un gran número de archivos, como el código fuente de una aplicación o una colección de documentos, es más fácil para las personas descargar un archivo comprimido que descargar archivos individualmente.
	•	Los archivos de registro tienen la costumbre de llenar los discos, por lo que es útil dividirlos por fecha y comprimir las versiones anteriores.
	•	Al hacer una copia de seguridad de los directorios, es más fácil mantenerlos todos en un solo archivo que versionar (actualizar) cada archivo.
	•	Algunos dispositivos de streaming, como las cintas, funcionan mejor si envías un flujo de datos en lugar de archivos individuales.
	•	A menudo puede ser más rápido comprimir un archivo antes de enviarlo a una unidad de cinta o a través de una red más lenta y descomprimirlo en el otro extremo de lo que sería enviarlo sin comprimir.

Es esencial que los administradores de Linux se familiaricen con las herramientas para archivar y comprimir archivos.

![](img/20241007094304.png)
## 9.2 Comprimir archivos
_La compresión_ reduce la cantidad de datos necesarios para almacenar o transmitir un archivo mientras se almacena de tal manera que el archivo se puede restaurar. Un archivo con texto legible por humanos puede tener palabras reemplazadas con frecuencia por algo más pequeño, o una imagen con un fondo sólido puede representar parches de ese color mediante un código. La versión comprimida del archivo no se suele ver ni utilizar, sino que se descomprime antes de su uso.

El _algoritmo de compresión_ es un procedimiento que utiliza la computadora para codificar el archivo original y, como resultado, hacerlo más pequeño. Los informáticos investigan estos algoritmos y crean otros mejores que pueden funcionar más rápido o hacer que el archivo de entrada sea más pequeño.

Cuando se habla de compresión, hay dos tipos:

- _Sin_ pérdidas: No se elimina ninguna información del archivo. Al comprimir un archivo y descomprimirlo, queda algo idéntico al original.
- _Con pérdida_: Es posible que la información se elimine del archivo. Está comprimido de tal manera que al descomprimir un archivo se obtendrá un archivo ligeramente diferente del original. Por ejemplo, una imagen con dos tonos de verde sutilmente diferentes podría hacerse más pequeña tratando esos dos tonos como si fueran iguales. A menudo, el ojo no puede distinguir la diferencia de todos modos.

Por lo general, los ojos y oídos humanos no notan ligeras imperfecciones en las imágenes y el audio, especialmente cuando se muestran en un monitor o se reproducen en altavoces. La compresión con pérdida a menudo beneficia a los medios porque da como resultado tamaños de archivo más pequeños y las personas no pueden diferenciar entre el original y la versión con los datos modificados. Para las cosas que deben permanecer intactas, como documentos, registros y software, necesita compresión sin pérdidas.

La mayoría de los formatos de imagen, como GIF, PNG y JPEG, implementan algún tipo de compresión. Los archivos JPEG utilizan la compresión con pérdidas, mientras que los GIF y los PNG se comprimen pero sin pérdidas. Por lo general, puede decidir cuánta calidad desea conservar. Una calidad más baja da como resultado un archivo más pequeño, pero después de la descompresión, puede notar artefactos como bordes ásperos o decoloraciones. La alta calidad se parecerá mucho a la imagen original, pero el tamaño del archivo será más cercano al original.

Comprimir un archivo ya comprimido no lo hará más pequeño. Este hecho a menudo se olvida cuando se trata de imágenes, ya que ya están almacenadas en un formato comprimido. Con la compresión sin pérdidas, esta compresión múltiple no es un problema, pero si comprimes y descomprimes un archivo varias veces utilizando un algoritmo con pérdidas, eventualmente tendrás algo que es irreconocible.

Linux proporciona varias herramientas para comprimir archivos; El más común es GZIP. Aquí mostramos un archivo antes y después de la compresión:

```
**sysadmin@localhost:~$** cd Documents
**sysadmin@localhost:~/Documents$** ls -l longfile*
-rw-r--r-- 1 sysadmin sysadmin 66540 Dec 20  2017 longfile.txt
**sysadmin@localhost:~/Documents$** gzip longfile.txt
**sysadmin@localhost:~/Documents$** ls -l longfile*
-rw-r--r-- 1 sysadmin sysadmin 341 Dec 20  2017 longfile.txt.gz
```

En el ejemplo anterior, hay un archivo llamado longfile.txt que es de 66540 bytes. El archivo se comprime invocando el comando gzip con el nombre del archivo como único argumento. Una vez completado ese comando, el archivo original desaparece y se deja en su lugar una versión comprimida con una extensión de archivo de .gz. El tamaño del archivo es ahora de 341 bytes.

El comando gzip proporcionará esta información, mediante el uso de la opción –l, como se muestra aquí:

```
**sysadmin@localhost:~/Documents$** gzip -l longfile.txt.gz
         compressed        uncompressed  ratio uncompressed_name
                341               66540  99.5% longfile.txt
```

La relación de compresión es del 99,5%, una reducción impresionante ayudada por la información repetitiva en el archivo original. Además, cuando se descomprima el archivo, se volverá a llamar longfile.txt.
Los archivos comprimidos se pueden restaurar a su forma original utilizando el comando gunzip o el comando gzip –d. Este proceso se denomina descompresión. Después de que gunzip hace su trabajo, el archivo longfile.txt se restaura a su tamaño y nombre de archivo originales.

```
sysadmin@localhost:~/Documents$ gunzip longfile.txt.gz
sysadmin@localhost:~/Documents$ ls -l longfile*
-rw-r--r-- 1 sysadmin sysadmin 66540 Dec 20  2017 longfile.txt
```

> El comando gunzip es solo un script que llama a gzip con los parámetros correctos.

Hay otros comandos que funcionan prácticamente de forma idéntica a gzip y gunzip. Estos incluyen bzip2 y bunzip2, así como xz y unxz.

El comando gzip utiliza el algoritmo de compresión de  datos Lempel-Ziv, mientras que las utilidades bzip utilizan un algoritmo de compresión diferente llamado  clasificación de bloques Burrows-Wheeler, que puede comprimir archivos más pequeños que gzip a expensas de más tiempo de CPU. Estos archivos se pueden reconocer porque tienen una extensión .bz o .bz2 en lugar de una extensión .gz.

Las herramientas xz y unxz son funcionalmente similares a gzip y gunzip en el sentido de que utilizan el algoritmo de  cadena Lempel-Ziv-Markov (LZMA), que puede dar como resultado tiempos de CPU de descompresión más bajos que están a la par con gzip, al tiempo que proporcionan las mejores relaciones de compresión típicamente asociadas con las herramientas bzip2. Los archivos comprimidos con el comando xz utilizan la extensión .xz.
## 9.3 Archivado de archivos
Si tienes varios archivos para enviar a alguien, puedes optar por comprimir cada uno individualmente. Tendría una cantidad menor de datos en total que si enviara archivos sin comprimir, sin embargo, aún tendría que lidiar con muchos archivos a la vez.

_El archivo_ es la solución a este problema. La utilidad tradicional de UNIX para archivar archivos se llama tar, que es una forma abreviada de _TApe aRchive_. Se utilizaba para transmitir muchos archivos a una cinta para copias de seguridad o transferencia de archivos. El comando tar toma varios archivos y crea un solo archivo de salida que se puede dividir nuevamente en los archivos originales en el otro extremo de la transmisión.

El comando tar tiene tres modos con los que es útil familiarizarse:

- _Crear:_ Cree un nuevo archivo a partir de una serie de archivos.
- _Extracción:_ Extraiga uno o más archivos de un archivo.
- _Lista: Muestra_ el contenido del archivo sin extraerlo.

Recordar los modos es clave para averiguar las opciones de línea de comandos necesarias para hacer lo que desea. Además del modo, recuerde dónde especificar el nombre del archivo, ya que es posible que esté ingresando varios nombres de archivo en una línea de comandos.
### 9.3.1 Modo de creación

```
tar -c [-f ARCHIVE] [OPTIONS] [FILE...]
```

La creación de un archivo con el comando tar requiere dos opciones con nombre:

|**Opción**|**Función**|
|---|---|
|-c|Cree un archivo.|
|-f ARCHIVO|Utilice el archivo de almacenamiento.<br><br>El argumento ARCHIVE será el nombre del archivo de almacenamiento resultante.|

Todos los argumentos restantes se consideran nombres de archivo de entrada, ya sea como un comodín, una lista de archivos o ambos.

En el siguiente ejemplo se muestra un _archivo tar_, también llamado _tarball_, que se crea a partir de varios archivos. El primer argumento crea un archivo llamado alpha_files.tar. La opción comodín * se utiliza para incluir todos los archivos que comienzan con alpha en el archivo:

```
sysadmin@localhost:~/Documents$ tar -cf alpha_files.tar alpha*
sysadmin@localhost:~/Documents$ ls -l alpha_files.tar
-rw-rw-r-- 1 sysadmin sysadmin 10240 Oct 31 17:07 alpha_files.tar
```

El tamaño final de alpha_files.tar es de 10240 bytes. Normalmente, los archivos tarball son ligeramente más grandes que los archivos de entrada combinados debido a la información de sobrecarga al recrear los archivos originales. Los tarballs se pueden comprimir para facilitar el transporte, ya sea usando gzip en el archivo o haciendo que tar lo haga con la opción -z.

|**Opción**|**Función**|
|---|---|
|-z|Comprima (o descomprima) un archivo comprimido mediante el comando gzip.|

El siguiente ejemplo muestra el mismo comando que el ejemplo anterior, pero con la adición de la opción -z.

```
sysadmin@localhost:~/Documents$ tar -czf alpha_files.tar.gz alpha*
sysadmin@localhost:~/Documents$ ls -l alpha_files.tar.gz
-rw-rw-r-- 1 sysadmin sysadmin 417 Oct 31 17:15 alpha_files.tar.gz
```

La salida es mucho más pequeña que el propio tarball, y el archivo resultante es compatible con gzip, que se puede utilizar para ver los detalles de compresión. El archivo sin comprimir es del mismo tamaño que tendría si lo hubieras comprimido en un paso separado:

```
sysadmin@localhost:~/Documents$ gzip -l alpha_files.tar.gz
         compressed        uncompressed  ratio uncompressed_name
                417               10240  96.1% alpha_files.tar
```

Si bien las extensiones de archivo no afectan la forma en que se trata un archivo, la convención es usar .tar para los archivos comprimidos y .tar.gz o .tgz para los archivos comprimidos.

La compresión bzip2 se puede usar en lugar de gzip sustituyendo la opción -j por la opción -z y usando .tar.bz2, .tbz o .tbz2 como extensión de archivo.

|**Opción**|**Función**|
|---|---|
|-j|Comprima (o descomprima) un archivo comprimido mediante el comando bzip2.|

Por ejemplo, para archivar y comprimir el directorio de la escuela:

```
sysadmin@localhost:~/Documents$ tar -cjf folders.tbz School
```
### 9.3.2 Modo de lista

```
tar -t [-f ARCHIVE] [OPTIONS]
```

Dado un archivo tar, comprimido o no, puede ver lo que hay en él utilizando la opción -t. En el siguiente ejemplo se utilizan tres opciones:

|**Opción**|**Función**|
|---|---|
|-t|Enumere los archivos en un archivo.|
|-j|Descomprima con un comando bzip2.|
|-f ARCHIVO|Opere en el archivo dado.|

Para enumerar el contenido del archivo folders.tbz:

```
sysadmin@localhost:~/Documents$ tar -tjf folders.tbz
School/
School/Engineering/
School/Engineering/hello.sh
School/Art/
School/Art/linux.txt
School/Math/
School/Math/numbers.txt
```

En el ejemplo, el directorio School/ tiene el prefijo de los archivos. El comando tar se repetirá en subdirectorios automáticamente al comprimir y almacenará la información de la ruta dentro del archivo.

> Para mostrar que este archivo todavía no es nada especial, enumeraremos el contenido del archivo en dos pasos usando una canalización, el | carácter.

```
sysadmin@localhost:~/Documents$ bunzip2 -c folders.tbz | tar -t
School/
School/Engineering/
School/Engineering/hello.sh
School/Art/
School/Art/linux.txt
School/Math/
School/Math/numbers.txt
```

> El lado izquierdo de la canalización es bunzip2 –c folders.tbz, que descomprime el archivo, pero la opción -c envía la salida a la pantalla. La salida se redirige a tar –t. Si no especifica un archivo con –f, tar leerá desde la entrada estándar, que en este caso es el archivo sin comprimir.

> Las tuberías y los insumos estándar se tratarán en detalle más adelante en el curso.

> Los ejemplos anteriores están diseñados para demostrar la capacidad del comando tar para recursarse en subdirectorios. Los archivos que se muestran en el ejemplo no están disponibles en el entorno de máquina virtual de este curso.
### 9.3.3 Modo de extracción

```
tar -x [-f ARCHIVE] [OPTIONS]
```

La creación de archivos se utiliza a menudo para facilitar el movimiento de varios archivos. Antes de extraer los archivos, reubíquelos en el directorio de descargas:

```
sysadmin@localhost:~/Documents$ cd ~
sysadmin@localhost:~$ cp Documents/folders.tbz Downloads/folders.tbz
sysadmin@localhost:~$ cd Downloads
```

Finalmente, puede extraer el archivo con la opción –x una vez que se haya copiado en un directorio diferente. En el ejemplo siguiente se usa un patrón similar al anterior, especificando la operación, la compresión y un nombre de archivo en el que operar.

|**Opción**|**Función**|
|---|---|
|-x|Extraer archivos de un archivo.|
|-j|Descomprima con el comando bzip2.|
|-f ARCHIVO|Opere en el archivo dado.|
```
sysadmin@localhost:~/Downloads$ tar -xjf folders.tbz
sysadmin@localhost:~/Downloads$ ls -l
total 8
drwx------ 5 sysadmin sysadmin 4096 Dec 20  2017 School
-rw-rw-r-- 1 sysadmin sysadmin  413 Oct 31 18:37 folders.tbz
```

El archivo original no se toca y se crea el nuevo directorio. Dentro del directorio, se encuentran los directorios y archivos originales.

```
sysadmin@localhost:~/Downloads$ cd School
sysadmin@localhost:~/Downloads/School$ ls -l
total 12
drwx------ 2 sysadmin sysadmin 4096 Oct 31 17:45 Art
drwx------ 2 sysadmin sysadmin 4096 Oct 31 17:47 Engineering
drwx------ 2 sysadmin sysadmin 4096 Oct 31 17:46 Math
```

Agregue el indicador –v y obtendrá una salida detallada de los archivos procesados, lo que facilita el seguimiento de lo que está sucediendo:

|**Opción**|**Función**|
|---|---|
|-v|Enumere detalladamente los archivos procesados.|

El siguiente ejemplo repite el ejemplo anterior, pero con la adición de la opción –v:

```
sysadmin@localhost:~/Downloads$ tar -xjvf folders.tbz
School/
School/Engineering/
School/Engineering/hello.sh
School/Art/
School/Art/linux.txt
School/Math/
School/Math/numbers.txt
```

Es importante mantener el indicador –f al final, ya que tar asume que lo que sigue a esta opción es un nombre de archivo. En el siguiente ejemplo, se transpusieron los indicadores –f y –v, lo que llevó a tar a interpretar el comando como una operación en un archivo llamado v, que no existe.

```
sysadmin@localhost:~/Downloads$ tar -xjfv folders.tbz 
tar (child): v: Cannot open: No such file or directory
tar (child): Error is not recoverable: exiting now
tar: Child returned status 2
tar: Error is not recoverable: exiting now
```

Si solo desea que algunos archivos salgan del archivo, agregue sus nombres al final del comando, pero de forma predeterminada, deben coincidir exactamente con el nombre del archivo o usar un patrón.

En el ejemplo siguiente se muestra el mismo archivo que antes, pero extrayendo solo el archivo School/Art/linux.txt. La salida del comando (ya que el modo detallado se solicitó con el indicador –v) muestra que solo se ha extraído un archivo:

```
sysadmin@localhost:~/Downloads$ tar -xjvf folders.tbz School/Art/linux.txt
School/Art/linux.txt
```

El comando tar tiene muchas más características, como la capacidad de usar patrones al extraer archivos, excluir ciertos archivos o enviar los archivos extraídos a la pantalla en lugar de al disco. La documentación de tar tiene información detallada.
## 9.4 Archivos ZIP
La utilidad de archivado de facto en Microsoft es el archivo ZIP. ZIP no es tan frecuente en Linux, pero está bien soportado por los comandos zip y unzip. Sin embargo, con tar y gzip/gunzip los mismos comandos y opciones se pueden usar indistintamente para hacer la creación y extracción, pero este no es el caso con zip. La misma opción tiene significados diferentes para los dos comandos diferentes.

El modo predeterminado de zip es agregar archivos a un archivo y comprimirlo.

```
zip [OPTIONS] [zipfile [file…]]
```

El primer argumento zipfile es el nombre del archivo que se va a crear, después de eso, una lista de archivos que se van a agregar. En el ejemplo siguiente se muestra un archivo comprimido llamado alpha_files.zip que se está creando:

```
sysadmin@localhost:~/Documents$ zip alpha_files.zip alpha*
  adding: alpha-first.txt (deflated 32%)
  adding: alpha-second.txt (deflated 36%)
  adding: alpha-third.txt (deflated 48%)
  adding: alpha.txt (deflated 53%)
  adding: alpha_files.tar.gz (stored 0%) 
```

La salida muestra los archivos y la relación de compresión.

Debe tenerse en cuenta que tar requiere la opción –f para indicar que se está pasando un nombre de archivo, mientras que zip y unzip requieren un nombre de archivo y, por lo tanto, no necesitan que informe al comando que se está pasando un nombre de archivo.

El comando zip no se repetirá en los subdirectorios de forma predeterminada, lo cual es un comportamiento diferente al comando tar. Es decir, simplemente agregando School solo se agregará el directorio vacío y no los archivos que se encuentran debajo de él. Si desea un comportamiento similar a tar, debe usar la opción –r para indicar que se va a usar la recursividad:

```
sysadmin@localhost:~/Documents$ zip -r School.zip School
updating: School/ (stored 0%)
updating: School/Engineering/ (stored 0%)
updating: School/Engineering/hello.sh (deflated 88%)
updating: School/Art/ (stored 0%)
updating: School/Art/linux.txt (deflated 49%)
updating: School/Math/ (stored 0%)
updating: School/Math/numbers.txt (stored 0%)
  adding: School/Art/red.txt (deflated 33%)
  adding: School/Art/hidden.txt (deflated 1%)
  adding: School/Art/animals.txt (deflated 2%)
```

En el ejemplo anterior, se agregan todos los archivos del directorio School porque utiliza la opción –r. Las primeras líneas de salida indican que se agregaron directorios al archivo, pero por lo demás la salida es similar al ejemplo anterior.

La opción –l _list_ del comando unzip enumera los archivos en .zip archivos:

```
sysadmin@localhost:~/Documents$ unzip -l School.zip
Archive:  School.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
        0  2017-12-20 16:46   School/
        0  2018-10-31 17:47   School/Engineering/
      647  2018-10-31 17:47   School/Engineering/hello.sh
        0  2018-10-31 19:31   School/Art/
       83  2018-10-31 17:45   School/Art/linux.txt
        0  2018-10-31 17:46   School/Math/
       10  2018-10-31 17:46   School/Math/numbers.txt
       51  2018-10-31 19:31   School/Art/red.txt
       67  2018-10-31 19:30   School/Art/hidden.txt
       42  2018-10-31 19:31   School/Art/animals.txt
---------                     -------
      900                     10 files
        0  2018-10-31 17:46   School/Math/
       10  2018-10-31 17:46   School/Math/numbers.txt
---------                     -------
      740                     7 files
```

Extraer los archivos es igual que crear el archivo, ya que la operación predeterminada del comando de descompresión es extraer. Ofrece varias opciones si al descomprimir los archivos se sobrescribirán los existentes:

```
sysadmin@localhost:~/Documents$ unzip School.zip
Archive:  School.zip
replace School/Engineering/hello.sh? [y]es, [n]o, [A]ll, [N]one, [r]ename: n
replace School/Art/linux.txt? [y]es, [n]o, [A]ll, [N]one, [r]ename: n
replace School/Math/numbers.txt? [y]es, [n]o, [A]ll, [N]one, [r]ename: n
replace School/Art/red.txt? [y]es, [n]o, [A]ll, [N]one, [r]ename: n
replace School/Art/hidden.txt? [y]es, [n]o, [A]ll, [N]one, [r]ename: n
replace School/Art/animals.txt? [y]es, [n]o, [A]ll, [N]one, [r]ename: n
```

Esto se puede evitar copiando el archivo zip en un nuevo directorio:

```
sysadmin@localhost:~/Documents$ mkdir tmp
sysadmin@localhost:~/Documents$ cp School.zip tmp/School.zip
sysadmin@localhost:~/Documents$ cd tmp
sysadmin@localhost:~/Documents/tmp$ unzip School.zip
Archive:  School.zip
   creating: School/
   creating: School/Engineering/
  inflating: School/Engineering/hello.sh
   creating: School/Art/
  inflating: School/Art/linux.txt
   creating: School/Math/
 extracting: School/Math/numbers.txt
  inflating: School/Art/red.txt
  inflating: School/Art/hidden.txt
  inflating: School/Art/animals.txt
```

Aquí, extraemos todos los archivos del archivo en el directorio actual. Al igual que tar, puede pasar nombres de archivo en la línea de comandos. Los siguientes ejemplos muestran tres intentos diferentes de extraer un archivo.

En primer lugar, solo se pasa el nombre del archivo sin el componente de directorio. Al igual que tar, el archivo no coincide.

```
sysadmin@localhost:~/Documents/tmp$ unzip School.zip linux.txt
Archive:  School.zip
caution: filename not matched:  linux.txt
```

Un segundo intento pasa el componente de directorio junto con el nombre del archivo, que extrae solo ese archivo.

```
sysadmin@localhost:~/Documents/tmp$ unzip School.zip School/Math/numbers.txt
Archive:  School.zip
 extracting: School/Math/numbers.txt
```

La tercera versión utiliza un comodín, que extrae los cuatro archivos que coinciden con el patrón, al igual que tar.

```
sysadmin@localhost:~/Documents/tmp$ unzip School.zip School/Art/*t
Archive:  School.zip
  inflating: School/Art/linux.txt
  inflating: School/Art/red.txt
  inflating: School/Art/hidden.txt
  inflating: School/Art/animals.txt
```

Las páginas del manual zip y unzip describen las otras cosas que puede hacer con estas herramientas, como reemplazar archivos dentro del archivo, usar diferentes niveles de compresión e incluso usar cifrado.