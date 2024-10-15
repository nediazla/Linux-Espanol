# 8. Administrando Archivos y Directorios
## 8.1 Introducción

Cuando se trabaja en un sistema operativo Linux, es importante saber cómo manipular archivos y directorios. Algunas distribuciones de Linux tienen aplicaciones basadas en GUI que le permiten administrar archivos, pero es ventajoso saber cómo realizar estas operaciones a través de la línea de comandos.

> Es importante tener en cuenta que todo en Linux distingue entre mayúsculas y minúsculas, una característica heredada de Unix. Esto significa que el shell reconoce un carácter minúsculo de la A a la Z como completamente diferente de un carácter de la A a la Z mayúscula. Al manipular archivos, preste atención a las mayúsculas: un archivo hello.txt es diferente de los archivos HELLO.txt y Hello.txt.

> El estándar de caracteres utilizado para Linux es el estándar UTF-8 que se basa en el ASCII (Código Estándar Americano para el Intercambio de Información). Para obtener más información sobre ASCII, ejecute el comando ascii man -s 7.
## 8.2 Globbing
_Los caracteres glob_ a menudo se denominan comodines. Estos son caracteres de símbolo que tienen un significado especial para el shell.

Los globs son eficaces porque permiten especificar patrones que coinciden con los nombres de archivo de un directorio. Por lo tanto, en lugar de manipular un solo archivo a la vez, puede ejecutar fácilmente comandos que afectan a muchos archivos. Por ejemplo, mediante el uso de caracteres glob, es posible manipular todos los archivos con una extensión específica o con una longitud de nombre de archivo particular.

A diferencia de los comandos que ejecuta el shell, o las opciones y argumentos que el shell pasa a los comandos, los caracteres glob son interpretados por el propio shell antes de intentar ejecutar cualquier comando. Como resultado, los caracteres glob se pueden usar con _cualquier_ comando.

Los ejemplos proporcionados en este capítulo utilizan el comando echo para la demostración.
### 8.2.1 Asterisco * Carácter
El asterisco * se utiliza para representar cero o más de cualquier carácter en un nombre de archivo. Por ejemplo, para mostrar todos los archivos del directorio /etc que comiencen por la letra t:

```shell
sysadmin@localhost:~$ echo /etc/t*                              
/etc/terminfo /etc/timezone /etc/tmpfiles.d
```

El patrón t* coincide con cualquier archivo en el directorio /etc que comience con el carácter t seguido de cero o más de cualquier carácter. En otras palabras, cualquier archivo que comience con la letra t.

Puede utilizar el carácter de asterisco en cualquier lugar dentro del patrón de nombre de archivo. Por ejemplo, lo siguiente coincide con cualquier nombre de archivo en el directorio /etc que termine en .d:

```shell
sysadmin@localhost:~$ echo /etc/*.d                                 
/etc/apparmor.d /etc/binfmt.d /etc/cron.d /etc/depmod.d /etc/init.d /etc/insserv
.conf.d /etc/ld.so.conf.d /etc/logrotate.d /etc/modprobe.d /etc/modules-load.d /
etc/pam.d /etc/profile.d /etc/rc0.d /etc/rc1.d /etc/rc2.d /etc/rc3.d /etc/rc4.d 
/etc/rc5.d /etc/rc6.d /etc/rcS.d /etc/rsyslog.d /etc/sudoers.d /etc/sysctl.d /et
c/tmpfiles.d /etc/update-motd.d
```

En el siguiente ejemplo, se muestran todos los archivos del directorio /etc que comienzan con la letra r y terminan con .conf:

```shell
sysadmin@localhost:~$ echo /etc/r*.conf                             
/etc/resolv.conf /etc/rsyslog.conf
```
### 8.2.2 ¿Signo de interrogación? Carácter
¿El signo de interrogación? character representa cualquier carácter individual. Cada carácter de signo de interrogación coincide exactamente con un carácter, ni más ni menos.

Supongamos que desea mostrar todos los archivos en el directorio /etc que comienzan con la letra t y tienen exactamente 7 caracteres después del carácter t:

```shell
sysadmin@localhost:~$ echo /etc/t???????      
/etc/terminfo /etc/timezone
```

Los caracteres globales se pueden usar juntos para encontrar patrones aún más complejos. El patrón /etc/*???????????????????? solo coincide con los archivos en el directorio /etc con veinte o más caracteres en el nombre del archivo:

```shell
sysadmin@localhost:~$ echo /etc/*????????????????????            
/etc/bindresvport.blacklist /etc/ca-certificates.conf
```

El asterisco y el signo de interrogación también se pueden usar juntos para buscar archivos con extensiones de tres letras mediante el /etc/*.??? patrón:

```shell
sysadmin@localhost:~$ echo /etc/*.???                
/etc/issue.net /etc/locale.gen
```
### 8.2.3 Corchetes [ ] Caracteres
Los caracteres de corchete [] se utilizan para hacer coincidir un solo carácter representando un rango de caracteres que son posibles caracteres de coincidencia. Por ejemplo, el patrón /etc/[gu]* coincide con cualquier archivo que comience con un carácter g o u y contenga cero o más caracteres adicionales:

```shell
sysadmin@localhost:~$ echo /etc/[gu]*                              
/etc/gai.conf /etc/groff /etc/group /etc/group- /etc/gshadow /etc/gshadow- /etc/
gss /etc/ucf.conf /etc/udev /etc/ufw /etc/update-motd.d /etc/updatedb.conf  
```

Los corchetes también se pueden utilizar para representar un rango de caracteres. Por ejemplo, el patrón /etc/[a-d]* coincide con todos los archivos que comienzan con cualquier letra entre y a y d inclusive:

```shell
sysadmin@localhost:~$ echo /etc/[a-d]*
/etc/adduser.conf /etc/alternatives /etc/apparmor /etc/apparmor.d /etc/apt /etc/
bash.bashrc /etc/bind /etc/bindresvport.blacklist /etc/binfmt.d /etc/ca-certific
ates /etc/ca-certificates.conf /etc/calendar /etc/console-setup /etc/cron.d /etc
/cron.daily /etc/cron.hourly /etc/cron.monthly /etc/cron.weekly /etc/crontab /et
c/dbus-1 /etc/debconf.conf /etc/debian_version /etc/default /etc/deluser.conf /e
tc/depmod.d /etc/dhcp /etc/dpkg 
```

El patrón /etc/*[0-9]* muestra cualquier archivo que contenga al menos un número:

El rango se basa en la tabla de  **texto ASCII**. Esta tabla define una lista de caracteres, organizándolos en un orden estándar específico. Si se proporciona un pedido no válido, no se realizarán coincidencias:

```shell
sysadmin@localhost:~$ echo /etc/*[0-9]*                            
/etc/X11 /etc/dbus-1 /etc/iproute2 /etc/mke2fs.conf /etc/python3 /etc/python3.6 
/etc/rc0.d /etc/rc1.d /etc/rc2.d /etc/rc3.d /etc/rc4.d /etc/rc5.d /etc/rc6.d  
```

> La tabla de texto ASCII se puede visualizar en nuestras máquinas virtuales ejecutando el comando ascii.
### 8.2.4 ¡Signo de exclamación! Carácter
El signo de exclamación ! se utiliza junto con los corchetes para negar un intervalo. Por ejemplo, el patrón /etc/[! DP]* coincide con cualquier archivo que _no_ comience con D o _P_.

```shell
sysadmin@localhost:~$ echo /etc/[!a-t]*
/etc/X11 /etc/ucf.conf /etc/udev /etc/ufw /etc/update-motd.d /etc/updatedb.conf 
/etc/vim /etc/vtrgb /etc/wgetrc /etc/xdg
```
### 8.2.5 Listado con Globs
El comando ls se utiliza normalmente para listar archivos en un directorio; Como resultado, usar el comando echo puede parecer una elección extraña. Sin embargo, hay algo en el comando ls que causa problemas al enumerar archivos usando patrones glob.

Tenga en cuenta que es el shell, no el comando echo o ls, el que expande el patrón glob en los nombres de archivo correspondientes. En otras palabras, si se ejecuta el comando echo /etc/a*, lo que hizo el shell _antes_ de ejecutar el comando echo fue reemplazar a* con todos los archivos y directorios dentro del directorio /etc que coincidan con el patrón.

Por lo tanto, si se ejecuta el comando ls /etc/a*, lo que realmente ejecutaría el shell sería lo siguiente:

```shell
ls /etc/adduser.conf  /etc/alternatives  /etc/apparmor  /etc/apparmor.d  /etc/apt
```

Cuando el comando ls ve varios argumentos, realiza una operación de lista en cada elemento por separado. En otras palabras, ls /etc/a* es lo mismo que ejecutar los siguientes comandos consecutivamente:

```bash
ls /etc/adduser.conf  
ls /etc/alternatives  
ls /etc/apparmor  
ls /etc/apparmor.d  
ls /etc/apt
```

Ahora considere lo que sucede si al comando ls se le pasa un archivo, como /etc/adduser.conf:

```shell
sysadmin@localhost:~$ ls /etc/adduser.conf
/etc/adduser.conf
```

La ejecución del comando ls en un solo archivo da como resultado el nombre del archivo que se imprime; Normalmente, esto es útil si se utiliza la opción -l para ver detalles sobre un archivo específico:

```
sysadmin@localhost:~$ ls -l /etc/adduser.conf                                   
-rw-r--r-- 1 root root 3028 May 26  2018 /etc/adduser.conf
```

Sin embargo, ¿qué pasa si al comando ls se le da un nombre de directorio como argumento? En este caso, la salida del comando es diferente que si el argumento fuera un archivo normal:

```shell
sysadmin@localhost:~$ ls /etc/apparmor                                          
init  parser.conf  subdomain.conf 
```

Si al comando ls se le asigna un nombre de directorio, el comando muestra el _contenido_ del directorio (los nombres de los archivos en el directorio), no solo el nombre del directorio. Los nombres de archivo en el ejemplo anterior son los nombres de los archivos en el directorio /etc/apparmor.

¿Por qué es esto un problema cuando se usan globs? Considere el siguiente resultado:

```sehll
sysadmin@localhost:~$ ls /etc/ap*                                               
/etc/apparmor:                                                                  
init  parser.conf  subdomain.conf                                               
                                                                                
/etc/apparmor.d:                                                                
abstractions  disable         local          tunables     usr.sbin.named        
cache         force-complain  sbin.dhclient  usr.bin.man  usr.sbin.rsyslogd     
                                                                                
/etc/apt:                                                                       
apt.conf.d  preferences.d  sources.list  sources.list.d  trusted.gpg.d 
```

Cuando el comando ls ve un nombre de archivo como argumento, solo muestra el nombre de archivo. Sin embargo, para cualquier directorio, muestra el contenido del directorio, no solo el nombre del directorio.

Esto se vuelve aún más confuso en una situación como la siguiente:

```shell
 sysadmin@localhost:~$ ls /etc/x*                                                
autostart  systemd  user-dirs.conf  user-dirs.defaults 
```

En el ejemplo anterior, parece que el comando ls es simplemente incorrecto. Sin embargo, lo que realmente sucedió es que lo único que coincide con el glob /etc/x* es el directorio /etc/xdg.

Por lo tanto, el comando ls solo mostraba los archivos en ese directorio.

Hay una solución simple a este problema: siempre use la opción -d con globs, que le dice al comando ls que muestre el nombre de los directorios, en lugar de su contenido:

```shell
sysadmin@localhost:~$ls -d /etc/x*                                             
/etc/xdg
```
## 8.3 Copia de archivos
El comando cp se utiliza para copiar archivos. Requiere un origen y un destino. La estructura del comando es la siguiente:

```shell
cp source destination
```

El _origen_ es el archivo que se va a copiar. El _destino_ es donde se va a ubicar la copia. Cuando tiene éxito, el comando cp no tiene ninguna salida (ninguna noticia es una buena noticia). El siguiente comando copia el archivo /etc/hosts en su directorio de inicio:

```shell
sysadmin@localhost:~$ cp /etc/hosts ~                                     
sysadmin@localhost:~$ ls
Desktop    Downloads  Pictures  Templates  hosts                          
Documents  Music      Public    Videos 
```
**Recordatorio:** El carácter ~ representa su directorio de inicio.
### 8.3.1 Modo detallado
La opción -v hace que el comando cp produzca una salida si tiene éxito. La opción -v significa _verbose_:
```shell
sysadmin@localhost:~$ cp -v /etc/hosts ~                              
`/etc/hosts' -> `/home/sysadmin/hosts'
```
Cuando el destino es un directorio, el nuevo archivo resultante mantiene el mismo nombre que el archivo original. Para dar un nombre diferente al nuevo archivo, proporcione el nuevo nombre como parte del destino:

```shell
sysadmin@localhost:~$ cp /etc/hosts ~/hosts.copy                      
sysadmin@localhost:~$ ls                                               
Desktop    Downloads  Pictures  Templates  hosts                       
Documents  Music      Public    Videos     hosts.copy    
```
### 8.3.2 Evite sobrescribir datos
El comando cp puede ser destructivo para los datos existentes si el archivo de destino ya existe. En el caso de que exista el archivo de destino, el comando cp sobrescribe el contenido del archivo existente con el contenido del archivo fuente.

Para ilustrar este problema potencial, primero se crea un nuevo archivo en el directorio de inicio copiando un archivo existente:

```shell
sysadmin@localhost:~$ cp /etc/hostname example.txt
```

Vea la información sobre el archivo con el comando ls:

```shell
sysadmin@localhost:~$ ls -l example.txt                                         
-rw-r--r-- 1 sysadmin sysadmin 10 Dec 15 22:55 example.txt
```

Vea el contenido del archivo usando el comando cat:

```
sysadmin@localhost:~$ cat example.txt                                           
localhost  
```

En el siguiente ejemplo, el comando cp destruye el contenido original del archivo example.txt:

```shell
sysadmin@localhost:~$ cp /etc/timezone example.txt
```

Tenga en cuenta que una vez completado el comando cp, el tamaño del archivo ha cambiado y el contenido es diferente:

```sehll
sysadmin@localhost:~$ ls -l example.txt                                         
-rw-r--r-- 1 sysadmin sysadmin 8 Dec 15 22:58 example.txt
sysadmin@localhost:~$ cat example.txt
Etc/UTC
```

Se pueden utilizar dos opciones para protegerse contra sobrescrituras accidentales. Con la opción interactiva -i  , el comando cp avisa al usuario antes de sobrescribir un archivo. En el ejemplo siguiente se muestra esta opción, restaurando primero el contenido del archivo original:

```shell
sysadmin@localhost:~$ cp -i /etc/hosts example.txt                   
cp: overwrite `/home/sysadmin/example.txt'? n   
```

Si se diera un valor de y (sí), entonces el proceso de copia habría tenido lugar. Sin embargo, se proporcionó el valor de n (no) cuando se le pidió que sobrescribiera el archivo, por lo que no se realizaron cambios en el archivo.

La opción -i requiere que responda y o n para cada copia que podría terminar sobrescribiendo el contenido de un archivo existente. Esto puede ser tedioso cuando se producen un montón de sobrescrituras, como en el ejemplo que se muestra a continuación:

```shell
sysadmin@localhost:~$ cp -i /etc/skel/.* ~                             
cp: -r not specified; omitting directory '/etc/skel/.'                          
cp: -r not specified; omitting directory '/etc/skel/..'                                   
cp: overwrite `/home/sysadmin/.bash_logout'? n                         
cp: overwrite `/home/sysadmin/.bashrc'? n                              
cp: overwrite `/home/sysadmin/.profile'? n                            
cp: overwrite `/home/sysadmin/.selected_editor'? n
```

Como puede ver en el ejemplo anterior, el comando cp intentó sobrescribir cuatro archivos existentes, lo que obligó al usuario a responder cuatro solicitudes. Si esta situación ocurriera para 100 archivos, podría volverse muy molesta, muy rápidamente.

Para responder a n a cada pregunta automáticamente, utilice la opción -n. Significa _que no hay clobber_, o _que no se sobrescribe_.

```shell
sysadmin@localhost:~$ cp -n /etc/skel/.* ~                                      
cp: -r not specified; omitting directory '/etc/skel/.'                          
cp: -r not specified; omitting directory '/etc/skel/..'
```
### 8.3.3 Copiar directorios
De forma predeterminada, el comando cp no copiará directorios; Cualquier intento de hacerlo da como resultado un mensaje de error:

```shell
sysadmin@localhost:~$ cp -n /etc/skel/.* ~                                      
cp: -r not specified; omitting directory '/etc/skel/.'                          
cp: -r not specified; omitting directory '/etc/skel/..'    
```

Sin embargo, la  _opción recursiva_ -r permite que el comando cp copie tanto archivos como directorios.

```
cp -r source_directory destination_directory
```
Ten cuidado con esta opción. Se copiará toda la estructura de directorios, lo que podría resultar en la copia de una gran cantidad de archivos y directorios.

> Las opciones -r y -R tienen el mismo propósito. Sin embargo, -R se puede usar con la mayoría de los comandos, mientras que -r puede tener diferentes significados con algunos comandos. Por ejemplo, mientras que cp -r significa _copiar de forma recursiva_ (tanto archivos como directorios), ls -r significa _ordenación inversa_.
## 8.4 Mover archivos
Para mover un archivo, utilice el comando mv. La sintaxis del comando mv es muy parecida a la del comando cp:

```shel
mv source destination
```
En el siguiente ejemplo, el archivo hosts que se generó anteriormente se mueve del directorio actual al directorio Videos:

```shell
sysadmin@localhost:~$ ls                                               
Desktop    Downloads  Pictures  Templates  example.txt  hosts.copy     
Documents  Music      Public    Videos     hosts                       
sysadmin@localhost:~$ mv hosts Videos                                  
sysadmin@localhost:~$ ls                                               
Desktop    Downloads  Pictures  Templates  example.txt                 
Documents  Music      Public    Videos     hosts.copy                 
sysadmin@localhost:~$ ls Videos                                        
hosts
```

Cuando se mueve un archivo, el archivo se elimina de la ubicación original y se coloca en una nueva ubicación. Mover archivos puede ser algo complicado en Linux porque los usuarios necesitan permisos específicos para eliminar archivos de un directorio. Sin los permisos adecuados, se devuelve un mensaje de error Permiso denegado:

```shell
sysadmin@localhost:~$ mv /etc/hosts .
mv: cannot move `/etc/hosts' to `./hosts': Permission denied
```

> Los permisos se tratarán en detalle más adelante en el curso.
### 8.4.1 Cambiar el nombre de los archivos
El comando mv no solo se usa para mover un archivo, sino también para cambiar el nombre de un archivo. Si el destino del comando mv es un directorio, el archivo se mueve al directorio especificado. El nombre del archivo solo cambia si también se especifica un nombre de archivo de destino.

```shell
sysadmin@localhost:~$ ls                                               
Desktop    Downloads  Pictures  Templates  example.txt                          
Documents  Music      Public    Videos     hosts.copy                 
sysadmin@localhost:~$ mv example.txt Videos/newexample.txt             
sysadmin@localhost:~$ ls
Desktop    Downloads  Pictures  Templates  hosts.copy                           
Documents  Music      Public    Videos                               
sysadmin@localhost:~$ ls Videos                                       
hosts  newexample.txt  
```

Si no se especifica un directorio de destino, se cambia el nombre del archivo con el nombre del archivo de destino y permanece en el directorio de origen. Por ejemplo, los siguientes comandos cambian el nombre del archivo newexample.txt a myfile.txt:

```shell
sysadmin@localhost:~$ cd Videos                                        
sysadmin@localhost:~/Videos$ ls                                        
hosts  newexample.txt                                                  
sysadmin@localhost:~/Videos$ mv newexample.txt myfile.txt           
sysadmin@localhost:~/Videos$ ls
hosts  myfile.txt
```
Piense en el ejemplo anterior de mv para significar _mover el archivo newexample.txt del directorio actual al directorio actual y darle al nuevo archivo el nombre myfile.txt_.
### 8.4.2 Opciones de movimiento adicionales
Al igual que el comando cp, el comando mv proporciona las siguientes opciones:

| **Opción** | **Significado**                                                      |
| ---------- | -------------------------------------------------------------------- |
| -i         | _Interactivo_: Pregunta si se va a sobrescribir un archivo.          |
| -n         | _Sin Clobber_: No sobrescriba el contenido de un archivo de destino. |
| -v         | _Detallado_: muestra el movimiento resultante.                       |

> No hay opción -r ya que el comando mv mueve directorios de forma predeterminada.
## 8.5 Creación de archivos
Hay varias formas de crear un nuevo archivo, incluido el uso de un programa diseñado para editar un archivo (un editor de texto).

También hay una manera de crear un archivo vacío que se puede rellenar con datos en un momento posterior. Esta característica es útil para algunos sistemas operativos, ya que la mera existencia de un archivo podría alterar el funcionamiento de un comando o servicio. También es útil crear un archivo como un "marcador de posición" para recordarle que debe crear el contenido del archivo en un momento posterior.

Para crear un archivo vacío, utilice el comando táctil como se muestra a continuación:

```shell
sysadmin@localhost:~$ touch sample                                     
sysadmin@localhost:~$ ls -l sample                                     
-rw-rw-r-- 1 sysadmin sysadmin 0 Nov  9 16:48 sample
```

Observe que el tamaño del nuevo archivo es de 0 bytes. Como se mencionó anteriormente, el comando táctil no coloca ningún dato dentro del nuevo archivo.

_Los editores de texto se tratarán en detalle más adelante en el curso._
## 8.6 Eliminación de archivos
Para eliminar un archivo, utilice el comando rm:

```shell
sysadmin@localhost:~$ ls                                               
Desktop    Downloads  Pictures  Templates  hosts.copy                           
Documents  Music      Public    Videos     sample                         
sysadmin@localhost:~$ rm sample    
sysadmin@localhost:~$ rm hosts.copy                                    
sysadmin@localhost:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos 
```

Tenga en cuenta que los archivos se eliminaron sin hacer preguntas. Esto podría causar problemas al eliminar varios archivos mediante el uso de caracteres glob. Dado que estos archivos se eliminan sin lugar a dudas, un usuario podría terminar eliminando archivos que no estaban destinados a eliminarse.

> Advertencia
> Los archivos se eliminan de forma permanente. No hay ningún comando para recuperar un archivo y no hay una papelera desde la que recuperar los archivos eliminados.

Como precaución, los usuarios deben usar la opción -i al eliminar varios archivos:

```shell
sysadmin@localhost:~$ touch sample.txt example.txt test.txt            
sysadmin@localhost:~$ ls                                               
Desktop    Downloads  Pictures  Templates  example.txt  test.txt       
Documents  Music      Public    Videos     sample.txt                 
sysadmin@localhost:~$ rm -i *.txt                                     
rm: remove regular empty file `example.txt'? y                         
rm: remove regular empty file `sample.txt'? n                          
rm: remove regular empty file `test.txt'? y                            
sysadmin@localhost:~$ ls                                               
Desktop    Downloads  Pictures  Templates  sample.txt                  
Documents  Music      Public    Videos   
```
### 8.6.1 Eliminación de directorios

Puede eliminar directorios mediante el comando rm. Sin embargo, el comportamiento predeterminado (sin opciones) del comando rm es no eliminar directorios:

```shell
sysadmin@localhost:~$ rm Videos                                        
rm: cannot remove `Videos': Is a directory   
```

Para eliminar un directorio con el comando rm, use la  opción _recursiva_ -r  :

```shell
sysadmin@localhost:~$ ls                                               
Desktop    Downloads  Pictures  Templates  sample.txt                  
Documents  Music      Public    Videos                                 
sysadmin@localhost:~$ rm -r Videos                                     
sysadmin@localhost:~$ ls                                               
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  sample.txt 
```

Cuando un usuario elimina un directorio, todos los archivos y subdirectorios se eliminan sin ninguna pregunta interactiva. Lo mejor es utilizar la opción -i con el comando rm.

También puede eliminar un directorio con el comando rmdir, pero solo si el directorio está vacío.

```shell
sysadmin@localhost:~$ rmdir Documents                                           
rmdir: failed to remove 'Documents': Directory not empty
```
## 8.7 Creación de directorios
Para crear un directorio, utilice el comando mkdir:

```shell
sysadmin@localhost:~$ ls                                               
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  sample.txt
sysadmin@localhost:~$ mkdir test                                       
sysadmin@localhost:~$ ls                                               
Desktop    Downloads  Pictures  Templates   test                       
Documents  Music      Public    sample.txt
```