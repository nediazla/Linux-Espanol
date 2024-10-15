# 18. Directorios y Archivos especiales
## 18.1 Introducción
En la mayoría de los casos, los permisos básicos de Linux (lectura, escritura y ejecución) son suficientes para satisfacer las necesidades de seguridad de usuarios individuales u organizaciones.

Sin embargo, cuando varios usuarios necesitan trabajar de forma rutinaria en los mismos directorios y archivos, estos permisos pueden no ser suficientes. Los permisos especiales setuid, setgid y sticky bit están diseñados para abordar estas preocupaciones.

![](img/20241011102905.png)

## 18.2 Setuid
Cuando el  _permiso setuid_ se establece en un _archivo binario ejecutable_ (un programa), el archivo binario se ejecuta como el propietario del archivo, no como el usuario que lo ejecutó. Este permiso se establece en un puñado de utilidades del sistema para que puedan ser ejecutadas por usuarios normales, pero ejecutadas con los permisos de root, proporcionando acceso a archivos del sistema a los que el usuario normal normalmente no tiene acceso.

Considere el siguiente escenario en el que el usuario sysadmin intenta ver el contenido del archivo /etc/shadow:

```shell
sysadmin@localhost:~$ more /etc/shadow
/etc/shadow: Permission denied
sysadmin@localhost:~$ ls -l /etc/shadow
-rw-r-----. 1 root root 5195 Oct 21 19:57 /etc/shadow
```

Los permisos de /etc/shadow no permiten a los usuarios normales ver (o modificar) el archivo. Dado que el archivo es propiedad del usuario root, el administrador puede modificar temporalmente los permisos para ver o modificar este archivo.

Ahora considere el comando passwd. Cuando se ejecuta este comando, modifica el archivo /etc/shadow, lo que parece imposible porque fallan otros comandos que ejecuta el usuario administrador del sistema que intentan acceder a este archivo. Entonces, ¿por qué el usuario administrador del sistema puede modificar el archivo /etc/shadow mientras ejecuta el comando passwd cuando normalmente este usuario no tiene acceso al archivo?

El comando passwd tiene el conjunto de  permisos _especial setuid_. Cuando se ejecuta el comando passwd y el comando accede al archivo /etc/shadow, el sistema actúa como si el usuario que accede al archivo fuera el propietario del comando passwd (el usuario raíz), no el usuario que está ejecutando el comando.

Puede ver este conjunto de permisos ejecutando el comando ls -l:

```shell
sysadmin@localhost:~$ ls -l /usr/bin/passwd
-rwsr-xr-x 1 root root 31768 Jan 28 2010 /usr/bin/passwd
```

Observe la salida del comando ls anterior; El permiso Setuid está representado por la S en los permisos del propietario, donde normalmente se representaría el permiso de ejecución. Una s minúscula significa que se establecen tanto el permiso setuid como el permiso de ejecución, mientras que una S mayúscula significa que solo se establece setuid y no el permiso de ejecución del usuario.

Al igual que los permisos de lectura, escritura y ejecución, los permisos especiales se pueden establecer con el comando chmod, utilizando los métodos simbólico y octal.

Para agregar el permiso setuid simbólicamente, ejecute:

```shell
chmod u+s file
```

Para agregar el permiso setuid numéricamente, agregue 4000 a los permisos existentes del archivo (suponga que el archivo originalmente tenía 775 para su permiso en el ejemplo siguiente):

```shell
chmod 4775 file
```

Para quitar el permiso setuid simbólicamente, ejecute:

```shell
chmod u-s file
```

Para eliminar numéricamente el permiso setuid, reste 4000 de los permisos existentes del archivo:

```shell
chmod 0775 file
```

Anteriormente, establecíamos el permiso con el método octal utilizando códigos de tres dígitos. Cuando se proporciona un código de tres dígitos, el comando chmod asume que el primer dígito antes del código de tres dígitos es 0. Solo cuando se especifican cuatro dígitos se establece un permiso especial.

Si se especifican tres dígitos al cambiar el permiso en un archivo que ya tiene un conjunto de permisos especiales, el primer dígito se establecerá en 0 y el permiso especial se eliminará del archivo.
## 18.3 Setgid
El  permiso _setgid_ es similar a setuid, pero hace uso de los permisos del propietario del grupo. Hay dos formas de permisos setgid: setgid en un archivo y setgid en un directorio. El comportamiento de setgid depende de si se establece en un archivo o en un directorio.
### 18.3.1 Setgid en archivos
El permiso setgid en un archivo es muy similar a setuid; permite a un usuario ejecutar un archivo binario ejecutable de una manera que le proporciona acceso de grupo adicional (temporal). El sistema permite que el usuario que ejecuta el comando pertenezca efectivamente al grupo propietario del archivo, pero solo _en_ el programa setgid.

Un buen ejemplo del permiso setgid en un archivo ejecutable es el comando /usr/bin/wall. Observe los permisos para este archivo, así como para el propietario del grupo:

```shell
sysadmin@localhost:~$ ls -l /usr/bin/wall
-rwxr-sr-x 1 root tty 30800 May 16  2018 /usr/bin/wall
```

Puede ver que este archivo se establece por la presencia de la s en la posición de ejecución del grupo. Debido a que este ejecutable es propiedad del grupo tty, cuando un usuario ejecuta este comando, el comando puede acceder a los archivos que son propiedad del grupo tty.

Este acceso es importante porque el comando /usr/bin/wall envía mensajes a los terminales, lo que se logra escribiendo datos en archivos como los siguientes:

```shell
sysadmin@localhost:~$ ls -l /dev/tty?
crw--w----. 1 root tty  4, 0 Mar 29  2013 /dev/tty0
crw--w----. 1 root tty  4, 1 Oct 21 19:57 /dev/tty1
```

Tenga en cuenta que el grupo tty tiene permiso de escritura en los archivos anteriores, mientras que los usuarios que no están en el grupo tty ("otros") no tienen permisos en estos archivos. Sin el permiso setgid, el comando /usr/bin/wall fallaría.
### 18.3.2 Setgid en Directorios
Cuando se establece en un directorio, el permiso setgid hace que los archivos creados en el directorio sean propiedad del grupo propietario del directorio automáticamente. Este comportamiento es contrario a cómo funcionaría normalmente la propiedad de un nuevo grupo de archivos, ya que, de forma predeterminada, los nuevos archivos son propiedad del grupo principal del usuario que creó el archivo.

Además, cualquier directorio creado dentro de un directorio con el conjunto de permisos setgid no solo es propiedad del grupo propietario del directorio setgid, sino que el nuevo directorio también tiene setgid establecido automáticamente en él. En otras palabras, si un directorio es setgid, entonces cualquier directorio creado dentro de ese directorio hereda el permiso setgid.

De forma predeterminada, cuando el comando ls se ejecuta en un directorio, genera información sobre los archivos contenidos en el directorio. Para ver información sobre el directorio en sí, agregue la opción -d. Usado con la opción -l, se puede usar para determinar si el permiso setgid está establecido. En el ejemplo siguiente se muestra que el directorio /tmp/data tiene el conjunto de permisos setgid y que es propiedad del grupo de demostración.

```shell
sysadmin@localhost:~$ ls -ld /tmp/data
drwxrwsrwx. 2 root demo 4096 Oct 30 23:20 /tmp/data
```

En una lista larga, el permiso setgid está representado por una s en la posición de ejecución del grupo. Una s minúscula significa que se establecen los permisos de ejecución setgid y group:

```shell
drwxrwsrwx. 2 root demo 4096 Oct 30 23:20 /tmp/data
```

Una S mayúscula significa que solo se establece el permiso setgid y no group extribute. Si ve una S mayúscula en la posición de ejecución del grupo de los permisos, indica que, aunque el permiso setgid esté establecido, no está realmente en efecto porque el grupo carece del permiso de ejecución para usarlo:

```shell
drwxrwSr-x. 2 root root 5036 Oct 30 23:22 /tmp/data2
```

Normalmente, los archivos creados por el usuario sysadmin son propiedad de su grupo principal, también llamado sysadmin.

```shell
sysadmin@localhost:~$ id
uid=500(sysadmin) gid=500(sysadmin)
groups=500(sysadmin),10001(research),10002(development) 
context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```

> La información de UID y GID en el ejemplo anterior puede diferir de la salida en nuestro entorno virtual.

Sin embargo, si el usuario sysadmin crea un archivo en el directorio /tmp/data, el directorio setgid del ejemplo anterior, la propiedad del grupo del archivo no es el grupo sysadmin, sino el grupo que posee el directorio demo:

```shell
sysadmin@localhost:~$  touch /tmp/data/file.txt
sysadmin@localhost:~$  ls -ld /tmp/data/file.txt
-rw-rw-r--. 1 bob demo 0 Oct 30 23:21 /tmp/data/file.txt
```

¿Por qué querría un administrador configurar un directorio setgid? En primer lugar, considere las siguientes cuentas de usuario:

- El usuario bob es miembro del grupo de nómina.
- El usuario demandar es miembro del grupo de personal.
- El usuario tim es miembro del grupo acct.

En este escenario, estos tres usuarios deben trabajar en un proyecto conjunto. Se acercan al administrador para pedirle un directorio compartido en el que puedan trabajar juntos, pero que nadie más pueda acceder a sus archivos. El administrador hace lo siguiente:

1. Crea un nuevo grupo llamado equipo.
2. Agrega a Bob, Sue y Tim al grupo de equipo.
3. Crea un nuevo directorio llamado /home/team.
4. Hace que el propietario del grupo del directorio /home/team sea el grupo de equipo.
5. Otorga al directorio /home/team los siguientes permisos: rwxrwx---

Como resultado, bob, sue y tim pueden acceder al directorio /home/team y agregar archivos. Sin embargo, existe un problema potencial: cuando bob crea un archivo en el directorio /home/team, el nuevo archivo es propiedad de su grupo principal:

![](img/20241015190856.png)

Desafortunadamente, aunque sue y tim pueden acceder al directorio /home/team, no pueden hacer nada con el archivo de bob. Sus permisos para ese archivo son los otros permisos (---).

Si el administrador establece el permiso setgid en el directorio /home/team, entonces cuando bob crea un archivo, es propiedad del grupo de equipo:

![](img/20241015190910.png)

Como resultado, sue y tim tendrían acceso al archivo a través de los permisos del propietario del grupo (r--).

Ciertamente, bob podría cambiar la propiedad del grupo o los permisos de los demás después de crear el archivo, pero eso se volvería tedioso si se crearan muchos archivos nuevos. El permiso setgid facilita esta situación.
### 18.3.3 Configuración de Setgid
Utilice la siguiente sintaxis para agregar simbólicamente el permiso setgid:

```
chmod g+s <file|directory>
```

Para agregar el permiso setgid numéricamente, agregue 2000 a los permisos existentes del archivo (suponga en el siguiente ejemplo que el directorio originalmente tenía 775 para sus permisos):

```shell
chmod 2775 <file|directory>
```

Para eliminar simbólicamente el permiso setgid, ejecute:

```shell
chmod g-s <file|directory>
```

Para eliminar el permiso setgid numéricamente, reste 2000 de los permisos existentes del archivo:

```shell
chmod 0775 <file|directory>
```

## 18.4 Sticky Bit
El  permiso _de bits pegajosos_ se utiliza para evitar que otros usuarios eliminen archivos que no son de su propiedad en un directorio compartido. Recuerde que cualquier usuario con permiso de escritura en un directorio puede crear archivos en ese directorio, así como eliminar cualquier archivo en el directorio, ¡incluso si no es el propietario del archivo!

El permiso de bits pegajosos permite que los archivos se compartan con otros usuarios, cambiando el permiso de escritura en el directorio para que los usuarios aún puedan agregar y eliminar archivos en el directorio, pero los archivos solo pueden ser eliminados por el propietario del archivo o el usuario raíz.

Un buen ejemplo del uso de directorios de bits pegajosos serían los directorios /tmp y /var/tmp. Estos directorios están diseñados como ubicaciones donde cualquier usuario puede crear un archivo temporal.

Dado que estos directorios están pensados para que todos los usuarios puedan escribir, están configurados para usar el bit pegajoso. Sin este permiso especial, los usuarios podrían eliminar cualquier archivo de este directorio, incluidos los que pertenecen a otros usuarios.

La salida del comando ls -l muestra el bit pegajoso con un carácter t en el bit execute del grupo de permisos others:

```shell
sysadmin@localhost:~$ ls -ld /tmp                                               
drwxrwxrwt 1 root root 4096 Mar 14  2016 /tmp    
```

Una t minúscula significa que tanto el bit pegajoso como los permisos de ejecución están establecidos para otros. Una T mayúscula significa que solo se establece el permiso de sticky bit.

Mientras que la S mayúscula indica un problema con los permisos setuid o setgid, una T mayúscula no indica necesariamente un problema, siempre y cuando el propietario del grupo siga teniendo el permiso de ejecución.

Para establecer simbólicamente el permiso de bit pegajoso, ejecute un comando como el siguiente:

```shell
chmod o+t <directory>
```

Para establecer el permiso de bit persistente numéricamente, agregue 1000 a los permisos existentes del directorio (suponga que el directorio del ejemplo siguiente originalmente tenía 775 para sus permisos):

```shell
chmod 1775 <file|directory>
```

Para eliminar simbólicamente el permiso permanente, ejecute:

```shell
chmod o-t <directory>
```

Para quitar numéricamente el permiso de bit pegajoso, reste 1000 de los permisos existentes del directorio:

```shell
chmod 0775 <directory>
```

## 18.5 Enlaces
Considere un escenario en el que hay un archivo profundamente enterrado en el sistema de archivos llamado:

```shell
/usr/share/doc/superbigsoftwarepackage/data/2013/october/tenth/valuable-information.txt
```

Otro usuario actualiza este archivo de forma rutinaria y debe acceder a él con regularidad. El nombre de archivo largo no es una opción ideal para escribir, pero el archivo debe residir en esta ubicación. También se actualiza con frecuencia, por lo que no puede simplemente hacer una copia del archivo.

En una situación como esta, los enlaces son útiles. Puede crear un archivo que esté vinculado al que está profundamente enterrado. Este nuevo archivo se puede colocar en el directorio de inicio o en cualquier otra ubicación conveniente. Cuando se accede al archivo vinculado, se accede al contenido del archivo valuable-information.txt.

Cada método de vinculación, duro y simbólico, da como resultado el mismo acceso general, pero utiliza técnicas diferentes. Cada método tiene sus pros y sus contras, por lo que es importante conocer ambas técnicas y cuándo utilizarlas.
### 18.5.1 Creación de enlaces físicos
Para comprender los _enlaces físicos_, es útil comprender un poco sobre cómo el sistema de archivos realiza un seguimiento de los archivos. Por cada archivo creado, hay un bloque de datos en el sistema de archivos que almacena los _metadatos_ del archivo. Los metadatos incluyen información sobre el archivo, como los permisos, la propiedad y las marcas de tiempo. Los metadatos no incluyen el nombre del archivo ni el contenido del archivo, pero sí incluyen casi toda la demás información sobre el archivo.

Estos metadatos se denominan _tabla de inodos del archivo_. La tabla de inodos también incluye punteros a los otros bloques del sistema de archivos llamados _bloques de datos_ donde se almacenan los datos.

Cada archivo de una partición tiene un número de identificación único llamado _número de inodo_. El comando ls -i muestra el número de inodo de un archivo.

```shell
sysadmin@localhost:~$ ls -i /tmp/file.txt                                       
215220874 /tmp/file.txt  
```

Al igual que los usuarios y los grupos, lo que define a un archivo no es su nombre, sino el número que se le ha asignado. La tabla de inodos no incluye el nombre del archivo. Para cada archivo, también hay una entrada que se almacena en el área de datos de un directorio (bloque de datos) que incluye una asociación entre un número de inodo y un nombre de archivo.

En el bloque de datos para el directorio /etc, habría una lista de todos los archivos en este directorio y su número de inodo correspondiente. Por ejemplo:

|**Nombre de archivo**|**Número de inodo**|
|---|---|
|passwd|123|
|sombra|175|
|grupo|144|
|gsombra|897|

Cuando intenta acceder al archivo /etc/passwd, el sistema utiliza esta tabla para traducir el nombre del archivo a un número de inodo. A continuación, recupera los datos del archivo examinando la información de la tabla de inodos del archivo.

Los enlaces físicos son dos nombres de archivo que apuntan al mismo inodo. Por ejemplo, considere las siguientes entradas de directorio:

|**Nombre de archivo**|**Número de inodo**|
|---|---|
|passwd|123|
|mypasswd|123|
|sombra|175|
|grupo|144|
|gsombra|897|

Debido a que tanto el archivo passwd como mypasswd tienen el mismo número de inodo, esencialmente son el mismo archivo. Puede acceder a los datos del archivo utilizando cualquiera de los nombres de archivo.

Al ejecutar el comando ls -li, el número que aparece para cada archivo entre los permisos y el propietario del usuario es el número de recuento de vínculos:

```
sysadmin@localhost:~$ echo data > file.original 
sysadmin@localhost:~$ ls -li file.* 
278772 -rw-rw-r--. 1 sysadmin sysadmin 5 Oct 25 15:42 file.original
```

El número de recuento de enlaces indica cuántos enlaces físicos se han creado. Cuando el número es un valor de uno, el archivo solo tiene un nombre vinculado al inodo.

Para crear enlaces físicos, se utiliza el comando ln con dos argumentos. El primer argumento es un nombre de archivo existente al que vincular, denominado destino, y el segundo argumento es el nuevo nombre de archivo que se va a vincular al destino.

```
ln target link_name
```

Cuando se utiliza el comando ln para crear un enlace físico, el número de recuento de enlaces aumenta en uno por cada nombre de archivo adicional:

```
sysadmin@localhost:~$ ln file.original file.hard.1
sysadmin@localhost:~$ ls -li file.*
278772 -rw-rw-r--. 2 sysadmin sysadmin 5 Oct 25 15:53 file.hard.1
278772 -rw-rw-r--. 2 sysadmin sysadmin 5 Oct 25 15:53 file.original
```
### 18.5.2 Creación de enlaces simbólicos
Un _enlace simbólico_, también llamado _enlace suave_, es simplemente un archivo que apunta a otro archivo. Ya hay varios enlaces simbólicos en el sistema, incluyendo varios en el directorio /etc:

```
sysadmin@localhost:~$ ls -l /etc/grub.conf
lrwxrwxrwx. 1 root root 22 Feb 15  2011 /etc/grub.conf -> ../boot/grub/grub.conf
```

En el ejemplo anterior, el fichero /etc/grub.conf "apunta" al archivo .. /boot/grub/grub.conf. Por lo tanto, si intentara ver el contenido del archivo /etc/grub.conf, seguiría el puntero y le mostraría el contenido del archivo .. /boot/grub/grub.conf.

Para crear un enlace simbólico, use la opción -s con el comando ln:

```
ln -s target link_name
```

```
sysadmin@localhost:~$  ln -s /etc/passwd mypasswd
sysadmin@localhost:~$  ls -l mypasswd
lrwxrwxrwx. 1 sysadmin sysadmin 11 Oct 31 13:17 mypasswd -> /etc/passwd
```

> Observe que el primer bit de la salida ls -l es el carácter l, lo que indica que el tipo de archivo es un enlace.
### 18.5.3 Comparación de enlaces físicos y simbólicos
Si bien los enlaces duros y simbólicos tienen el mismo resultado, las diferentes técnicas producen diferentes ventajas y desventajas. De hecho, la ventaja de una técnica compensa la desventaja de la otra técnica.

**Ventaja: Los enlaces duros no tienen un solo punto de fallo.**

Una de las ventajas de utilizar enlaces físicos es que todos los nombres de archivo para el contenido del archivo son equivalentes. Si tiene cinco archivos vinculados entre sí, la eliminación de cuatro de estos archivos no dará como resultado la eliminación del contenido real del archivo.

Recuerde que un archivo está asociado a un número de inodo único. Mientras uno de los archivos vinculados de forma rígida permanezca, ese número de inodo seguirá existiendo y los datos del archivo seguirán existiendo.

Los enlaces simbólicos, sin embargo, tienen un único punto de error: el archivo original. Considere el siguiente ejemplo en el que se produce un error en el acceso a los datos si se elimina el archivo original. El archivo mytest.txt es un enlace simbólico al archivo text.txt:

```
sysadmin@localhost:~$ ls -l mytest.txt
lrwxrwxrwx. 1 sysadmin sysadmin 8 Oct 31 13:29 mytest.txt -> test.txt
sysadmin@localhost:~$ more test.txt
hi there
sysadmin@localhost:~$ more mytest.txt
hi there
```

Si se elimina el archivo original, el archivo test.txt, se produce un error en los archivos vinculados a él, incluido el archivo mytest.txt:

```
sysadmin@localhost:~$ rm test.txt
sysadmin@localhost:~$ more mytest.txt
mytest.txt: No such file or directory
sysadmin@localhost:~$ ls -l mytest.txt
lrwxrwxrwx. 1 sysadmin sysadmin 8 Oct 31 13:29 mytest.txt -> test.txt
```

**Ventaja: Los enlaces blandos son más fáciles de ver.**

A veces puede ser difícil saber dónde están los enlaces físicos a un archivo. Si ve un archivo normal con un número de enlaces mayor que uno, puede utilizar el comando find con los criterios de búsqueda -inum para localizar los demás archivos que tienen el mismo número de inodo. Para encontrar el número de inodo, primero debe usar el comando ls -i:

```
sysadmin@localhost:~$ ls -i file.original 
278772 file.original
sysadmin@localhost:~$ find / -inum 278772 2> /dev/null
/home/sysadmin/file.hard.1
/home/sysadmin/file.original
```

Los enlaces suaves son mucho más visuales, no requieren ningún comando adicional más allá del comando ls para determinar el enlace:

```
sysadmin@localhost:~$ ls -l mypasswd
lrwxrwxrwx. 1 sysadmin sysadmin 11 Oct 31 13:17 mypasswd -> /etc/passwd
```

**Ventaja: Los enlaces suaves pueden vincular a cualquier archivo.**

Dado que cada sistema de archivos (partición) tiene un conjunto separado de inodos, no se pueden crear enlaces físicos que intenten cruzar sistemas de archivos:

```
sysadmin@localhost:~$ ln /boot/vmlinuz-2.6.32-358.6.1.el6.i686 Linux.Kernel
ln: creating hard link `Linux.Kernel' => `/boot/vmlinuz-2.6.32-358.6.1.el6.i686': Invalid cross-device link
```

En el ejemplo anterior, se intentó crear un vínculo físico entre un archivo en el sistema de archivos /boot y el sistema de archivos /; Se produjo un error porque cada uno de estos sistemas de archivos tiene un conjunto único de números de inodo que no se pueden utilizar fuera del sistema de archivos.

Sin embargo, debido a que un enlace simbólico apunta a otro archivo usando un nombre de ruta, puede crear un enlace suave a un archivo en otro sistema de archivos:

```
sysadmin@localhost:~$ ln -s /boot/vmlinuz-2.6.32-358.6.1.el6.i686 Linux.Kernel
sysadmin@localhost:~$ ls -l Linux.Kernel
lrwxrwxrwx. 1 sysadmin sysadmin 11 Oct 31 13:17 Linux.Kernel -> /boot/vmlinuz-2.6.32-358.6.1.el6.i686
```

**Ventaja: Los enlaces suaves pueden enlazar a un directorio.**

Otra limitación de los enlaces duros es que no se pueden crear en directorios. La razón de esta limitación es que el propio sistema operativo utiliza enlaces físicos para definir la jerarquía de la estructura de directorios. En el ejemplo siguiente se muestra el mensaje de error que se muestra si se intenta establecer un vínculo físico a un directorio:

```
sysadmin@localhost:~$ ln /bin binary
ln: `/bin': hard link not allowed for directory
```

Se permite enlazar a directorios mediante un enlace simbólico:

```
sysadmin@localhost:~$ ln -s /bin binary
sysadmin@localhost:~$ ls -l binary
lrwxrwxrwx. 1 sysadmin sysadmin 11 Oct 31 13:17 binary -> /bin
```