# 10. Trabajando con Texto
## 10.1 Introducción
Un gran número de los archivos de un sistema de archivos típico son archivos de _texto_. Los archivos de texto solo contienen texto, no las características de formato que podría ver en un archivo de procesamiento de texto.

Debido a que hay tantos de estos archivos en un sistema Linux típico, existe un número significativo de comandos para ayudar a los usuarios a manipular archivos de texto. Hay comandos para ver y modificar estos archivos de varias maneras.

Además, hay características disponibles para que el shell controle la salida de los comandos, por lo que en lugar de tener la salida colocada en la ventana del terminal, la salida se puede _redirigir_ a otro archivo u otro comando. Estas características de redireccionamiento proporcionan a los usuarios un entorno mucho más flexible y potente en el que trabajar.

![](img/20241008173805.png)

### 10.1.1 Visualización de archivos en el terminal
El comando cat, abreviatura  _de concatenar,_ es un comando simple pero útil cuyas funciones incluyen crear y mostrar archivos de texto, así como combinar copias de archivos de texto. Uno de los usos más populares de cat es mostrar el contenido de archivos de texto. Para mostrar un archivo en la salida estándar utilizando el comando cat, escriba el comando seguido del nombre del archivo:

```
sysadmin@localhost:~$ cd Documents
sysadmin@localhost:~/Documents$ cat food.txt 
Food is good.
```

Aunque el terminal es la salida predeterminada de este comando, el comando cat también se puede usar para redirigir el contenido del archivo a otros archivos o ingresar para otro comando mediante el uso de caracteres de redirección.
### 10.1.2 Visualización de archivos mediante un localizador
Si bien la visualización de archivos pequeños con el comando `cat` no plantea problemas, no es una opción ideal para archivos grandes. El comando `cat` no proporciona formas sencillas de pausar y reiniciar la pantalla, por lo que todo el contenido del archivo se vuelca a la pantalla.

En el caso de archivos más grandes, utilice un comando de _paginación_ para ver el contenido. Los comandos de paginación muestran una página de datos a la vez, lo que le permite avanzar y retroceder en el archivo mediante teclas de movimiento.

Hay dos comandos de paginador de uso común:

- El comando `less` proporciona una capacidad de paginación muy avanzada. Por lo general, es el paginador predeterminado utilizado por comandos como el comando `man`.
- La mayor cantidad de comando ha existido desde los primeros días de UNIX. Aunque tiene menos funciones que el comando less, sin embargo, el comando less no se incluye en todas las distribuciones de Linux. El comando more siempre está disponible.

Los comandos de más y menos permiten a los usuarios moverse por el documento mediante comandos de pulsación de teclas. Dado que los desarrolladores basaron el comando less en la funcionalidad del comando more, todos los comandos de pulsación de teclas disponibles en el comando more también funcionan en el comando less.

El enfoque de nuestro contenido está en el comando menos avanzado. El comando more sigue siendo útil recordarlo para los momentos en que el comando less no está disponible. Recuerde que la mayoría de los comandos de pulsación de teclas proporcionados funcionan para ambos comandos.
#### 10.1.2.1 Comandos de movimiento del localizador
Para ver un archivo con el comando less, pase el nombre del archivo como argumento:

```
sysadmin@localhost:~/Documents$ less words
```

Hay muchos comandos de movimiento para el comando menor, cada uno con múltiples teclas posibles o combinaciones de teclas. Si bien esto puede parecer intimidante, no es necesario memorizar todos estos comandos de movimiento. Al ver un archivo con el comando less, use la tecla **H** o **Mayús+H** para mostrar una pantalla de ayuda:

```
                   SUMMARY OF LESS COMMANDS

      Commands marked with * may be preceded by a number, N.
      Notes in parentheses indicate the behavior if N is given.
      A key preceded by a caret indicates the Ctrl key; thus ^K is ctrl-K.

  h  H                 Display this help.
  q  :q  Q  :Q  ZZ     Exit.
 ------------------------------------------------------------------------
                           MOVING
                                                                        
  e  ^E  j  ^N  CR  *  Forward  one line   (or N lines).
  y  ^Y  k  ^K  ^P  *  Backward one line   (or N lines).
  f  ^F  ^V  SPACE  *  Forward  one window (or N lines).
  b  ^B  ESC-v      *  Backward one window (or N lines).
  z                 *  Forward  one window (and set window to N).
  w                 *  Backward one window (and set window to N).
  ESC-SPACE         *  Forward  one window, but don't stop at end-of-file.
  d  ^D             *  Forward  one half-window (and set half-window to N).
  u  ^U             *  Backward one half-window (and set half-window to N).
  ESC-)  RightArrow *  Left  one half screen width (or N positions).
  ESC-(  LeftArrow  *  Right one half screen width (or N positions).
HELP -- Press RETURN for more, or q when done
```

El primer grupo de comandos de movimiento en los que hay que centrarse son los que se utilizan con más frecuencia. Para hacerlo aún más conveniente, las claves que son idénticas en más y menos se resumen a continuación para demostrar cómo moverse más y menos al mismo tiempo:

|**Llave**|**Movimiento**|
|---|---|
|**Barra espaciadora**|Ventana hacia adelante|
|**B**|Ventana hacia atrás|
|**Entrar**|Línea hacia adelante|
|**Q**|Salida|
|**H**|Ayuda|

Cuando se usa less como paginador, la forma más fácil de avanzar una página es presionar la **barra espaciadora**.
#### 10.1.2.2 Comandos de búsqueda de buscapersonas
Hay dos formas de buscar en el comando less: mirando hacia adelante o hacia atrás desde su posición actual.

Para iniciar una búsqueda para mirar hacia adelante desde su posición actual, use la barra / tecla  . A continuación, escriba el texto o el patrón que desee y pulse la tecla **Intro**.

```
Abdul
Abdul's
Abe
/frog
```

Si se puede encontrar una coincidencia, el cursor se mueve en el documento hasta la coincidencia. Por ejemplo, en el siguiente gráfico se buscó la expresión "rana" en el archivo de palabras:

```
bullfrog
bullfrog's
bullfrogs
bullheaded
bullhorn
bullhorn's
```

Nótese que "rana" no tenía que ser una palabra por sí misma. Observe también que, mientras el comando menos se movió a la primera coincidencia desde la posición actual, todas las coincidencias se resaltaron.

Si no se pueden encontrar coincidencias hacia adelante desde su posición actual, entonces la última línea de la pantalla informará Patrón no encontrado:

```
Pattern not found (press RETURN)
```

Para buscar hacia atrás desde su posición actual, presione el signo de interrogación **?** y, a continuación, escriba el texto o el motivo que desee hacer coincidir y pulse la tecla **Intro**. El cursor se mueve hacia atrás hasta la primera coincidencia que puede encontrar o informa de que no se puede encontrar el patrón.

Si se puede encontrar más de una coincidencia mediante una búsqueda, use la tecla n  para mover la _siguiente_ coincidencia y use la combinación de teclas **Mayús + N** para ir a una  coincidencia _anterior_.

> Los términos de búsqueda en realidad usan patrones llamados _expresiones regulares_. Más adelante en este capítulo se proporcionan más detalles sobre las expresiones regulares.
### 10.1.3 Cabeza y cola
Los comandos head y tail se utilizan para mostrar solo las primeras o las últimas líneas de un archivo, respectivamente (o, cuando se utilizan con una tubería, la salida de un comando anterior). De forma predeterminada, los comandos head y tail muestran diez líneas del archivo que se proporciona como argumento.

Por ejemplo, el siguiente comando muestra las primeras diez líneas del archivo /etc/sysctl.conf:

```
sysadmin@localhost:~/Documents$ cd
sysadmin@localhost:~$ head /etc/sysctl.conf
#
# /etc/sysctl.conf - Configuration file for setting system variables
# See /etc/sysctl.d/ for additional system variables
# See sysctl.conf (5) for information.
#

#kernel.domainname = example.com

# Uncomment the following to stop low-level messages on console
#kernel.printk = 3 4 1 3
```

Pasar un número como opción hará que los comandos head y tail generen el número especificado de líneas, en lugar de la decena estándar. Por ejemplo, para mostrar las últimas cinco líneas del archivo `/etc/sysctl.conf`, utilice la opción -5:

```
sysadmin@localhost:~$ tail -5 /etc/sysctl.conf
# Protects against creating or following links under certain conditions
# Debian kernels have both set to 1 (restricted)
# See https://www.kernel.org/doc/Documentation/sysctl/fs.txt
#fs.protected_hardlinks=0
#fs.protected_symlinks=0
```

La opción -n también se puede utilizar para indicar el número de líneas que se van a generar. Pase un número como argumento a la opción:

```
sysadmin@localhost:~$ head -n 3 /etc/sysctl.conf
#
# /etc/sysctl.conf - Configuration file for setting system variables
# See /etc/sysctl.d/ for additional system variables
```

**Opción de valor negativo**
Tradicionalmente en UNIX, el número de líneas a generar se especificaba como una opción con cualquiera de los comandos, por lo que -3 significaba mostrar tres líneas. Para el comando tail, -3 o -n -3 todavía significa mostrar tres líneas.

Sin embargo, la versión GNU del comando head reconoce -n -3 como show _all excepto las últimas tres líneas_, y aún así el head command aún reconoce la opción -3 como show las tres primeras líneas.

**Opción de Valor Positivo**
La versión GNU del comando tail permite una variación de cómo especificar el número de líneas a imprimir. Si se utiliza la opción -n con un número precedido por el signo más, el comando tail reconoce que esto significa mostrar el contenido comenzando en la línea especificada y continuando hasta el final.

Por ejemplo, a continuación se muestra el contenido de /etc/passwd desde la línea 25 hasta el final del archivo:

```
sysadmin@localhost:~$ nl /etc/passwd | tail -n +25
    25  sshd:x:103:65534::/var/run/sshd:/usr/sbin/nologin
    26  operator:x:1000:37::/root:/bin/sh
    27  sysadmin:x:1001:1001:System Administrator,,,,:/home/sysadmin:/bin/bash
```

> Los cambios en los archivos en vivo se pueden ver mediante la opción -f del comando tail, lo que resulta útil cuando se desea ver los cambios en un archivo a medida que se producen.
> Un buen ejemplo de esto sería cuando se visualizan archivos de registro como administrador del sistema. Los archivos de registro se pueden usar para solucionar problemas y los administradores a menudo los ven "interactivamente" con el comando tail mientras ejecutan comandos en una ventana separada.
> Por ejemplo, si tuviera que iniciar sesión como usuario root, podría solucionar problemas con el servidor de correo electrónico viendo los cambios en vivo en el archivo de registro `/var/log/mail.log`.
## 10.2 Canalizaciones de línea de comandos
El  carácter pipe | se puede utilizar para enviar la salida de un comando a otro. Normalmente, cuando un comando tiene salida o genera un error, la salida se muestra en la pantalla; Sin embargo, esto no tiene por qué ser así. En lugar de imprimirse en la pantalla, la salida de un comando se convierte en entrada para el siguiente comando. Esta herramienta puede ser poderosa, especialmente cuando se buscan datos específicos; _Las tuberías_ se utilizan a menudo para refinar los resultados de un comando inicial.

En ejemplos anteriores, los comandos head y tail recibían archivos como argumentos para operar. Sin embargo, el carácter de barra vertical le permite utilizar estos comandos no solo en archivos, sino también en la salida de otros comandos. Esto puede ser útil cuando se enumera un directorio grande, por ejemplo, el directorio /etc:

```
sysadmin@localhost:~$ ls /etc
X11                     gss              mke2fs.conf     rpc
adduser.conf            host.conf        modprobe.d      rsyslog.conf
alternatives            hostname         modules         rsyslog.d
apparmor                hosts            modules-load.d  securetty
apparmor.d              hosts.allow      motd            security
apt                     hosts.deny       mtab            selinux
bash.bashrc             init.d           nanorc          services
bind                    initramfs-tools  netplan         shadow
bindresvport.blacklist  inputrc          network         shadow-
binfmt.d                insserv.conf.d   networks        shells
ca-certificates         iproute2         newt            skel
ca-certificates.conf    issue            nsswitch.conf   ssh
calendar                issue.net        opt             ssl
console-setup           kernel           os-release      subgid
cron.d                  ld.so.cache      pam.conf        subgid-
cron.daily              ld.so.conf       pam.d           subuid
cron.hourly             ld.so.conf.d     passwd          subuid-
cron.monthly            ldap             passwd-         sudoers
cron.weekly             legal            perl            sudoers.d
crontab                 libaudit.conf    pinforc         sysctl.conf
dbus-1                  locale.alias     ppp             sysctl.d
debconf.conf            locale.gen       profile         systemd
debian_version          localtime        profile.d       terminfo
default                 logcheck         protocols       timezone
deluser.conf            login.defs       python3         tmpfiles.d
depmod.d                logrotate.conf   python3.6       ucf.conf
dhcp                    logrotate.d      rc0.d           udev
dpkg                    lsb-release      rc1.d           ufw
environment             machine-id       rc2.d           update-motd.d
fstab                   magic            rc3.d           updatedb.conf
gai.conf                magic.mime       rc4.d           vim
groff                   mailcap          rc5.d           vtrgb
group                   mailcap.order    rc6.d           wgetrc
group-                  manpath.config   rcS.d           xdg
gshadow                 mc               resolv.conf
gshadow-                mime.types       rmt
```

El comando anterior enumera un gran número de archivos. Si ejecuta esto en nuestro terminal, la salida se corta y solo se puede ver si se desplaza hacia arriba. Para ver más fácilmente el comienzo de la salida, críela al comando head. En el ejemplo siguiente se muestran solo las diez primeras líneas:

```
sysadmin@localhost:~$ ls /etc | head
X11
adduser.conf
alternatives
apparmor
apparmor.d
apt
bash.bashrc
bind
bindresvport.blacklist
binfmt.d
```

La salida completa del comando ls se pasa al comando head por el shell en lugar de imprimirse en la pantalla. El comando head toma esta salida del comando ls como datos de entrada, y la salida de head se imprime en la pantalla.

Se pueden usar varias tuberías consecutivamente para vincular varios comandos entre sí. Si se canalizan tres comandos juntos, la salida del primer comando se pasa al segundo comando. A continuación, la salida del segundo comando se pasa al tercer comando. A continuación, la salida del tercer comando se imprimiría en la pantalla.

Es importante elegir cuidadosamente el orden en el que se canalizan los comandos, ya que cada comando solo ve la entrada del comando anterior. Los siguientes ejemplos ilustran esto usando el comando nl, que agrega números de línea a la salida. En el primer ejemplo, el comando nl se utiliza para numerar las líneas de la salida del comando ls anterior:

```
sysadmin@localhost:~$ ls /etc/ssh | nl
     1  moduli
     2  ssh_config
     3  ssh_host_ecdsa_key
     4  ssh_host_ecdsa_key.pub
     5  ssh_host_ed25519_key
     6  ssh_host_ed25519_key.pub
     7  ssh_host_rsa_key
     8  ssh_host_rsa_key.pub
     9  ssh_import_id
    10  sshd_config
```

En el siguiente ejemplo, observe que el comando ls se ejecuta primero y su salida se envía al comando nl, numerando todas las líneas de la salida del comando ls. A continuación, se ejecuta el comando tail, que muestra las últimas cinco líneas de la salida del comando nl:

```
sysadmin@localhost:~$ ls /etc/ssh | nl | tail -5
     6  ssh_host_ed25519_key.pub
     7  ssh_host_rsa_key
     8  ssh_host_rsa_key.pub
     9  ssh_import_id
    10  sshd_config
```

Compare el resultado anterior con el siguiente ejemplo:

```
sysadmin@localhost:~$ ls /etc/ssh | tail -5 | nl
     1  ssh_host_ed25519_key.pub
     2  ssh_host_rsa_key
     3  ssh_host_rsa_key.pub
     4  ssh_import_id
     5  sshd_config
```

Observe cómo los números de línea son diferentes. ¿A qué se debe esto?

En el segundo ejemplo, la salida del comando ls se envía primero al comando tail, que solo toma las últimas cinco líneas de la salida. Luego, el comando tail envía esas cinco líneas al comando nl, que las numera del 1 al 5.

Las canalizaciones pueden ser potentes, pero es necesario tener en cuenta cómo se canalizan los comandos para garantizar que se muestre la salida deseada.
## 10.3 Redirección de entrada/salida
_La redirección de entrada/salida (E/S)_ permite que la información de la línea de comandos se pase a diferentes flujos. Antes de hablar sobre la redirección, es importante comprender los _flujos estándar_.

- STDIN

_La entrada estándar_, o _STDIN,_ es la información introducida normalmente por el usuario a través del teclado. Cuando un comando solicita datos al shell, el shell proporciona al usuario la capacidad de escribir comandos que, a su vez, se envían al comando como STDIN.

- STDOUT

_La salida estándar_, o _STDOUT,_ es la salida normal de los comandos. Cuando un comando funciona correctamente (sin errores), la salida que produce se denomina STDOUT. De forma predeterminada, STDOUT se muestra en la ventana del terminal donde se está ejecutando el comando. STDOUT también se conoce como _flujo_ o _canal_ #1.

- STDERR

_El error estándar_, o _STDERR_, son mensajes de error generados por comandos. De forma predeterminada, STDERR se muestra en la ventana del terminal donde se está ejecutando el comando. STDERR también se conoce como _flujo_ o _canal_ #2.
‌⁠​

La redirección de E/S permite al usuario redirigir STDIN para que los datos provengan de un archivo y STDOUT/STDERR para que la salida vaya a un archivo. La redirección se logra mediante el uso de la flecha < > caracteres.
### 10.3.1 STDOUT
STDOUT se puede dirigir a archivos. Para comenzar, observe la salida del siguiente comando echo que se muestra en la pantalla:

```
sysadmin@localhost:~$ echo "Line 1"
Line 1
```

Usando el carácter >, la salida se puede redirigir a un archivo en su lugar:

```
sysadmin@localhost:~$ echo "Line 1" > example.txt
```

Este comando no muestra ninguna salida porque STDOUT se envió al archivo example.txt en lugar de a la pantalla. Puede ver el nuevo archivo con la salida del comando ls.

```
sysadmin@localhost:~$ ls
Desktop    Downloads  Pictures  Templates  example.txt
Documents  Music      Public    Videos   
```

El archivo contiene la salida del comando echo, que se puede ver con el comando cat:

```
sysadmin@localhost:~$ cat example.txt                                  
Line 1
```

Es importante tener en cuenta que la flecha única sobrescribe cualquier contenido de un archivo existente:

```
sysadmin@localhost:~$ cat example.txt
Line 1
sysadmin@localhost:~$ echo "New line 1" > example.txt
sysadmin@localhost:~$ cat example.txt
New line 1
```

El contenido original del archivo desaparece y se reemplaza con la salida del nuevo comando echo.

También es posible conservar el contenido de un archivo existente añadiéndolo. Utilice dos caracteres de >> de flecha para anexar a un archivo en lugar de sobrescribirlo:

```
sysadmin@localhost:~$ cat example.txt
New line 1
sysadmin@localhost:~$ echo "Another line" >> example.txt
sysadmin@localhost:~$ cat example.txt
New line 1
Another line
```

En lugar de sobrescribirse, la salida del comando echo se agrega a la parte inferior del archivo.
### 10.3.2 STDERR
STDERR se puede redirigir de forma similar a STDOUT. Cuando se utiliza el carácter de flecha para redirigir, se asume la secuencia #1 (STDOUT) a menos que se especifique otra secuencia. Por lo tanto, la secuencia #2 debe especificarse al redirigir STDERR colocando el número 2 antes de la flecha > carácter.

Para demostrar el redireccionamiento de STDERR, primero observe el siguiente comando que produce un error porque el directorio especificado no existe:

```
sysadmin@localhost:~$ ls /fake
ls: cannot access /fake: No such file or directory
```

Tenga en cuenta que no hay nada en el ejemplo anterior que implique que la salida sea STDERR. La salida es claramente un mensaje de error, pero ¿cómo podría saber que se está enviando a STDERR? Una forma sencilla de determinar esto es redirigir STDOUT:

```
sysadmin@localhost:~$ ls /fake > output.txt
ls: cannot access /fake: No such file or directory
```

En el ejemplo anterior, STDOUT se redirigió al archivo output.txt. Por lo tanto, la salida que se muestra no puede ser STDOUT porque se habría colocado en el archivo output.txt en lugar del terminal. Dado que todos los resultados de los comandos van a STDOUT o STDERR, el resultado que se muestra arriba debe ser STDERR.

La salida STDERR de un comando se puede enviar a un archivo:

```
sysadmin@localhost:~$ ls /fake 2> error.txt
```

En el ejemplo, el 2> indica que todos los mensajes de error deben enviarse al archivo error.txt, lo que se puede confirmar mediante el comando cat:

```
sysadmin@localhost:~$ cat error.txt
ls: cannot access /fake: No such file or directory
```
### 10.3.3 Redireccionamiento de múltiples flujos
Es posible dirigir tanto el STDOUT como el STDERR de un comando al mismo tiempo. El siguiente comando genera STDOUT y STDERR porque uno de los directorios especificados existe y el otro no:

```
sysadmin@localhost:~$ ls /fake /etc/ppp
ls: cannot access /fake: No such file or directory
/etc/ppp:
ip-down.d  ip-up.d
```

Si solo se envía el STDOUT a un archivo, STDERR se sigue imprimiendo en la pantalla:

```
sysadmin@localhost:~$ ls /fake /etc/ppp > example.txt
ls: cannot access /fake: No such file or directory
sysadmin@localhost:~$ cat example.txt
/etc/ppp:
ip-down.d
ip-up.d
```

Si solo se envía el STDERR a un archivo, STDOUT se sigue imprimiendo en la pantalla:

```
sysadmin@localhost:~$ ls /fake /etc/ppp 2> error.txt
/etc/ppp:
ip-down.d
ip-up.d
sysadmin@localhost:~$ cat error.txt
ls: cannot access /fake: No such file or directory
```

Tanto STDOUT como STDERR se pueden enviar a un archivo utilizando el carácter & delante de la flecha > carácter. El conjunto de caracteres &> significa 1> y 2>:

```
sysadmin@localhost:~$ ls /fake /etc/ppp &> all.txt
sysadmin@localhost:~$ cat all.txt
ls: cannot access /fake: No such file or directory
/etc/ppp:
ip-down.d
ip-up.d
```

Tenga en cuenta que cuando utiliza &>, la salida aparece en el archivo con todos los mensajes STDERR en la parte superior y todos los mensajes STDOUT debajo de todos los mensajes STDERR:

```
sysadmin@localhost:~$ ls /fake /etc/ppp /junk /etc/sound &> all.txt
sysadmin@localhost:~$ cat all.txt
ls: cannot access '/fake': No such file or directory
ls: cannot access '/junk': No such file or directory
ls: cannot access '/etc/sound': No such file or directory
/etc/ppp:
ip-down.d
ip-up.d
```

Si no desea que STDERR y STDOUT vayan al mismo archivo, se pueden redirigir a archivos diferentes mediante > y 2>. Por ejemplo, para dirigir STDOUT a example.txt y STDERR a error.txt ejecute lo siguiente:

```
sysadmin@localhost:~$ ls /fake /etc/ppp > example.txt 2> error.txt
sysadmin@localhost:~$ cat error.txt
ls: cannot access /fake: No such file or directory
sysadmin@localhost:~$ cat example.txt
/etc/ppp:
ip-down.d
ip-up.d
```

> El orden en el que se especifican las secuencias no importa.
### 10.3.4 STDIN
El concepto de redireccionar STDIN es difícil porque es más difícil entender _por qué_ querría redirigir STDIN. Con STDOUT y STDERR, su propósito es sencillo; A veces es útil almacenar la salida en un archivo para su uso futuro.

La mayoría de los usuarios de Linux terminan redirigiendo STDOUT de forma rutinaria, STDERR en ocasiones y STDIN muy raramente.

Hay muy pocos comandos que requieran que redirija STDIN porque con la mayoría de los comandos, si desea leer datos de un archivo en un comando, puede especificar el nombre del archivo como argumento para el comando.

En el caso de algunos comandos, si no especifica un nombre de archivo como argumento, vuelven a usar STDIN para obtener datos. Por ejemplo, considere el siguiente comando cat:

```
sysadmin@localhost:~$ cat
hello
hello‌⁠​​⁠​ 
how are you?
how are you?
goodbye
goodbye
```

> Si intenta ejecutar el comando cat sin argumentos, finalice el proceso y vuelva al símbolo del sistema mediante **Ctrl+C**.

En el ejemplo anterior, al comando cat no se le proporciona un nombre de archivo como argumento. Por lo tanto, pide que los datos se muestren en la pantalla de STDIN. El usuario escribe hello y, a continuación, el comando cat muestra hello en la pantalla. Si bien esto es medianamente entretenido, no es particularmente útil.

Sin embargo, si la salida del comando cat se redirigiera a un archivo, este método podría usarse para agregar texto a un archivo existente o para colocar texto en un archivo nuevo.

El primer comando en el ejemplo siguiente redirige la salida del comando cat a un archivo recién creado llamado new.txt. A esta acción le sigue proporcionando al comando cat el archivo new.txt como argumento para mostrar el texto redirigido en STDOUT.

```
sysadmin@localhost:~$ cat > new.txt
Hello
How are you?
Goodbye
sysadmin@localhost:~$ cat new.txt                               
Hello
How are you?
Goodbye
```

Aunque en el ejemplo anterior se muestra otra ventaja de redirigir STDOUT, no se aborda por qué o cómo se puede dirigir STDIN. Para entender esto, considere un nuevo comando llamado tr. Este comando toma un conjunto de caracteres y  _los traduce_ en otro conjunto de caracteres.

Por ejemplo, para poner en mayúsculas una línea de texto, utilice el comando tr de la siguiente manera:

```
sysadmin@localhost:~$ tr 'a-z' 'A-Z'
watch how this works
WATCH HOW THIS WORKS
```

El comando tr tomó el STDIN del teclado y convirtió todas las letras minúsculas antes de enviar STDOUT a la pantalla.

Parecería que un mejor uso del comando tr sería realizar la traducción en un archivo, no en la entrada del teclado. Sin embargo, el comando tr no admite argumentos de nombre de archivo:

```
sysadmin@localhost:~$ cat example.txt
/etc/ppp:
ip-down.d
ip-up.d
sysadmin@localhost:~$ tr 'a-z' 'A-Z' example.txt
tr: extra operand `example.txt'
Try `tr --help' for more information
```

Es posible, sin embargo, decirle al shell que obtenga STDIN de un archivo en lugar de hacerlo desde el teclado utilizando el carácter <:

```
sysadmin@localhost:~$ tr 'a-z' 'A-Z' < example.txt
/ETC/PPP:
IP-DOWN.D
IP-UP.D
```

La mayoría de los comandos aceptan nombres de archivo como argumentos, por lo que este caso de uso es relativamente raro. Sin embargo, para aquellos que no lo hacen, este método podría usarse para hacer que el shell lea del archivo en lugar de confiar en el comando para tener esta habilidad.

Una última nota para guardar la salida resultante, rediríjala a otro archivo:

```
sysadmin@localhost:~$ tr 'a-z' 'A-Z' < example.txt > newexample.txt
sysadmin@localhost:~$ cat newexample.txt
/ETC/PPP:
IP-DOWN.D
IP-UP.D
```
## 10.4 Clasificación de archivos o entradas
El comando de ordenación se puede utilizar para reorganizar las líneas de los archivos o la entrada en orden de diccionario o numérico. En el siguiente ejemplo se crea un archivo pequeño, utilizando el comando head para tomar las primeras 5 líneas del archivo /etc/passwd y enviar la salida a un archivo llamado mypasswd.

```
sysadmin@localhost:~$ head -5 /etc/passwd > mypasswd
sysadmin@localhost:~$ cat mypasswd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
```

Ahora le haremos con `sort` el archivo mypasswd:

```
sysadmin@localhost:~$ sort mypasswd
bin:x:2:2:bin:/bin:/usr/sbin/nologin
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
root:x:0:0:root:/root:/bin/bash
sync:x:4:65534:sync:/bin:/bin/sync
sys:x:3:3:sys:/dev:/usr/sbin/nologin
```

Tras un examen detallado de la salida en el ejemplo anterior, el comando de ordenación ha organizado las líneas del archivo en orden alfabético. Compare este resultado con el resultado del comando cat anterior.
### 10.4.1 Campos y opciones de clasificación
El comando de ordenación puede reorganizar la salida en función del contenido de uno o más campos. Los campos están determinados por un _delimitador de campo_ contenido en cada línea. En informática, un delimitador es un carácter que separa una cadena de texto o datos; De forma predeterminada, utiliza espacios en blanco, como espacios o tabulaciones.

El siguiente comando se puede utilizar para ordenar numéricamente el tercer campo del archivo mypasswd. Para lograr esta clasificación se utilizan tres opciones:

|**Opción**|**Función**|
|---|---|
|-t:|La opción -t especifica el _delimitador de campo_. Si el archivo o la entrada están separados por un delimitador que no sea un espacio en blanco, por ejemplo, una coma o dos puntos, la opción -t permitirá especificar otro separador de campo como argumento.<br><br>El archivo mypasswd utilizado en el ejemplo anterior utiliza un carácter de dos puntos : como delimitador para separar los campos, por lo que en el ejemplo siguiente se utiliza la opción -t:.|
|-k3|La opción -k especifica el número de _campo_. Para especificar el campo por el que se va a ordenar, utilice la opción -k con un argumento para indicar el número de campo, empezando por 1 para el primer campo.<br><br>En el ejemplo siguiente se utiliza la opción -k3 para ordenar por el tercer campo.|
|-n|Esta opción especifica el tipo de _ordenación_.<br><br>El tercer campo del archivo mypasswd contiene números, por lo que la opción -n se utiliza para realizar una ordenación numérica.|
```
sysadmin@localhost:~$ sort -t: -n -k3 mypasswd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
```

Otra opción comúnmente utilizada para el comando de ordenación es la opción -r, que se utiliza para realizar una  ordenación inversa. A continuación se muestra el mismo comando que el ejemplo anterior, con la adición de la opción -r, lo que hace que los números más altos del tercer campo aparezcan en la parte superior de la salida:

```
sysadmin@localhost:~$ sort -t: -n -r -k3 mypasswd
sync:x:4:65534:sync:/bin:/bin/sync 
sys:x:3:3:sys:/dev:/usr/sbin/nologin  
bin:x:2:2:bin:/bin:/usr/sbin/nologin  
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
root:x:0:0:root:/root:/bin/bash  
```

Por último, es posible que desee realizar ordenaciones más complejas, como ordenar por un campo primario y, a continuación, por un campo secundario. Por ejemplo, considere el siguiente  _archivo de valores separados por comas_, un archivo en el que el carácter de coma es el delimitador de campo:

> Utilice el siguiente comando para cambiar al directorio Documentos:
```
sysadmin@localhost:~$ cd ~/Documents
```

```
sysadmin@localhost:~/Documents$ cat os.csv
1970,Unix,Richie
1987,Minix,Tanenbaum
1970,Unix,Thompson
1991,Linux,Torvalds
```

Para ordenar primero por el sistema operativo (campo #2) y luego por año (campo #1) y luego por apellido (campo #3), use el siguiente comando:

```
sysadmin@localhost:~/Documents$ sort -t, -k2 -k1n -k3 os.csv
1991,Linux,Torvalds
1987,Minix,Tanenbaum
1970,Unix,Richie
1970,Unix,Thompson
```

En la tabla siguiente se desglosan las opciones utilizadas en el ejemplo anterior:

|**Opción**|**Función**|
|---|---|
|-t,|Especifica el carácter de coma como delimitador de campo|
|-k2|Ordenar por campo #2|
|-k1n|_Ordenar numéricamente_ por campo #1|
|-k3|Ordenar por campo #3|
> Utilice el siguiente comando para volver al directorio principal:
```
sysadmin@localhost:~/Documents$ cd
sysadmin@localhost:~$
```
## 10.5 Visualización de estadísticas de archivos
El comando wc proporciona el número de líneas, palabras y bytes (1 byte = 1 carácter en un archivo de texto) para un archivo, y un recuento total de líneas si se especifica más de un archivo. De forma predeterminada, el comando wc permite imprimir hasta tres estadísticas para cada archivo proporcionado, así como el total de estas estadísticas si se proporciona más de un nombre de archivo:

```
sysadmin@localhost:~$ wc /etc/passwd /etc/passwd-
  35   56 1710 /etc/passwd
  34   55 1665 /etc/passwd-
  69  111 3375 total 
```

La salida del ejemplo anterior tiene cuatro columnas:

1. Número de líneas
2. Número de palabras
3. Número de bytes
4. Nombre de archivo

También es posible ver solo estadísticas específicas, utilizando la opción -l para mostrar solo el número de líneas, la opción -w para mostrar solo el número de palabras, la opción -c para mostrar solo el número de bytes, o cualquier combinación de estas opciones.

El comando wc puede ser útil para contar el número de líneas emitidas por algún otro comando a través de una tubería. Por ejemplo, si desea conocer el número total de archivos en el directorio /etc, canalice la salida de ls a wc y cuente solo el número de líneas:

```
sysadmin@localhost:~$ ls /etc/ | wc -l
142
```
## 10.6 Filtrar secciones de archivos
El comando cut puede extraer columnas de texto de un archivo o de una entrada estándar. Se utiliza principalmente para trabajar con archivos de base de datos delimitados. De nuevo, los archivos delimitados son archivos que contienen columnas separadas por un delimitador. Estos archivos son muy comunes en los sistemas Linux.

De forma predeterminada, el comando cut espera que su entrada esté separada por el carácter de tabulación, pero la opción -d puede especificar delimitadores alternativos, como los dos puntos o la coma.

La opción -f puede especificar los campos que se van a mostrar, ya sea como un rango con guiones o como una lista separada por comas.

En el siguiente ejemplo, se muestran los campos primero, quinto, sexto y séptimo del archivo de base de datos mypasswd:

```
sysadmin@localhost:~$ cut -d: -f1,5-7 mypasswd
root:root:/root:/bin/bash
daemon:daemon:/usr/sbin:/usr/sbin/nologin
bin:bin:/bin:/usr/sbin/nologin
sys:sys:/dev:/usr/sbin/nologin
sync:sync:/bin:/bin/sync
```

El comando cut también puede extraer columnas de texto en función de la posición del carácter con la opción -c, lo que resulta útil cuando se trabaja con archivos de bases de datos de ancho fijo o salidas de comandos.

Por ejemplo, los campos del comando ls -l siempre están en las mismas posiciones de caracteres. A continuación se mostrará solo el tipo de archivo (carácter 1), los permisos (caracteres 2-10), un espacio (carácter 11) y el nombre de archivo (caracteres 50+):

```
sysadmin@localhost:~$ ls -l | cut -c1-11,50-
total 44
drwxr-xr-x Desktop
drwxr-xr-x Documents
drwxr-xr-x Downloads
drwxr-xr-x Music
drwxr-xr-x Pictures
drwxr-xr-x Public
drwxr-xr-x Templates
drwxr-xr-x Videos
-rw-rw-r-- all.txt
-rw-rw-r-- example.txt
-rw-rw-r-- mypasswd
-rw-rw-r-- new.txt
```
## 10.7 Filtrar el contenido del archivo
El comando grep se puede utilizar para filtrar líneas en un archivo o la salida de otro comando que coincida con un patrón especificado. Ese patrón puede ser tan simple como el texto exacto que desea hacer coincidir o puede ser mucho más avanzado mediante el uso de expresiones regulares.

Por ejemplo, para encontrar todos los usuarios que pueden iniciar sesión en el sistema con el shell BASH, se puede usar el comando grep para filtrar las líneas del archivo /etc/passwd para las líneas que contienen el patrón bash:

```
sysadmin@localhost:~$ grep bash /etc/passwd
root:x:0:0:root:/root:/bin/bash
sysadmin:x:1001:1001:System Administrator,,,,:/home/sysadmin:/bin/bash
```

Para que sea más fácil ver qué coincide exactamente, use la opción --color. Esta opción resaltará los elementos coincidentes en rojo:

```
sysadmin@localhost:~$ grep --color bash /etc/passwd
root:x:0:0:root:/root:/bin/bash
sysadmin:x:1001:1001:System Administrator,,,,:/home/sysadmin:/bin/bash
```

> En nuestras máquinas virtuales, el comando grep tiene un alias para incluir la opción --color automáticamente.

En algunos casos, puede que no sea importante encontrar las líneas específicas que coincidan con el patrón, sino cuántas líneas coinciden con el patrón. La opción -c proporciona un recuento de cuántas líneas coinciden:

```
sysadmin@localhost:~$ grep -c bash /etc/passwd
2
```

Al ver la salida del comando grep, puede ser difícil determinar los números de línea originales. Esta información puede ser útil cuando se vuelve a entrar en el archivo (tal vez para editar el archivo) para encontrar rápidamente una de las líneas coincidentes.

La opción -n del comando grep mostrará los números de línea originales. Para mostrar todas las líneas y sus números de línea en el archivo /etc/passwd que contiene el patrón bash:

```
sysadmin@localhost:~$ grep -n bash /etc/passwd                          
1:root:x:0:0:root:/root:/bin/bash                                       
27:sysadmin:x:1001:1001:System Administrator,,,,:/home/sysadmin:/bin/bash
```

La opción -v invierte la coincidencia, generando todas las líneas que no contienen el patrón. Para mostrar todas las líneas que no contengan nologin en el archivo /etc/passwd:

```
sysadmin@localhost:~$ grep -v nologin /etc/passwd
root:x:0:0:root:/root:/bin/bash
sync:x:4:65534:sync:/bin:/bin/sync
operator:x:1000:37::/root:/bin/sh
sysadmin:x:1001:1001:System Administrator,,,,:/home/sysadmin:/bin/bash
```

La opción -i ignora las distinciones entre mayúsculas y minúsculas. A continuación se busca el patrón en newhome.txt, lo que permite que cada carácter esté en mayúsculas o minúsculas:

```
sysadmin@localhost:~$ cd Documents
sysadmin@localhost:~/Documents$ grep -i the newhome.txt
There are three bathrooms.
**Beware** of the ghost in the bedroom.
The kitchen is open for entertaining.
**Caution** the spirits don't like guests.
```

La opción -w solo devuelve líneas que contienen coincidencias que forman palabras completas. Para ser una palabra, la cadena de caracteres debe ir precedida y seguida de un carácter que no sea una palabra. Los caracteres de palabra incluyen letras, dígitos y el carácter de subrayado.

En los ejemplos siguientes se busca el patrón are en el archivo newhome.txt. El primer comando busca sin opciones, mientras que el segundo comando incluye la opción -w. Compare los resultados:

```
sysadmin@localhost:~/Documents$ grep are newhome.txt
There are three bathrooms.
**Beware** of the ghost in the bedroom.
sysadmin@localhost:~/Documents$ grep -w are newhome.txt
There are three bathrooms.
```
## 10.8 Expresiones regulares básicas
_Las expresiones regulares_, también conocidas como _expresiones regulares_, son una colección de  caracteres _normales_ y _especiales_ que se utilizan para encontrar patrones simples o complejos, respectivamente, en los archivos. Estos caracteres son caracteres que se utilizan para realizar una función de coincidencia determinada en una búsqueda.

_Los caracteres normales_ son caracteres alfanuméricos que coinciden entre sí. Por ejemplo, una a coincidiría con una a. _Los caracteres especiales_ tienen significados especiales cuando se usan dentro de patrones mediante comandos como el comando grep. Se comportan de una manera más compleja y no coinciden consigo mismos.

 _Hay expresiones regulares básicas_ (disponibles para una amplia variedad de comandos de Linux) y _expresiones regulares extendidas_ (disponibles para comandos de Linux más avanzados). Las expresiones regulares básicas incluyen lo siguiente:

|**Carácter**|**Partidos**|
|---|---|
|.|Cualquier carácter individual|
|[ ]|Una lista o rango de caracteres para que coincida con un carácter<br><br>Si el primer carácter entre corchetes es el símbolo de intercalación ^, significa cualquier carácter _que no esté_ en la lista|
|*|El carácter anterior se repitió cero o más veces|
|^|Si es el primer carácter del patrón, el patrón debe estar al principio de la línea para que coincida, de lo contrario, solo un carácter ^ literal|
|$|Si es el último carácter del patrón, el patrón debe estar al final de la línea para que coincida, de lo contrario, solo un carácter $ literal|

El comando grep es solo uno de los muchos comandos que admiten expresiones regulares. Algunos otros comandos incluyen los comandos more y less.

> Si bien algunas de las expresiones regulares se entrecomillan innecesariamente con comillas simples, es una buena práctica usar comillas simples alrededor de las expresiones regulares para evitar que el shell intente interpretar un significado especial de ellas.
### 10.8.1 El período . Carácter
Una de las expresiones más útiles es el punto . carácter. Coincide con cualquier carácter, excepto con el carácter de nueva línea. Considere el contenido sin filtrar del archivo ~/Documents/red.txt:

```
sysadmin@localhost:~/Documents$ cat red.txt
red
reef
rot
reeed
rd
rod
roof
reed
root
reel
read 
```

El patrón r.. f encontraría cualquier línea que contuviera la letra r seguida exactamente de dos caracteres y luego la letra f:

```
sysadmin@localhost:~/Documents$ grep 'r..f' red.txt
reef
roof
```
La línea no tiene que ser una coincidencia exacta, simplemente debe contener el patrón, como se ve aquí cuando son. t se busca en el archivo /etc/passwd:

```
sysadmin@localhost:~/Documents$ grep 'r..t' /etc/passwd
root:x:0:0:root:/root:/bin/bash
operator:x:1000:37::/root:
```
El carácter de punto se puede utilizar cualquier número de veces. Para buscar todas las palabras que tengan al menos cuatro caracteres, se puede usar el siguiente patrón:

```
sysadmin@localhost:~/Documents$ grep '....' red.txt
reef
reeed
roof
reed
root
reel
read
```
### 10.8.2 Los caracteres de corchetes [ ]
Al utilizar el carácter ., _cualquier_ carácter posible podría coincidir con él. En algunos casos, desea especificar exactamente qué caracteres desea que coincidan, como un carácter del alfabeto en minúsculas o un carácter numérico.

Los corchetes [ ] coinciden con un solo carácter de la lista o rango de caracteres posibles contenidos entre corchetes. Por ejemplo, dado el archivo profile.txt:

```
sysadmin@localhost:~/Documents$ cat profile.txt
Hello my name is Joe.
I am 37 years old.
3121991
My favorite food is avocados.
I have 2 dogs.
123456789101112
```
Para encontrar todas las líneas de profile.txt que tienen un número, use el patrón [0123456789] o [0-9]:

```
sysadmin@localhost:~/Documents$ grep '[0-9]' profile.txt
I am 37 years old.
3121991
I have 2 dogs.
123456789101112
```

Tenga en cuenta que cada carácter posible se puede enumerar [abcd] o proporcionar como un rango [a-d], siempre que el rango esté en el orden correcto. Por ejemplo, [d-a] no funcionaría porque no es un rango válido:

```
sysadmin@localhost:~/Documents$ grep '[d-a]' profile.txt
grep: Invalid range end
```

El rango se especifica mediante un estándar llamado tabla ASCII. Esta tabla es una colección de todos los caracteres imprimibles en un orden específico. Puede ver la tabla ASCII con el comando ascii. Una pequeña muestra:

```
      041  33  21  !                                 141   97  61  a 
      042  34  22  “                                 142   98  62  b
      043  35  23  #                                 143   99  63  c
      044  36  24  $                                 144   100 64  d
      045  37  25  %                                 145   101 65  e
      046  38  26  &                                 146   102 66  f
```

El valor ASCII de la letra a es 97, mientras que el valor de d es 100. Dado que 97 es menor que 100, el rango a-d (97-100) es un rango válido.

¿Qué pasa con la eximencia de caracteres?, por ejemplo, para que coincida con un carácter que puede ser cualquier cosa excepto x, y o z? Sería ineficaz proporcionar un conjunto con todos los caracteres excepto x, y o z.

Para que coincida con un carácter que _no sea_  uno de los caracteres enumerados, inicie el conjunto con un símbolo ^. Para encontrar todas las líneas que contienen caracteres no numéricos, inserte un ^ como primer carácter dentro de los corchetes. Este carácter _niega_ los caracteres enumerados:

```
sysadmin@localhost:~/Documents$ grep '[^0-9]' profile.txt
Hello my name is Joe.
I am 37 years old.
My favorite food is avocados.
I have 2 dogs.
```

> No confunda [^0-9] para que coincida con líneas que _no contienen números_. De hecho, coincide con líneas que _no contienen números_. Mire el archivo original para ver la diferencia. La tercera y sexta línea solo contienen números; No contienen números que no sean números, por lo que esas líneas no coinciden.
### 10.8.3 El Asterisco * Personaje
El asterisco * se utiliza para hacer coincidir cero o más apariciones de un carácter o patrón que lo precede. Por ejemplo, e* coincidiría con cero o más apariciones de la letra e:

```
sysadmin@localhost:~/Documents$ cat red.txt
red
reef
rot
reeed
rd
rod
roof
reed
root
reel
read
sysadmin@localhost:~/Documents$ grep 're*d' red.txt
red
reeed
rd
reed
```

También es posible hacer coincidir cero o más apariciones de una lista de caracteres utilizando los corchetes. El patrón [oe]* utilizado en el ejemplo siguiente coincide con cero o más apariciones del carácter o del carácter e:

```
sysadmin@localhost:~/Documents$ grep 'r[oe]*d' red.txt
red
reeed
rd
rod
reed
```

Cuando se usa con un solo carácter, * no es muy útil. Cualquiera de los siguientes patrones coincidiría con todas las cadenas o líneas del archivo: '.*' 'e*' 'b*' 'z*' porque el carácter de asterisco * puede coincidir con cero apariciones de un patrón.

```
sysadmin@localhost:~/Documents$ grep 'z*' red.txt
red
reef
rot
reeed
rd
rod
roof
reed
root
reel
read
```

```
sysadmin@localhost:~/Documents$ grep 'e*' red.txt
red
reef
rot
reeed
rd
rod
roof
reed
root
reel
read
```

Para que el carácter de asterisco sea útil, es necesario crear un patrón que incluya más de un carácter que lo preceda. Por ejemplo, los resultados anteriores se pueden refinar agregando otra e para hacer que el patrón ee* coincida efectivamente con cada línea que contenga al menos una e.

```
sysadmin@localhost:~/Documents$ grep 'ee*' red.txt
red
reef
reeed
reed
reel
read
```
### 10.8.4 Caracteres de anclaje
Al realizar una coincidencia de patrón, la coincidencia podría ocurrir en cualquier parte de la línea. _Los caracteres de anclaje_ son una de las formas en que se pueden usar las expresiones regulares para restringir los resultados de búsqueda. Especifican si la coincidencia se produce al principio o al final de la línea.

Por ejemplo, la raíz del patrón aparece muchas veces en el archivo /etc/passwd:

```
sysadmin@localhost:~/Documents$ grep 'root' /etc/passwd
root:x:0:0:root:/root:/bin/bash
operator:x:1000:37::/root:
```

El carácter de intercalación (circunflejo) ^ se utiliza para garantizar que aparezca un patrón al principio de la línea. Por ejemplo, para encontrar todas las líneas en /etc/passwd que comienzan con root, use el patrón ^root. Tenga en cuenta que ^ debe ser el _primer_ carácter del patrón para que sea efectivo:

```
sysadmin@localhost:~/Documents$ grep '^root' /etc/passwd
root:x:0:0:root:/root:/bin/bash
```

El segundo carácter de anclaje $ se puede utilizar para garantizar que aparezca un patrón al final de la línea, lo que reduce efectivamente los resultados de la búsqueda. Para encontrar las líneas que terminan con r en el archivo alpha-first.txt, usa el patrón r$:

```
sysadmin@localhost:~/Documents$ cat alpha-first.txt
A is for Animal
B is for Bear
C is for Cat
D is for Dog
E is for Elephant
F is for Flower
sysadmin@localhost:~/Documents$ grep 'r$' alpha-first.txt
B is for Bear
F is for Flower
```

De nuevo, la posición de este personaje es importante. El $ debe ser el _último_ carácter del patrón para que sea efectivo como anclaje.
### 10.8.5 La barra invertida \ Carácter
En algunos casos, es posible que desee hacer coincidir un carácter que resulta ser un carácter de expresión regular especial. Por ejemplo, considere lo siguiente:

```
sysadmin@localhost:~/Documents$ cat newhome.txt
Thanks for purchasing your new home!!

**Warning** it may be haunted.

There are three bathrooms.

**Beware** of the ghost in the bedroom.

The kitchen is open for entertaining.

**Caution** the spirits don't like guests.

Good luck!!!
sysadmin@localhost:~/Documents$ grep 're*' newhome.txt
Thanks for purchasing your new home!!
**Warning** it may be haunted.
There are three bathrooms.
**Beware** of the ghost in the bedroom.
The kitchen is open for entertaining.
**Caution** the spirits don't like guests.
```

En la salida del comando grep anterior, la búsqueda de re* coincidía con cada línea que contenía una r seguida de cero o más de la letra e. Para buscar un carácter de asterisco * real, coloque un carácter de barra diagonal inversa \ antes del carácter de asterisco *:

```
sysadmin@localhost:~/Documents$ grep 're\*' newhome.txt
**Beware** of the ghost in the bedroom.
```
### 10.8.6 Expresiones regulares extendidas
El uso de expresiones regulares extendidas a menudo requiere que se proporcione una opción especial al comando para reconocerlas. Históricamente, hay un comando llamado egrep, que es similar a grep, pero puede entender expresiones regulares extendidas. Ahora, el comando egrep está en desuso en favor del uso de grep con la opción -E.

Las siguientes expresiones regulares se consideran extendidas:

|**Carácter**|**Significado**|
|---|---|
|?|Coincide con el carácter anterior cero o una vez, por lo que es un carácter opcional|
|+|Coincide con el carácter anterior repetido una o más veces|
|\||Alternancia o como un operador "o" lógico|

Para que coincida con colo seguido de cero o un carácter u seguido de un carácter r:

```
sysadmin@localhost:~/Documents$ grep -E 'colou?r' spelling.txt
American English: Do you consider gray to be a color or a shade?
British English: Do you consider grey to be a colour or a shade?
```

Para que coincida con uno o más caracteres e:

```
sysadmin@localhost:~/Documents$ grep -E 'e+' red.txt
red
reef
reeed
reed
reel
read ]
```

Para que coincida con el gris o el gris:

```
sysadmin@localhost:~/Documents$ grep -E 'gray|grey' spelling.txt
American English: Do you consider gray to be a color or a shade?
British English: Do you consider grey to be a colour or a shade?
```