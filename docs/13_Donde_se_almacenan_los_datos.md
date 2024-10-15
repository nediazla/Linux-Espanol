# 13. Dónde se almacenan los datos

## 13.1 Introducción
‌⁠​​⁠
> Cuando la mayoría de las personas se refieren a Linux, en realidad se refieren a una combinación de software llamada GNU/Linux, que define el sistema operativo. GNU es el software libre que rodea al núcleo y proporciona equivalentes de código abierto de muchos comandos UNIX comunes. La parte Linux de esta combinación es el núcleo Linux, que es el núcleo del sistema operativo. El núcleo se carga en el momento del arranque y permanece en ejecución para administrar cada aspecto del sistema en funcionamiento.

Una implementación del núcleo Linux incluye muchos subsistemas que son parte del núcleo mismo y otros que pueden cargarse de manera modular cuando sea necesario. Las funciones clave del núcleo Linux incluyen una interfaz de llamada al sistema, administración de procesos, administración de memoria, sistemas de archivos virtuales, redes y controladores de dispositivos.

En resumen, a través de un shell, el núcleo acepta comandos del usuario y administra los procesos que ejecutan dichos comandos dándoles acceso a dispositivos como memoria, discos, interfaces de red, teclados, ratones, monitores y más.

Un sistema Linux típico tiene miles de archivos. El estándar de jerarquía de sistemas de archivos proporciona una guía para las distribuciones sobre cómo organizar estos archivos. Es importante comprender el papel del núcleo de Linux y cómo procesa y proporciona información sobre el sistema bajo los pseudosistemas de archivos /proc y /sys.

![](img/20241009111102.png)
## 13.2 Procesos
El kernel proporciona acceso a la información sobre los procesos activos a través de un _pseudo sistema de archivos_ que es visible bajo el directorio /proc. Los dispositivos de hardware están disponibles a través de archivos especiales en el directorio /dev, mientras que la información sobre esos dispositivos se puede encontrar en otro pseudo sistema de archivos en el directorio /sys.

Los pseudo sistemas de archivos parecen ser archivos reales en el disco, pero solo existen en la memoria. La mayoría de los pseudo sistemas de archivos, como /proc, están diseñados para parecer un árbol jerárquico fuera de la raíz del sistema de directorios, archivos y subdirectorios, pero en realidad solo existen en la memoria del sistema, y solo parecen residir en el dispositivo de almacenamiento en el que se encuentra el sistema de archivos raíz.

El directorio /proc no solo contiene información sobre los procesos en ejecución, como su nombre sugiere, sino que también contiene información sobre el hardware del sistema y la configuración actual del kernel.

El directorio /proc es leído, y su información utilizada por muchos comandos diferentes en el sistema, incluyendo pero no limitado a top, free, mount, umount y muchos otros. Rara vez es necesario que un usuario extraiga el directorio /proc directamente, ya que es más fácil usar los comandos que utilizan su información.

Vea un ejemplo de salida a continuación:

```
sysadmin@localhost:~$ ls /proc
1          cpuinfo      irq          modules       sys
128        crypto       kallsyms     mounts        sysrq-trigger
17         devices      kcore        mtrr          sysvipc
21         diskstats    key-users    net           thread-self 
23         dma          keys         pagetypeinfo  timer_list
39         driver       kmsg         partitions    timer_stats
60         execdomains  kpagecgroup  sched_debug   tty
72         fb           kpagecount   schedstat     uptime
acpi       filesystems  kpageflags   scsi          version
buddyinfo  fs           loadavg      self          version_signature
bus        interrupts   locks        slabinfo      vmallocinfo
cgroups    iomem        mdstat       softirqs      vmstat
cmdline    ioports      meminfo      stat          zoneinfo
consoles   ipmi         misc         swaps
```

La salida muestra una variedad de directorios con nombre y numerados. Hay un directorio numerado para cada proceso en ejecución en el sistema, donde el nombre del directorio coincide con el ID de _proceso (PID)_ para el proceso en ejecución.

Por ejemplo, los números 72 denotan PID 72, un programa en ejecución, que está representado por un directorio del mismo nombre, que contiene muchos archivos y subdirectorios que describen ese proceso en ejecución, su configuración, uso de memoria y muchos otros elementos.

En un sistema Linux en ejecución, siempre hay un ID de proceso o PID 1.

También hay una serie de archivos regulares en el directorio /proc que proporcionan información sobre el kernel en ejecución:

|**Archivo**|**Contenido**|
|---|---|
|/proc/cmdline|Información que se pasó al kernel cuando se inició por primera vez, como parámetros de línea de comandos e instrucciones especiales|
|/proc/meminfo|Información sobre el uso de la memoria por parte del kernel|
|/proc/módulos|Una lista de módulos cargados actualmente en el kernel para agregar funcionalidad adicional|

Una vez más, rara vez es necesario ver estos archivos directamente, ya que otros comandos ofrecen una salida más fácil de usar y una forma alternativa de ver esta información.

Mientras que la mayoría de los "archivos" debajo del directorio /proc no pueden ser modificados, incluso por el usuario root, los "archivos" debajo del directorio /proc/sys están potencialmente destinados a ser cambiados por el usuario root. La modificación de estos archivos cambia el comportamiento del kernel de Linux.

La modificación directa de estos archivos solo provoca cambios temporales en el núcleo. Para hacer que los cambios sean permanentes (persistentes incluso después de reiniciar), se pueden agregar entradas a la sección apropiada del archivo /etc/sysctl.conf.

Por ejemplo, el directorio /proc/sys/net/ipv4 contiene un archivo denominado icmp_echo_ignore_all. Si ese archivo contiene un carácter cero 0, como lo hace normalmente, entonces el sistema responderá a las solicitudes icmp. Si ese archivo contiene un carácter 1, entonces el sistema no responderá a las solicitudes icmp:

```
sysadmin@localhost:~$ su -
Password: 
root@localhost:~# cat /proc/sys/net/ipv4/icmp_echo_ignore_all 
0
root@localhost:~# ping -c1 localhost
PING localhost.localdomain (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost.localdomain (127.0.0.1): icmp_seq=1 ttl=64 time=0.026 ms
 
--- localhost.localdomain ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.026/0.026/0.026/0.000 ms
root@localhost:~# echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all
root@localhost:~# ping -c1 localhost
PING localhost.localdomain (127.0.0.1) 56(84) bytes of data.
 
--- localhost.localdomain ping statistics ---
1 packets transmitted, 0 received, 100% packet loss, time 10000ms
```
### 13.2.1 Jerarquía de procesos
Cuando el kernel termina de cargarse durante el procedimiento de arranque, inicia el  proceso de inicio y le asigna un PID de 1. A continuación, este proceso inicia otros procesos del sistema, y a cada proceso se le asigna un PID en orden secuencial.

> En un  **sistema basado en System V**, el proceso de inicio sería el programa /sbin/init. En un  sistema basado en **systemd**, el archivo /bin/systemd se ejecuta normalmente, pero casi siempre es un enlace al ejecutable /lib/system/systemd.

> Independientemente del tipo de proceso de inicio del sistema que se esté ejecutando, la información sobre el proceso se puede encontrar en el directorio /proc/1.

A medida que cualquiera de los procesos de inicio pone en marcha otros procesos, ellos, a su vez, pueden iniciar procesos, que pueden poner en marcha otros procesos, y así sucesivamente. Cuando un proceso inicia otro proceso, el proceso que realiza el inicio se denomina _proceso principal_ y el proceso que se inicia se denomina _proceso secundario_. Al ver procesos, el PID principal se etiqueta como PPID.

Cuando el sistema ha estado funcionando durante mucho tiempo, puede llegar a alcanzar el _valor PID máximo_, que se puede ver y configurar a través del archivo /proc/sys/kernel/pid_max. Una vez que se ha utilizado el PID más grande, el sistema se "revierte" y continúa sin problemas asignando valores PID que están disponibles en la parte inferior del rango.

Los procesos se pueden "mapear" en un árbol genealógico de acoplamientos padre e hijo. Si desea ver este árbol, el comando pstree lo muestra:

```
sysadmin@localhost:~$ pstree
init-+-cron
‌⁠​​⁠​ 
     |-login---bash---pstree
     |-named---18*[{named}]
     |-rsyslogd---2*[{rsyslogd}]
     `-sshd
```

Si tuviera que examinar la relación de los procesos padre y secundario utilizando la salida del comando anterior, podría describirse de la siguiente manera:

- init es el padre de login
- login es el hijo de init

- login es el padre de bash
- bash es el hijo de login

- bash es el padre de pstree
- pstree es el hijo de bash
### 13.2.2 Visualización de la instantánea del proceso
Otra forma de ver los procesos es con el  comando `ps`. De forma predeterminada, el  `comando ps` solo muestra los procesos actuales que se ejecutan en el shell actual. Irónicamente, aunque esté tratando de obtener información sobre los procesos, el  comando `ps` se incluye en la salida:

```
sysadmin@localhost:~$ ps
  PID TTY          TIME CMD
 6054 ?        00:00:00 bash
 6070 ?        00:00:01 xeyes
 6090 ?        00:00:01 firefox
 6146 ?        00:00:00 ps
```

Si ejecuta `ps` con la opción `--forest`, entonces, de manera similar al  `comando pstree`, muestra líneas que indican la relación padre y secundario:

```
sysadmin@localhost:~$ ps --forest
  PID TTY          TIME CMD
‌⁠​​⁠​ 
 6054 ?        00:00:00 bash
 6090 ?        00:00:02   \_ firefox
 6180 ?        00:00:00   \_ dash
 6181 ?        00:00:00        \_ xeyes
 6188 ?        00:00:00        \_ ps
```

Para poder ver todos los procesos en el sistema, ejecute el  comando `ps aux` o el  comando `ps -ef`:

```
sysadmin@localhost:~$ ps aux | head
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0  17872  2892 ?        Ss   08:06   0:00 /sbin?? /init
syslog      17  0.0  0.0 175744  2768 ?        Sl   08:06   0:00 /usr/sbin/rsyslogd -c5
root        21  0.0  0.0  19124  2092 ?        Ss   08:06   0:00 /usr/sbin/cron
root        23  0.0  0.0  50048  3460 ?        Ss   08:06   0:00 /usr/sbin/sshd
bind        39  0.0  0.0 385988 19888 ?        Ssl  08:06   0:00 /usr/sbin/named -u bind
root        48  0.0  0.0  54464  2680 ?        S    08:06   0:00 /bin/login -f
sysadmin    60  0.0  0.0  18088  3260 ?        S    08:06   0:00 -bash
sysadmin   122  0.0  0.0  15288  2164 ?        R+   16:26   0:00 ps aux
sysadmin   123  0.0  0.0  18088   496 ?        D+   16:26   0:00 -bash
```

```
sysadmin@localhost:~$ ps -ef | head
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 08:06 ?        00:00:00 /sbin?? /init
syslog      17     1  0 08:06 ?        00:00:00 /usr/sbin/rsyslogd -c5
root        21     1  0 08:06 ?        00:00:00 /usr/sbin/cron
root        23     1  0 08:06 ?        00:00:00 /usr/sbin/sshd
bind        39     1  0 08:06 ?        00:00:00 /usr/sbin/named -u bind
root        48     1  0 08:06 ?        00:00:00 /bin/login -f
sysadmin    60    48  0 08:06 ?        00:00:00 -bash
sysadmin   124    60  0 16:46 ?        00:00:00 ps -ef
sysadmin   125    60  0 16:46 ?        00:00:00 head
```

La salida de todos los procesos que se ejecutan en un sistema puede ser abrumadora. En los ejemplos anteriores, la salida del  `comando ps` fue filtrada por el  comando `head`, por lo que solo se mostraron los primeros diez procesos. Si no filtra la salida del  comando `ps`, es probable que tenga que desplazarse por cientos de procesos para encontrar lo que podría interesarle.

Una forma común de reducir el número de líneas de salida que el usuario podría tener que ordenar es utilizar el  `comando grep` para filtrar las líneas de visualización de salida que coinciden con una palabra clave, como el nombre de un proceso. Por ejemplo, para ver solo información sobre el  `proceso de firefox`, ejecute un comando como:

```
sysadmin@localhost:~$ ps -ef | grep firefox
 6090 pts/0    00:00:07 firefox
```

Enviar la salida a un localizador como el  comando `less` también puede hacer que la salida del  comando `ps` sea  más manejable.

Un administrador puede estar más preocupado por los procesos de otro usuario. Hay varios estilos de opciones que admite el  `comando ps`, lo que da como resultado diferentes formas de ver los procesos de un usuario individual. Para usar la opción tradicional de UNIX para ver los procesos de un usuario específico, use la  `opción -u`:

```
sysadmin@localhost:~$ ps -u root
  PID TTY          TIME CMD
    1 ?        00:00:00 init
   13 ?        00:00:00 cron
   15 ?        00:00:00 sshd
   43 ?        00:00:00 login
```
### 13.2.3 Visualización de procesos en tiempo real
Mientras que el comando ps proporciona una instantánea de los procesos que se ejecutan en el instante en que se ejecuta el comando, el comando top tiene una interfaz dinámica basada en pantalla que actualiza regularmente la salida de los procesos en ejecución. El comando top se ejecuta de la siguiente manera:

```
sysadmin@localhost:~$ top
```

De forma predeterminada, la salida del comando top se ordena por el porcentaje % de tiempo de CPU que cada proceso está utilizando actualmente, con los valores más altos enumerados primero, lo que significa que los procesos más intensivos de CPU se enumeran primero:

```
top - 00:26:56 up 28 days, 20:53,  1 user,  load average: 0.11, 0.15, 0.17
Tasks:   8 total,   1 running,   7 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.2 us,  0.2 sy,  0.0 ni, 99.6 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem : 13201464+total, 76979904 free, 47522152 used,  7512580 buff/cache
KiB Swap: 13419622+total, 13415368+free,    42544 used. 83867456 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
    1 root      20   0   18376   3036   2764 S   0.0  0.0   0:00.12 init
    9 syslog    20   0  191328   5648   3100 S   0.0  0.0   0:00.04 rsyslogd
   13 root      20   0   28356   2552   2320 S   0.0  0.0   0:00.00 cron
   15 root      20   0   72296   3228   2484 S   0.0  0.0   0:00.00 sshd
   24 bind      20   0  878888  39324   7148 S   0.0  0.0   0:02.72 named
   43 root      20   0   78636   3612   3060 S   0.0  0.0   0:00.00 login
   56 sysadmin  20   0   18508   3440   3040 S   0.0  0.0   0:00.00 bash
   72 sysadmin  20   0   36600   3132   2696 R   0.0  0.0   0:00.03 top
```

Hay una gran cantidad de comandos interactivos que se pueden ejecutar desde el programa running top. Utilice la tecla **H** para ver una lista completa.

Una de las ventajas del comando superior es que se puede dejar en ejecución para estar al _tanto_ de los procesos con fines de monitoreo. Si un proceso comienza a dominar, o a huir con el sistema, entonces por defecto aparecerá en la parte superior de la lista presentada por el comando superior. A continuación, un administrador que esté ejecutando el comando top puede realizar una de estas dos acciones:

|**Llave**|**Acción**|
|---|---|
|**K**|_Finalice_ el proceso de fuga.|
|**R**|_Ajuste la prioridad_ del proceso.|

Al presionar la tecla **K** mientras se ejecuta el comando superior, se le pedirá al usuario que proporcione el PID y luego un número de señal. El envío de la señal predeterminada _solicita_ que el proceso finalice, pero el envío de la señal número 9, la señal KILL, _obliga_ a que el proceso finalice.

Al presionar la tecla **R** mientras se ejecuta el comando superior, se solicitará al usuario que el proceso se _renice_ y, a continuación, un valor de niceness. Los valores de amabilidad pueden oscilar entre -20 y 19 y afectar a la prioridad. Solo el usuario root puede usar un valor de amabilidad que sea un número menor que el actual, o un valor de amabilidad negativo, lo que hace que el proceso se ejecute con una mayor prioridad. Cualquier usuario puede proporcionar un valor de amabilidad que sea mayor que el valor de amabilidad actual, lo que hace que el proceso se ejecute con una prioridad más baja.

Otra ventaja del comando superior es que puede proporcionar una representación general de lo ocupado que está el sistema actualmente y la tendencia a lo largo del tiempo.

```
top - 00:26:56 up 28 days, 20:53,  1 user,  load average: 0.11, 0.15, 0.17
Tasks:   8 total,   1 running,   7 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.2 us,  0.2 sy,  0.0 ni, 99.6 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem : 13201464+total, 76979904 free, 47522152 used,  7512580 buff/cache
KiB Swap: 13419622+total, 13415368+free,    42544 used. 83867456 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
    1 root      20   0   18376   3036   2764 S   0.0  0.0   0:00.12 init
    9 syslog    20   0  191328   5648   3100 S   0.0  0.0   0:00.04 rsyslogd
   13 root      20   0   28356   2552   2320 S   0.0  0.0   0:00.00 cron
   15 root      20   0   72296   3228   2484 S   0.0  0.0   0:00.00 sshd
   24 bind      20   0  878888  39324   7148 S   0.0  0.0   0:02.72 named
   43 root      20   0   78636   3612   3060 S   0.0  0.0   0:00.00 login
   56 sysadmin  20   0   18508   3440   3040 S   0.0  0.0   0:00.00 bash
   72 sysadmin  20   0   36600   3132   2696 R   0.0  0.0   0:00.03 top
```

Los _promedios de carga_ que se muestran en la primera línea de salida del comando superior indican qué tan ocupado ha estado el sistema durante los últimos uno, cinco y quince minutos. Esta información también se puede ver ejecutando el comando uptime o directamente mostrando el contenido del archivo /proc/loadavg:

> Para salir del programa superior y volver al símbolo del sistema, escriba **q**.

```
sysadmin@localhost:~$ cat /proc/loadavg
0.12 0.46 0.25 1/254 3052
```

- **Promedio de carga:**

![](img/20241009112238.png)

Los tres primeros números de este archivo indican el promedio de carga en los últimos intervalos de uno, cinco y quince minutos.

- **Número de procesos:**

![](img/20241009112251.png)

El cuarto valor es una fracción que muestra el número de procesos que se están ejecutando actualmente en código en la CPU 1 y el número total de procesos 254.

- **Último PID:**

![](img/20241009112305.png)

El quinto valor es el último valor PID que ejecutó código en la CPU.

El número notificado como media de carga es proporcional al número de núcleos de CPU que pueden ejecutar procesos. En una CPU de un solo núcleo, un valor de uno (1) significaría que el sistema está completamente cargado. En una CPU de cuatro núcleos, un valor de 1 significaría que el sistema está solo cargado entre 1/4 y 25%.

Otra razón por la que a los administradores les gusta mantener el comando superior en ejecución es la capacidad de monitorear el uso de la memoria en tiempo real. Tanto el comando superior como el free muestran estadísticas sobre cómo se utiliza la memoria general.

El comando top también puede mostrar el porcentaje de memoria utilizada por cada proceso, por lo que un proceso que está consumiendo una cantidad excesiva de memoria se puede identificar rápidamente.
## 13.3 Memoria
La memoria en un sistema Linux moderno está gobernada y administrada por el kernel. La memoria de hardware del sistema es compartida por todos los procesos del sistema, a través de un método llamado _direccionamiento virtual_. La memoria física puede ser referenciada por una serie de procesos, cualquiera de los cuales puede pensar que es capaz de abordar más memoria de la que realmente puede. El direccionamiento virtual permite que muchos procesos accedan a la misma memoria sin conflictos ni bloqueos. Lo hace asignando ciertas áreas de un disco duro físico (o virtual) para que se utilicen en lugar de la RAM física. La memoria se divide en _bloques_ de unidades del mismo tamaño que se pueden direccionar como cualquier otro recurso del sistema. El sistema no solo puede acceder a la memoria desde las direcciones del sistema local, sino que también puede acceder a la memoria que se encuentra en otro lugar, como en una computadora diferente, un dispositivo virtual o incluso en un volumen que se encuentra físicamente en otro continente.

Si bien una revisión detallada del direccionamiento de memoria de Linux está más allá del alcance de este curso, es importante tener en cuenta la diferencia entre el espacio de _usuario_ y  el _espacio del kernel_. El espacio del kernel es donde se almacena y ejecuta el código para el kernel. Esto se encuentra generalmente en un rango "protegido" de direcciones de memoria y permanece aislado de otros procesos con privilegios más bajos. El espacio de usuario, por otro lado, está disponible para usuarios y programas. Se comunican con el Kernel a través de API de "llamada al sistema" que actúan como intermediarios entre los programas regulares y el Kernel. Este sistema de separar los programas potencialmente inestables o maliciosos del trabajo crítico del Kernel es lo que da a los sistemas Linux la estabilidad y la resistencia en las que confían los desarrolladores de aplicaciones.
### 13.3.1 Memoria de visualización
La ejecución del comando free sin ninguna opción proporciona una instantánea de la memoria que se está utilizando en ese momento.

```
sysadmin@localhost:~$ free                                                      
             total       used       free     shared    buffers     cached       
Mem:      32953528   26171772    6781756          0       4136   22660364    
-/+ buffers/cache:    3507272   29446256                                        
Swap:            0          0          0  sysadmin@localhost:~$ free                                                      
             total       used       free     shared    buffers     cached       
Mem:      32953528   26171772    6781756          0       4136   22660364    
-/+ buffers/cache:    3507272   29446256                                        
Swap:            0          0          0  
```

Si desea supervisar el uso de la memoria a lo largo del tiempo con el comando free, puede ejecutarlo con la opción -s (con qué frecuencia actualizar) y especificar ese número de segundos. Por ejemplo, si se ejecuta el siguiente comando gratuito, se actualizaría la salida cada diez segundos:

```
sysadmin@localhost:~$ free -s 10
              total        used        free      shared  buff/cache   available
Mem:      132014640    47304084    77189512        3008     7521044    84085528
Swap:     134196220       42544   134153676

              total        used        free      shared  buff/cache   available
Mem:      132014640    47302928    77190668        3008     7521044    84086684
Swap:     134196220       42544   134153676
```

Para facilitar la interpretación de lo que genera el comando free, las opciones -m o -g pueden ser útiles mostrando la salida en megabytes o gigabytes, respectivamente. Sin estas opciones, la salida se muestra en bytes:

Al leer la salida del comando libre:

-  **Encabezado descriptivo:**
![](img/20241009112522.png)
- **Estadísticas de memoria física:**
![](img/20241009112537.png)
- **Ajuste de memoria:**
La tercera línea representa la cantidad de memoria física después de ajustar esos valores sin tener en cuenta ninguna memoria que esté en uso por el kernel para búferes y cachés. Técnicamente, esta memoria "usada" podría ser "recuperada" si fuera necesario:

![](img/20241009112602.png)
- **Memoria de intercambio:**
La cuarta línea de salida se refiere a la memoria de _intercambio_, también conocida como memoria virtual. Este es el espacio en el disco duro que se utiliza como memoria física cuando la cantidad de memoria física se vuelve baja. Efectivamente, esto hace que parezca que el sistema tiene más memoria de la que tiene, pero el uso del espacio de intercambio también puede ralentizar el sistema:

![](img/20241009112620.png)

Si la cantidad de memoria e intercambio disponible se vuelve muy baja, el sistema comenzará a terminar automáticamente los procesos, por lo que es fundamental monitorear el uso de memoria del sistema. Un administrador que note que el sistema se está quedando sin memoria libre puede usar top o kill para terminar los procesos de su propia elección, en lugar de dejar que el sistema elija.
## 13.4 Archivos de registro
A medida que el kernel y varios procesos se ejecutan en el sistema, producen una salida que describe cómo se están ejecutando. Algunos de estos resultados se muestran como salida estándar y error en la ventana del terminal donde se ejecutó el proceso, aunque algunos de estos datos no se envían a la pantalla. En su lugar, se escribe en varios archivos. Esta información se denomina _datos de registro_ o _mensajes de registro_.

_Los archivos de registro_ son útiles por muchas razones: ayudan a solucionar problemas y a determinar si se ha intentado o no un acceso no autorizado.

Algunos procesos pueden registrar sus propios datos en estos archivos, otros procesos dependen de un proceso separado (un _demonio_) para manejar estos archivos de datos de registro.

> _Syslog_ es el término que se utiliza casi genéricamente para describir el registro en los sistemas Linux, ya que ha estado en vigor durante bastante tiempo. En algunos casos, cuando un autor dice _syslog,_  lo que realmente quiere decir es _cualquier sistema de registro que esté actualmente en uso en esta distribución_.

Los demonios de registro difieren en dos formas principales en las distribuciones recientes. El método más antiguo para realizar el registro del sistema es dos demonios (llamados syslogd y klogd) trabajando juntos, pero en distribuciones más recientes, un solo servicio llamado rsyslogd combina estas dos funciones y más en un solo demonio.

En distribuciones aún más recientes, las basadas en systemd, el demonio de registro se denomina journald, y los registros están diseñados para permitir principalmente la salida de texto, pero también binaria. El método estándar para ver registros basados en journald es usar el comando journalctl.

Independientemente del proceso de demonio que se utilice, los propios archivos de registro casi siempre se colocan en la estructura de directorios /var/log. Aunque algunos de los nombres de archivo pueden variar, estos son algunos de los archivos más comunes que se pueden encontrar en este directorio:

|**Archivo**|**Contenido**|
|---|---|
|boot.log|Los mensajes generados como servicios se inician durante el inicio del sistema.|
|cron|Mensajes generados por el demonio crond para que los trabajos se ejecuten de forma recurrente.|
|DMESG|Mensajes generados por el kernel durante el arranque del sistema.|
|registro de correo|Mensajes generados por el demonio de correo para los mensajes de correo electrónico enviados o recibidos.|
|Mensajes|Mensajes del kernel y otros procesos que no pertenecen a otro lugar. A veces se denomina syslog en lugar de messages después del demonio que escribe este archivo.|
|seguro|Mensajes de procesos que requerían autorización o autenticación (como el proceso de inicio de sesión).|
|diario|Mensajes de la configuración predeterminada de systemd-journald.service; se puede configurar en el archivo /etc/journald.conf entre otros lugares.|
|Xorg.0.log|Mensajes del servidor X Windows (GUI).|

Puede ver el contenido de varios archivos de registro mediante dos métodos diferentes. En primer lugar, como con la mayoría de los otros archivos, puede usar el comando cat o el comando less para permitir la búsqueda, el desplazamiento y otras opciones.

El segundo método es usar el comando journalctl en sistemas basados en systemd, principalmente porque el archivo /var/log/journal ahora a menudo contiene información binaria y el uso de los comandos cat o menos puede producir un comportamiento de pantalla confuso a partir de códigos de control y elementos binarios en los archivos de registro.

Los archivos de registro se _rotan_, lo que significa que se cambia el nombre de los archivos de registro más antiguos y se reemplazan por archivos de registro más nuevos. Los nombres de archivo que aparecen en la tabla anterior pueden tener un sufijo numérico o de fecha agregado: por ejemplo, secure.0 o secure-20181103.

La rotación de un archivo de registro suele producirse de forma programada: por ejemplo, una vez a la semana. Cuando se rota un archivo de registro, el sistema deja de escribir en el archivo de registro y le agrega un sufijo. A continuación, se crea un nuevo archivo con el nombre original y el proceso de registro continúa con este nuevo archivo.

Con la mayoría de los demonios modernos, se utiliza un sufijo de fecha. Por lo tanto, al final de la semana que finaliza el 3 de noviembre de 2018, el demonio de registro podría dejar de escribir en /var/log/messages (o /var/log/journal), cambiar el nombre de ese archivo a /var/log/messages-20181103 y, a continuación, comenzar a escribir en un nuevo archivo /var/log/messages.

Aunque la mayoría de los archivos de registro contienen texto como contenido, que se puede ver de forma segura con muchas herramientas, otros archivos como los archivos /var/log/btmp y /var/log/wtmp contienen binarios. Mediante el comando file, los usuarios pueden comprobar el tipo de contenido del archivo antes de verlo para asegurarse de que es seguro verlo. El siguiente comando de archivo clasifica /var/log/wtmp como datos, lo que generalmente significa que el archivo es binario:

```
sysadmin@localhost:~$ file /var/log/wtmp
/var/log/wtmp: data
```

Para los archivos que contienen datos binarios, hay comandos disponibles que leerán los archivos, interpretarán su contenido y, a continuación, generarán texto. Por ejemplo, los comandos lastb y last se pueden usar para ver los archivos /var/log/btmp y /var/log/wtmp respectivamente.

> El `lastb` requiere que se ejecuten privilegios de root en nuestro entorno de máquina virtual.
## 13.5 Mensajes del kernel
El archivo /var/log/dmesg contiene los mensajes del kernel que se produjeron durante el inicio del sistema. El archivo /var/log/messages contiene mensajes del kernel que se producen mientras el sistema se está ejecutando, pero esos mensajes se mezclan con otros mensajes de demonios o procesos.

Aunque normalmente el kernel no tiene su propio archivo de registro, se puede configurar uno modificando el archivo /etc/syslog.conf o el archivo /etc/rsyslog.conf. Además, el comando dmesg se puede utilizar para ver el _búfer de anillo del kernel_, que contiene un gran número de mensajes generados por el kernel. ⁠​​⁠​ 

En un sistema activo, o uno que experimenta muchos errores del kernel, es posible que se exceda la capacidad de este búfer y que se pierdan algunos mensajes. El tamaño de este búfer se establece en el momento en que se compila el kernel, por lo que no es trivial cambiarlo.

La ejecución del comando dmesg puede producir hasta 512 kilobytes de texto, por lo que se recomienda filtrar el comando con una tubería a otro comando como less o grep. Por ejemplo, si un usuario estaba solucionando problemas con un dispositivo USB, es útil buscar el texto USB con el comando grep. La opción -i se utiliza para ignorar las mayúsculas y minúsculas:

```
sysadmin@localhost:~$ dmesg | grep -i usb
usbcore: registered new interface driver usbfs
usbcore: registered new interface driver hub
usbcore: registered new device driver usb
ehci_hcd: USB 2.0 'Enhanced' Host Controller (EHCI) Driver
ohci_hcd: USB 1.1 'Open' Host Controller (OHCI) Driver
ohci_hcd 0000:00:06.0: new USB bus registered, assigned bus number 1
usb usb1: New USB device found, idVendor=1d6b, idProduct=0001
usb usb1: New USB device strings: Mfr=3, Product=2, SerialNumber=1
```
## 13.6 Estándar de jerarquía del sistema de archivos
Entre los estándares admitidos por la Fundación Linux se encuentra el **Estándar de Jerarquía del Sistema de Archivos (FHS),** que se aloja en la URL [http://www.pathname.com/fhs/](http://www.pathname.com/fhs/).

Una norma es un conjunto de reglas o directrices que se recomienda seguir. Sin embargo, estas pautas ciertamente pueden romperse, ya sea por distribuciones completas o por administradores en máquinas individuales.

El estándar FHS categoriza cada directorio del sistema de dos maneras:

- Un directorio se puede clasificar como _compartible_ o no, refiriéndose a si el directorio se puede compartir en una red y ser utilizado por varias máquinas.
- El directorio se coloca en una categoría de tener  archivos _estáticos_ (el contenido del archivo no cambiará) o  _archivos variables_ (el contenido del archivo puede cambiar).

Para hacer estas clasificaciones, a menudo es necesario referirse a los subdirectorios por debajo del nivel superior de los directorios. Por ejemplo, el directorio /var en sí no se puede clasificar como compartible o no compartible, pero uno de sus subdirectorios, el directorio /var/mail, se puede compartir. Por el contrario, el directorio /var/lock no debe poder compartirse.

|              | **No se puede compartir** | **Compartible** |
| ------------ | ------------------------- | --------------- |
| **Variable** | /var/bloqueo              | /var/correo     |
| **Estático** | /etc                      | /optar          |
![](img/20241009113018.png)

El estándar FHS define cuatro jerarquías de directorios utilizados en la organización de los archivos del sistema de archivos. La jerarquía de nivel superior o raíz es la siguiente:

|**Directorio**|**Contenido**|
|---|---|
|/|La base de la estructura, o raíz del sistema de archivos, este directorio unifica todos los directorios independientemente de si son particiones locales, dispositivos extraíbles o recursos compartidos de red|
|/bote|Binarios esenciales como los comandos ls, cp y rm, y ser parte del sistema de archivos raíz|
|/bota|Archivos necesarios para arrancar el sistema, como el kernel de Linux y los archivos de configuración asociados|
|/Dev|Archivos que representan dispositivos de hardware y otros archivos especiales, como los archivos /dev/null y /dev/zero|
|/etc|Archivos de configuración de host esenciales, como los archivos /etc/hosts o /etc/passwd|
|/hogar|Directorios de inicio de usuario|
|/Lib|Bibliotecas esenciales para admitir los archivos ejecutables en los directorios /bin y /sbin|
|/lib64|Bibliotecas esenciales creadas para una arquitectura específica. Por ejemplo, el directorio /lib64 para procesadores compatibles con AMD/Intel x86 de 64 bits|
|/Medio|Punto de montaje para medios extraíbles montados automáticamente|
|/mnt|Punto de montaje para el montaje manual temporal de sistemas de archivos|
|/optar|Ubicación de instalación de software de terceros opcional|
|/Proc|Sistema de archivos virtual para que el kernel reporte información del proceso, así como otra información|
|/raíz|Directorio principal del usuario root|
|/sbin|Binarios esenciales del sistema utilizados principalmente por el usuario root|
|/sys|Sistema de archivos virtual para información sobre los dispositivos de hardware conectados al sistema|
|/srv|Ubicación en la que se pueden alojar los servicios específicos del sitio|
|/Tmp|Directorio donde todos los usuarios pueden crear archivos temporales y que se supone que se borran en el momento del arranque (pero a menudo no es así)|
|/Usr|Segunda jerarquía<br><br>Archivos no esenciales para uso multiusuario|
|/usr/local|Tercera jerarquía<br><br>Archivos para software que no se originan en la distribución|
|/Var|Cuarta jerarquía<br><br>Archivos que cambian con el tiempo|
|/var/caché|Archivos utilizados para almacenar en caché los datos de la aplicación|
|/var/registro|La mayoría de los archivos de registro|
|/var/bloqueo|Bloquear archivos para recursos compartidos|
|/var/carrete|Archivos de cola para impresión y correo|
|/var/tmp|Archivos temporales que se conservarán entre reinicios|

La segunda y tercera jerarquías, ubicadas bajo los directorios /usr y /usr/local, repiten el patrón de muchos de los directorios de claves que se encuentran bajo la primera jerarquía o sistema de archivos raíz. La cuarta jerarquía, el directorio /var, también repite algunos de los directorios de nivel superior, como lib, opt y tmp.

Mientras que el sistema de archivos raíz y su contenido se consideran esenciales o necesarios para arrancar el sistema, los directorios /var, /usr y /usr/local se consideran no esenciales para el proceso de arranque. Como resultado, el sistema de archivos raíz y sus directorios pueden ser los únicos disponibles en ciertas situaciones, como el arranque en modo de usuario único, un entorno diseñado para solucionar problemas del sistema.

El directorio /usr está diseñado para contener software para uso de múltiples usuarios. El directorio /usr a veces se comparte a través de la red y se monta como de solo lectura.

La jerarquía /usr/local es para la instalación de software que no se origina con la distribución. A menudo, este directorio se utiliza para el software que se compila a partir del código fuente.
### 13.6.1 Organización dentro de la jerarquía del sistema de archivos
Aunque el estándar FHS es útil para una comprensión detallada del diseño de los directorios utilizados por la mayoría de las distribuciones de Linux, a continuación se proporciona una descripción más generalizada del diseño de los directorios tal como existen en una distribución típica de Linux.

Directorios de inicio de usuario

El directorio /home tiene un directorio debajo para cada cuenta de usuario. Por ejemplo, un usuario bob tendrá un directorio de inicio de /home/bob. Normalmente, solo el usuario bob tendrá acceso a este directorio. Sin que se le asignen permisos especiales en otros directorios, un usuario solo puede crear archivos en su directorio principal, el directorio /tmp y el directorio /var/tmp.

Directorios binarios

Los directorios binarios contienen los programas que los usuarios y administradores ejecutan para iniciar procesos o aplicaciones que se ejecutan en el sistema.

Binarios específicos del usuario

Los directorios binarios que están destinados a ser utilizados por usuarios sin privilegios incluyen:

- /bote
- /usr/bin
- /usr/local/bin

A veces, el software de terceros también almacena sus archivos ejecutables en directorios como:

- /usr/local/aplicacion/bin
- /opt/aplicación/papelera

Además, no es inusual que cada usuario tenga su propio directorio bin ubicado en su directorio de inicio; Por ejemplo, /home/bob/bin.

Binarios restringidos a la raíz

Por otro lado, los directorios sbin están destinados principalmente a ser utilizados por el administrador del sistema (el usuario root). Por lo general, estos incluyen:

- /sbin
- /usr/sbin
- /usr/local/sbin

Algunas aplicaciones administrativas de terceros también podrían usar directorios como:

- /usr/local/aplicacion/sbin
- /opt/aplicación/sbin

Dependiendo de la distribución, es posible que la variable PATH no contenga todos los directorios bin y sbin posibles. Para ejecutar un comando en uno de estos directorios, el directorio debe estar incluido en la lista de variables PATH, o el usuario debe especificar la ruta al comando.

Directorios de aplicaciones de software

A diferencia del sistema operativo Windows, donde las aplicaciones pueden tener todos sus archivos instalados en un solo subdirectorio en el directorio C:\Program Files, las aplicaciones en Linux pueden tener sus archivos en varios directorios repartidos por todo el sistema de archivos de Linux. Para distribuciones derivadas de Debian, puede ejecutar el comando dpkg -L nombrepaquete  para obtener la lista de ubicaciones de archivos. En las distribuciones derivadas de Red Hat, puede ejecutar el comando rpm -ql nombrepaquete  para la lista de las ubicaciones de los archivos que pertenecen a esa aplicación.

Los archivos binarios ejecutables del programa pueden ir en el directorio /usr/bin si están incluidos en el sistema operativo, o bien pueden ir en los directorios /usr/local/bin o /opt/application/bin si provienen de un tercero.

Los datos de la aplicación pueden almacenarse en uno de los siguientes subdirectorios:

- /usr/compartir
- /usr/lib
- /opt/aplicación
- /var/lib

El fichero relacionado con la documentación podrá almacenarse en uno de los siguientes subdirectorios:

- /usr/compartir/doc
- /USR/Acción/Valor
- /USR/Compartir/Información

Lo más probable es que los archivos de configuración global de una aplicación se almacenen en un subdirectorio bajo el directorio /etc, mientras que los archivos de configuración personalizada (específicos para un usuario) de la aplicación probablemente se encuentren en un subdirectorio oculto del directorio principal del usuario.

Directorios de bibliotecas

Las bibliotecas son archivos que contienen código que se comparte entre varios programas. La mayoría de los nombres de archivo de biblioteca terminan en una extensión de archivo de .so, que significa _objeto compartido_.

Es posible que haya varias versiones de una biblioteca porque el código puede ser diferente dentro de cada archivo, aunque pueda realizar funciones similares a las de otras versiones de la biblioteca. Una de las razones por las que el código puede ser diferente, aunque pueda hacer lo mismo que otro archivo de biblioteca, es que está compilado para ejecutarse en un tipo diferente de procesador. Por ejemplo, es habitual que los sistemas que utilizan código diseñado para procesadores de tipo Intel/AMD de 64 bits tengan bibliotecas de 32 bits y bibliotecas de 64 bits.

Las bibliotecas que soportan los programas binarios esenciales que se encuentran en los directorios /bin y /sbin se encuentran normalmente en /lib o /lib64.

Para soportar los ejecutables /usr/bin y /usr/sbin, normalmente se utilizan los directorios de biblioteca /usr/lib y /usr/lib64.

Para dar soporte a aplicaciones que no se distribuyen con el sistema operativo, se utilizan con frecuencia los directorios de biblioteca /usr/local/lib y /opt/application/lib.

Directorios de datos variables

El directorio /var y muchos de sus subdirectorios pueden contener datos que cambian con frecuencia. Si su sistema se utiliza para correo electrónico, normalmente se utilizan /var/mail o /var/spool/mail para almacenar los datos de correo electrónico de los usuarios. Si está imprimiendo desde su sistema, el directorio /var/spool/cups se utiliza para almacenar los trabajos de impresión temporalmente.

En función de los eventos que se registren y de la cantidad de actividad que se produzca, el sistema determina el tamaño del archivo de registro. En un sistema ocupado, podría haber una cantidad considerable de datos en los archivos de registro. Estos archivos se almacenan en el directorio /var/log.

Si bien los archivos de registro pueden ser útiles para solucionar problemas, pueden causar problemas. Una de las principales preocupaciones de todos estos directorios es que pueden llenar el espacio en disco rápidamente en un sistema activo. Si el directorio /var _no es_  una partición separada, entonces el sistema de archivos raíz podría llenarse y hacer que el sistema se bloquee.