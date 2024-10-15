# 14. Configuración de Red
## 14.1 Introducción
Tener acceso a la red es una característica clave de la mayoría de los sistemas Linux. Los usuarios desean navegar por la red, enviar y recibir correo electrónico y transferir archivos con otros usuarios.

Por lo general, los programas que realizan estas funciones, como los navegadores web y los clientes de correo electrónico, son razonablemente fáciles de usar. Sin embargo, todos se basan en una característica importante: la capacidad de su computadora para comunicarse con otra computadora. Para tener esta comunicación, necesita saber cómo configurar la red de su sistema.

Linux le proporciona varias herramientas tanto para configurar su red como para monitorear su rendimiento.⁠​​⁠​
## 14.2 Terminología básica de la red
Antes de configurar una red o acceder a una red existente, es beneficioso conocer algunos términos clave relacionados con las redes. En esta sección se exploran los términos con los que debe estar familiarizado. Algunos de los términos son básicos y es posible que ya esté familiarizado con ellos. Sin embargo, otros son más avanzados.

| **Anfitrión** | Un _host_ es una computadora. Muchas personas piensan automáticamente en una computadora de escritorio o portátil cuando escuchan el término _computadora_. En realidad, muchos otros dispositivos, como teléfonos celulares, reproductores de música digital y muchos televisores modernos, también son computadoras. En términos de redes, un host es cualquier dispositivo que se comunica a través de una red con otro dispositivo.                                                                                                                                                               |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Red**       | Una _red_ es una colección de dos o más hosts (computadoras) que pueden comunicarse entre sí. Esta comunicación puede ser a través de una conexión por cable o inalámbrica.                                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Internet**  | Internet  es un ejemplo de red. Consiste en una red de acceso público que conecta a millones de hosts en todo el mundo. Muchas personas utilizan Internet para navegar por páginas web e intercambiar correos electrónicos, pero Internet tiene muchas capacidades adicionales además de estas actividades.                                                                                                                                                                                                                                                                                           |
| **Wi-Fi**     | El término _Wi-Fi_ se refiere a las redes inalámbricas.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| **Servidor**  | Un host que proporciona un servicio a otro host o cliente se denomina _servidor_. Por ejemplo, un servidor web almacena, procesa y entrega páginas web. Un servidor de correo electrónico recibe el correo entrante y entrega el correo saliente.                                                                                                                                                                                                                                                                                                                                                     |
| **Servicio**  | Una función proporcionada por un host es un _servicio_. Un ejemplo de un servicio sería cuando un host proporciona páginas web a otro host.                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Cliente**   | Un _cliente_ es un host que accede a un servidor. Cuando está trabajando en una computadora navegando por Internet, se considera que está en un host cliente.                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| **Enrutador** | También llamado _puerta de enlace_, un _enrutador_ es una máquina que conecta hosts de una red a otra red. Por ejemplo, si trabaja en un entorno de oficina, todos los ordenadores de la empresa pueden comunicarse a través de la _red local_ creada por los administradores. Para acceder a Internet, las computadoras tendrían que comunicarse con un enrutador que se utilizaría para reenviar las comunicaciones de red a Internet. Por lo general, cuando se comunica en una red grande (como Internet), se utilizan varios enrutadores antes de que la comunicación llegue a su destino final. |
## 14.3 Terminología de las características de red
Además de los términos de red discutidos en la última sección, hay algunos términos adicionales con los que debe estar familiarizado. Estos términos se centran más en los diferentes tipos de servicios de red que se utilizan habitualmente, así como en algunas de las técnicas que se utilizan para comunicarse entre máquinas.

| **Paquete**        | Un _paquete de red_ se utiliza para enviar comunicación de red entre hosts. Al dividir la comunicación en fragmentos más pequeños (paquetes), el método de entrega de datos es mucho más eficiente.                                                                                                                                                                                                                                                                                                                  |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Dirección IP**   | Una _dirección de Protocolo de Internet (IP)_ es un número único asignado a un host en una red. Los hosts utilizan estos números para _abordar_  la comunicación de red.                                                                                                                                                                                                                                                                                                                                             |
| **Máscara**        | También llamada _máscara de red_, _máscara de subred_ o _máscara_, una máscara de _red_ es un sistema numérico que se puede utilizar para definir qué direcciones IP se consideran dentro de una sola red. Debido a la forma en que los enrutadores realizan sus funciones, las redes deben estar claramente definidas.                                                                                                                                                                                              |
| **Nombre de host** | Cada host de una red podría tener su propio _nombre de host_ porque los nombres son más naturales de recordar para los humanos que los números, lo que nos facilita el direccionamiento de los paquetes de red a otro host. Los nombres de host se traducen a direcciones IP antes de que el paquete de red se envíe a la red.                                                                                                                                                                                       |
| **URL**            | Un _Localizador Uniforme de Recursos (URL),_ también llamado comúnmente dirección web, se utiliza para localizar un recurso, como una página web, en Internet. Es lo que escribes en tu navegador web para acceder a una página web. Por ejemplo, _http://www.netdevgroup.com_. Incluye el http:// de protocolo  y el nombre de host www.netdevgroup.com.                                                                                                                                                            |
| **DHCP**           | A los hosts se les pueden asignar nombres de host, direcciones IP y otra información relacionada con la red mediante un  _servidor DHCP (Dynamic Host Configuration Protocol)._ En el mundo de las computadoras, un protocolo es un conjunto bien definido de reglas. DHCP define cómo se asigna la información de red a los hosts cliente, y el servidor DHCP es la máquina que proporciona esta información.                                                                                                       |
| **DNS**            | Como se mencionó anteriormente, los nombres de host se traducen en direcciones IP, antes de que el paquete de red se envíe a la red. Por lo tanto, su host necesita conocer la dirección IP de todos los demás hosts con los que se está comunicando. Cuando se trabaja en una red grande (como Internet), esto puede suponer un reto, ya que hay muchos hosts. Un _Sistema de Nombres de Dominio (DNS)_ proporciona el servicio de traducir nombres de dominio en direcciones IP.                                   |
| **Ethernet**       | En un entorno de red cableada, _Ethernet_ es la forma más común de conectar físicamente los hosts a una red. Los cables Ethernet están conectados a tarjetas de red que admiten conexiones Ethernet. Los cables y dispositivos Ethernet (como los enrutadores) están diseñados específicamente para admitir diferentes velocidades de comunicación, siendo la más baja de 10 Mbps (10 Megabits por segundo) y la más alta de 100 Gbps (100 gigabits por segundo). Las velocidades más comunes son 100 Mbps y 1 Gbps. |
| **TCP/IP**         | El _Protocolo de Control de Transmisión/Protocolo de Internet (TCP/IP)_ es un nombre elegante para una colección de protocolos (recuerde, protocolo = conjunto de reglas) que se utilizan para definir cómo debe tener lugar la comunicación de red entre hosts. Aunque no es la única colección de protocolos utilizados para definir la comunicación de red, es la más utilizada. Por ejemplo, TCP/IP incluye la definición de cómo funcionan las direcciones IP y las máscaras de red.                            |
## 14.4 Direcciones IP
Como se mencionó anteriormente, los hosts _direccionan_ los paquetes de red mediante la dirección IP de la máquina de destino. El paquete de red también incluye una _dirección de retorno_, que es la dirección IP de la máquina de envío.

De hecho, hay dos tipos diferentes de direcciones IP: **IPv4** e **IPv6**. Para entender por qué hay dos tipos diferentes, es necesario entender un breve fragmento de la historia del direccionamiento IP.

Durante muchos años, la técnica de direccionamiento IP que utilizaban todos los ordenadores era IPv4. En una dirección IPv4, se utilizan un total de cuatro números de 8 bits para definir la dirección. Se considera una dirección de 32 bits (4 x 8 = 32). Por ejemplo:

```
192.168.10.120
```

8 bits se refiere a los números del 0 al 255.

Cada host en Internet debe tener una dirección IP única. En un entorno IPv4, hay un límite técnico de aproximadamente 4.3 mil millones de direcciones IP. Sin embargo, muchas de estas direcciones IP no se pueden utilizar por varias razones. Además, muchas organizaciones no han hecho uso de todas las direcciones IP que tienen disponibles.

Si bien parece que debería haber muchas direcciones IP para todos, varios factores han llevado a un problema: Internet comenzó a quedarse sin direcciones IP.

Este tema alentó el desarrollo de IPv6. IPv6 se creó oficialmente en 1998. En una red IPv6, las direcciones son mucho más grandes, direcciones de 128 bits que se ven así:

```
2001:0db8:85a3:0042:1000:8a2e:0370:7334
```

Esencialmente, esto proporciona un grupo de direcciones mucho más grande, tan grande que es muy poco probable que se quede sin direcciones en un futuro cercano.

Es importante tener en cuenta que la diferencia entre IPv4 e IPv6 no es solo un grupo de direcciones más grande. IPv6 tiene muchas otras características avanzadas que abordan algunas de las limitaciones de IPv4, incluida una mejor velocidad, una administración de paquetes más avanzada y un transporte de datos más eficiente.

Teniendo en cuenta todas las ventajas, se podría pensar que a estas alturas todos los hosts estarían usando IPv6. Sin embargo, la mayoría de los dispositivos conectados a la red en el mundo todavía usan IPv4 (algo así como el 98-99% de todos los dispositivos).

Entonces, ¿por qué el mundo no ha adoptado la tecnología superior de IPv6?

Hay principalmente dos razones:

- **NAT:** Inventada para superar la posibilidad de quedarse sin direcciones IP en un entorno IPv4, la traducción de direcciones de red (NAT) utilizó una técnica para proporcionar acceso a Internet a más hosts. En pocas palabras, un grupo de hosts se coloca en una red privada sin acceso directo a Internet; un enrutador especial proporciona acceso a Internet, y solo este enrutador necesita una dirección IP para comunicarse en Internet. En otras palabras, un grupo de hosts comparte una sola dirección IP, lo que significa que muchos más ordenadores pueden conectarse a Internet. Esta característica significa que la necesidad de pasar a IPv6 es menos crítica que antes de la invención de NAT.
- **Portabilidad:** La portabilidad es cambiar de una tecnología a otra. IPv6 tiene muchas características nuevas excelentes, pero todos los hosts deben poder utilizar estas características. Lograr que todos en Internet (o incluso solo algunos) hagan estos cambios plantea un desafío.

‌⁠​​No obstante, la mayoría de los expertos están de acuerdo en que IPv6 eventualmente reemplazará a IPv4, por lo que se recomienda comprender los conceptos básicos de ambos para aquellos que trabajan en la industria de TI.
## 14.5 Configuración de dispositivos de red
Al configurar dispositivos de red, hay dos preguntas iniciales que debe hacerse:

- _¿Con cable o inalámbrico?_ La configuración de un dispositivo inalámbrico es ligeramente diferente a la configuración de un dispositivo con cable debido a algunas de las características adicionales que normalmente se encuentran en los dispositivos inalámbricos (como la seguridad).
- _¿DHCP o dirección estática?_ Recuerde que un servidor DHCP proporciona información de red, como su dirección IP y máscara de subred. Si no utiliza un servidor DHCP, deberá proporcionar manualmente esta información a su host, que se llama mediante una dirección IP estática.

‌⁠​​⁠​En términos generales, las máquinas de escritorio usan redes cableadas, mientras que las computadoras portátiles usan redes inalámbricas. Normalmente, una máquina cableada utiliza una dirección IP estática, pero a menudo también se puede asignar a través de un servidor DHCP. En casi todos los casos, las máquinas inalámbricas utilizan DHCP, ya que casi siempre son móviles y están conectadas a diferentes redes.
### 14.5.1 Configuración de la red mediante archivos de configuración
Habrá ocasiones en las que no haya ninguna herramienta basada en GUI disponible. En esos casos, es útil conocer los archivos de configuración que se utilizan para almacenar y modificar los datos de red.

> Estos archivos pueden variar en función de la distribución de Linux en la que esté trabajando.
#### 14.5.1.1 Archivo de configuración IPv4 principal
En un sistema CentOS, el archivo de configuración principal para una interfaz de red IPv4 es el archivo /etc/sysconfig/network-scripts/ifcfg-eth0. La máquina virtual de este capítulo está basada en Debian y, por lo tanto, no tiene la carpeta sysconfig. Sin embargo, solo con fines de demostración, a continuación se muestra el aspecto de este archivo cuando se configura para una dirección IP estática

```
root@localhost:~# cat /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE="eth0"
BOOTPROTO=none
NM_CONTROLLED="yes"
ONBOOT=yes
TYPE="Ethernet"
UUID="98cf38bf-d91c-49b3-bb1b-f48ae7f2d3b5"
DEFROUTE=yes
IPV4 _FAILURE_FATAL=yes
IPV6INOT=no
NAME="System eth0"
IPADDR=192.168.1.1
PREFIX=24
GATEWAY=192.168.1.1
DNS1=192.168.1.2
HWADDR=00:50:56:90:18:18
LAST_CONNECT=1376319928
```

Si el dispositivo estuviera configurado para ser un cliente DHCP, el valor de BOOTPROTO se establecería en dhcp y los valores de IPADDR, GATEWAY y DNS1 no se establecerían.
#### 14.5.1.2 Archivo de configuración IPv6 primario
En un sistema CentOS, el archivo de configuración IPv6 principal es el mismo archivo donde se almacena la configuración IPv4; el archivo /etc/sysconfig/network-scripts/ifcfg-eth0. Si desea que su sistema tenga una dirección IPv6 estática, agregue lo siguiente al archivo de configuración:

```
IPV6INIT=yes
IPV6ADDR=<IPv6 IP Address>
IPV6_DEFAULTGW=<IPv6 IP Gateway Address>
```

Si desea que su sistema sea un cliente IPv6 DHCP, agregue la siguiente configuración:

```
DHCPV6C=yes
```

También debe agregar la siguiente configuración al archivo /etc/sysconfig/network:

```
NETWORKING_IPV6=yes
```

> El método ampliamente aceptado para realizar cambios en una interfaz de red es desconectar la interfaz mediante un comando como ifdown eth0, realizar los cambios deseados en el archivo de configuración y, a continuación, volver a poner en servicio la interfaz con un comando como ifup eth0.

> Otro método menos específico es reiniciar la red del sistema por completo, con un comando como service network restart, que elimina TODAS las interfaces, vuelve a leer todos los archivos de configuración relacionados y, a continuación, reinicia la red del sistema.

> Reiniciar el servicio de red puede interrumpir mucho más que la interfaz única que un usuario quería cambiar, así que use los comandos más limitados y específicos para reiniciar la interfaz si es posible.

> En el ejemplo siguiente se muestra cómo se debería ejecutar el comando de servicio en un sistema CentOS:

```
[root@localhost ~]# service network restart
Shutting down interface eth0:  Device state: 3 (disconnected)
                                                           [  OK  ]
Shutting down loopback interface:                          [  OK  ]
Bringing up loopback interface:                            [  OK  ]
Bringing up interface eth0:  Active connection state: activated
Active connection path: /org/freedesktop/NetworkManager/ActiveConnection/1
                                                           [  OK  ]
```
#### 14.5.1.3 Sistema de nombres de dominio (DNS)
Cuando se le pide a una computadora que acceda a un sitio web, como _www.example.com_, no necesariamente sabe qué dirección IP usar. Para que el equipo asocie una dirección IP con la URL o la solicitud de nombre de host, el equipo se basa en el servicio DNS de otro equipo. A menudo, la dirección IP del servidor DNS se descubre durante la solicitud DHCP, mientras una computadora recibe información de direccionamiento importante para comunicarse en la red.

La dirección del servidor DNS se almacena en el archivo /etc/resolv.conf. Un archivo /etc/resolv.conf típico se genera automáticamente y tiene el siguiente aspecto:

```
sysadmin@localhost:~$ cat /etc/resolv.conf                     
nameserver 127.0.0.1
```

La configuración del servidor de nombres a menudo se establece en la dirección IP del servidor DNS. En el ejemplo siguiente se utiliza el comando host, que funciona con DNS para asociar un nombre de host a una dirección IP. Tenga en cuenta que el servidor DNS asocia el servidor DNS con la dirección IP 192.168.1.2:

```
sysadmin@localhost:~$ host example.com                         
example.com has address 192.168.1.2  
```

También es común tener varias configuraciones de servidor de nombres, en el caso de que un servidor DNS no responda.
#### 14.5.1.4 Archivos de configuración de red
La resolución de nombres en un host Linux se logra mediante 3 archivos críticos: los archivos /etc/hosts, /etc/resolv.conf y /etc/nsswitch.conf. Juntos, describen la ubicación de la información del servicio de nombres, el orden en el que se deben consultar los recursos y dónde ir para obtener esa información.

- `/etc/hosts`
Este archivo contiene una tabla de nombres de host para direcciones IP. Se puede utilizar para complementar un servidor DNS.

```
sysadmin@localhost:~$ cat /etc/hosts  
127.0.0.1       localhost
```

- `/etc/resuelve.conf`
Este archivo contiene las direcciones IP de los servidores de nombres que el sistema debe consultar en cualquier intento de resolver nombres en direcciones IP. Estos servidores suelen ser servidores DNS. También puede contener palabras clave y valores adicionales que pueden afectar al proceso de resolución.

```
sysadmin@localhost:~$ cat /etc/resolv.conf  
nameserver 127.0.0.11
```

- `/etc/nsswitch.conf`
Este archivo se puede utilizar para modificar dónde se producen las búsquedas de nombres de host. Contiene una entrada particular que describe en qué orden se consultan las fuentes de resolución de nombres.

```
sysadmin@localhost:~$ cat /etc/nsswitch.conf 
# /etc/nsswitch.conf
# 

Output Omitted...

hosts:          files dns

Output Omitted...
```

Primero se busca en el archivo /etc/hosts, luego en el servidor DNS:

```
hosts: files dns 
```

Primero se buscaría en el servidor DNS, luego en los archivos locales:

```
hosts: dns files
```

Los comandos o programas del sistema, como el navegador, solicitan una conexión con un equipo remoto por nombre DNS. A continuación, el sistema consulta varios archivos en un orden determinado para intentar resolver ese nombre en una dirección IP utilizable.

1. En primer lugar, se consulta el archivo /etc/nsswitch.conf:

```
hosts:		files dns
```

Esta línea indica que el sistema debe consultar primero los archivos locales en un intento de resolver los nombres de host, lo que significa que el archivo /etc/hosts se analizará para ver si coincide con el nombre solicitado.

2. En segundo lugar, el sistema consultará el archivo /etc/hosts para intentar resolver el nombre. Si el nombre coincide con una entrada en /etc/hosts, se resuelve.

No conmutará por error (ni continuará) a la opción DNS, incluso si la resolución es inexacta. Esto puede ocurrir si la entrada en /etc/hosts apunta a una dirección IP no asignada.

3. En tercer lugar, si el archivo local /etc/hosts no da como resultado una coincidencia, el sistema utilizará las entradas del servidor DNS configuradas contenidas en el archivo /etc/resolv.conf para intentar resolver el nombre.

El archivo /etc/resolv.conf debe contener al menos dos entradas para servidores de nombres, como el archivo de ejemplo a continuación:

```
nameserver 10.0.2.3
nameserver 10.0.2.4
```

El sistema de resolución de DNS utilizará el servidor de nombres para un intento de búsqueda del nombre. Si no está disponible o se alcanza un período de tiempo de espera, se consultará al segundo servidor para obtener la resolución de nombres. Si se encuentra una coincidencia, se devuelve al sistema y se utiliza para iniciar una conexión y también se coloca en la caché DNS durante un período de tiempo configurable.

> Pueden aparecer otras dos palabras clave en el archivo /etc/resolv.conf del sistema. Aunque estos están más allá del alcance de este curso, se incluyen rutinariamente en los archivos /etc/resolv.conf predeterminados, por lo que incluimos explicaciones de estos términos a continuación:

| dominio | Seguido de un dominio calificado, como snowblower.example.com, permite que la consulta del host polaris se intente tanto como el host polaris, o en su defecto, anexándole el resto del nombre de dominio y, con suerte, que el servidor lo resuelva como ese nombre (por ejemplo, polaris.snowblower.example.com.). |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| buscar  | Seguido de un conjunto de dominios separados que se pueden consultar uno tras otro, con la esperanza de resolver el nombre.                                                                                                                                                                                          |
## 14.6 Herramientas de red
Hay varios comandos que puede utilizar para ver la información de la red. Estas herramientas también pueden ser útiles cuando se están solucionando problemas de red.
### 14.6.1 El comando ifconfig
El comando ifconfig representa la _configuración de la interfaz_ y se utiliza para mostrar información de configuración de red. No todas las configuraciones de red se cubren en este curso, pero es importante tener en cuenta en el resultado a continuación que la dirección IP del dispositivo de red principal eth0 es 192.168.1.2 y que el dispositivo está actualmente activo UP:

```
root@localhost:~# ifconfig                                     
eth0      Link encap:Ethernet  HWaddr b6:84:ab:e9:8f:0a    
          inet addr:192.168.1.2  Bcast:0.0.0.0  Mask:255.255.255.0  
          inet6 addr: fe80::b484:abff:fee9:8f0a/64 Scope:Link       
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1        
          RX packets:95 errors:0 dropped:4 overruns:0 frame:0       
          TX packets:9 errors:0 dropped:0 overruns:0 carrier:0      
          collisions:0 txqueuelen:1000                              
          RX bytes:25306 (25.3 KB)  TX bytes:690 (690.0 B)          
lo        Link encap:Local Loopback                               
          inet addr:127.0.0.1  Mask:255.0.0.0                       
          inet6 addr: ::1/128 Scope:Host                           
          UP LOOPBACK RUNNING  MTU:65536  Metric:1                  
          RX packets:6 errors:0 dropped:0 overruns:0 frame:0        
          TX packets:6 errors:0 dropped:0 overruns:0 carrier:0      
          collisions:0 txqueuelen:0                                 
          RX bytes:460 (460.0 B)  TX bytes:460 (460.0 B)
```

> El dispositivo lo se conoce como  dispositivo _de bucle invertido_. Es un dispositivo de red especial utilizado por el sistema cuando se envía datos basados en la red a sí mismo.

El comando ifconfig también se puede utilizar para modificar temporalmente la configuración de la red. Por lo general, estos cambios deben ser permanentes, por lo que el uso del comando ifconfig para realizar dichos cambios es relativamente raro.
### 14.6.2 El comando ip
El comando ifconfig se está volviendo obsoleto en algunas distribuciones de Linux (obsoleto) y está siendo reemplazado por una forma del comando ip, específicamente ip addr show.

El comando ip difiere de ifconfig en varios aspectos importantes, principalmente en que a través de su mayor funcionalidad y conjunto de opciones, puede ser casi una ventanilla única para la configuración y el control de la red de un sistema. El formato del comando ip es el siguiente:

```
ip [OPTIONS] OBJECT COMMAND
```

Mientras que ifconfig se limita principalmente a la modificación de los parámetros de red y a la visualización de los detalles de configuración de los componentes de red, el comando ip se ramifica para hacer parte del trabajo de varios otros comandos heredados, como route y arp.

> Los comandos de Linux y Unix no suelen desaparecer cuando se vuelven obsoletos; Se mantienen como un comando heredado, a veces durante muchos años, ya que la cantidad de scripts que dependen de esos comandos y la cantidad de memoria muscular entre los administradores del sistema, hacen que sea una buena idea mantenerlos por compatibilidad.

El comando ip puede parecer inicialmente un poco más detallado que el comando ifconfig, pero es una cuestión de redacción y un resultado de la filosofía detrás del funcionamiento del comando ip.

En el siguiente ejemplo, tanto el comando ifconfig como el comando ip se utilizan para mostrar todas las interfaces del sistema.

```
root@localhost:~# ifconfig
eth0      Link encap:Ethernet  HWaddr 00:0c:29:71:f0:bb  
          inet addr:172.16.241.140  Bcast:172.16.241.255  Mask:255.255.255.0
          inet6 addr: fe80::20c:29ff:fe71:f0bb/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:8506 errors:0 dropped:0 overruns:0 frame:0
          TX packets:1201 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:8933700 (8.9 MB)  TX bytes:117237 (117.2 KB)
 
lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:285 errors:0 dropped:0 overruns:0 frame:0
          TX packets:285 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1 
          RX bytes:21413 (21.4 KB)  TX bytes:21413 (21.4 KB)
root@localhost:~# ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:71:f0:bb brd ff:ff:ff:ff:ff:ff
    inet 172.16.241.140/24 brd 172.16.241.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fe71:f0bb/64 scope link 
       valid_lft forever preferred_lft forever
```

Ambos muestran el tipo de interfaz, protocolos, hardware y direcciones IP, máscaras de red y otra información sobre cada una de las interfaces activas en el sistema.
### 14.6.3 El comando de ruta
Recuerde que un enrutador (o puerta de enlace) es una máquina que permite que los hosts de una red se comuniquen con otra red. Para ver una tabla que describa dónde se envían los paquetes de red, utilice el comando route:

```
root@localhost:~# route                                             
Kernel IP routing table                                             
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface   
192.168.1.0     *               255.255.255.0   U     0      0        0 eth0  
default         192.168.1.1     0.0.0.0        UG     0      0        0 eth0 
```

La primera línea resaltada en el ejemplo anterior indica que cualquier paquete de red enviado a un equipo en la red 192.168.0 no se envía a un equipo de puerta de enlace (el * indica _que no hay puerta de enlace_). La segunda línea resaltada indica que todos los demás paquetes de red se envían al host con la dirección IP de 192.168.1.1 (el router).

Algunos usuarios prefieren mostrar esta información solo con datos numéricos, utilizando la opción -n del comando route. Por ejemplo, compare lo siguiente y concéntrese en dónde se mostraba la palabra predeterminado en la salida anterior:

```
root@localhost:~# route -n                                                      
Kernel IP routing table                                                         
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface   
192.168.1.0     0.0.0.0         255.255.255.0   U     0      0        0 eth0  
0.0.0.0        192.168.1.1     0.0.0.0         UG    0      0        0 eth0  
```

0.0.0.0 se refiere a _todas las demás máquinas_ y es el mismo que el _valor predeterminado_.

El comando route se está volviendo obsoleto en algunas distribuciones de Linux (obsoleto) y está siendo reemplazado por una forma del comando ip, específicamente ip route o ip route show. Tenga en cuenta que la misma información resaltada anteriormente también se puede encontrar usando este comando:

```
root@localhost:~# ip route show
default via 192.168.1.254 dev eth0 proto static                                
192.168.1.0/24 dev eth0  proto kernel  scope link  src 192.168.1.2   root@localhost:~# ip route show
default via 192.168.1.254 dev eth0 proto static                                
192.168.1.0/24 dev eth0  proto kernel  scope link  src 192.168.1.2   
```
### 14.6.4 El comando ping
El comando ping se puede utilizar para determinar si se _puede acceder_ a otro equipo. Si el comando ping puede enviar un paquete de red a otra máquina y recibir una respuesta, entonces debería poder conectarse a esa máquina.

De forma predeterminada, el comando ping continúa enviando paquetes sin fin. Para limitar el número de pings que se van a enviar, utilice la opción -c seguida de un número que indique el número de iteraciones que desea. En los ejemplos siguientes se muestra que ping se limita a 4 iteraciones.

Si el comando ping se ejecuta correctamente, se parece al siguiente ejemplo:

```
root@localhost:~# ping -c 4 192.168.1.2                                       
PING 192.168.1.2 (192.168.1.2) 56(84) bytes of data.                          
64 bytes from 192.168.1.2: icmp_req=1 ttl=64 time=0.051 ms                    
64 bytes from 192.168.1.2: icmp_req=2 ttl=64 time=0.064 ms                    
64 bytes from 192.168.1.2: icmp_req=3 ttl=64 time=0.050 ms                    
64 bytes from 192.168.1.2: icmp_req=4 ttl=64 time=0.043 ms                    
                                                                              
--- 192.168.1.2 ping statistics ---                                           
4 packets transmitted, 4 received, 0% packet loss, time 2999ms                
rtt min/avg/max/mdev = 0.043/0.052/0.064/0.007 ms
```

Si se produce un error en el comando ping, se muestra un mensaje que indica que el host de destino es inalcanzable:

```
root@localhost:~# ping -c 4 192.168.1.1                                       
PING 192.168.1.1 (192.168.1.1) 56(84) bytes of data.                          
From 192.168.1.2 icmp_seq=1 Destination Host Unreachable                      
From 192.168.1.2 icmp_seq=2 Destination Host Unreachable                      
From 192.168.1.2 icmp_seq=3 Destination Host Unreachable                      
From 192.168.1.2 icmp_seq=4 Destination Host Unreachable                      
                                                                                
--- 192.168.1.1 ping statistics ---                                           
4 packets transmitted, 0 received, +4 errors, 100% packet loss, time 2999ms   
pipe 4   
```

Es importante tener en cuenta que el hecho de que el comando ping falle no significa que no se pueda acceder al sistema remoto. Algunos administradores configuran sus máquinas (¡e incluso redes enteras!) para que no respondan a las solicitudes de ping porque un servidor puede ser atacado por algo llamado _ataque de denegación de servicio_. En este tipo de ataque, un servidor se ve abrumado por una gran cantidad de paquetes de red. Al ignorar las solicitudes de ping, el servidor es menos vulnerable.

Como resultado, el comando ping puede ser útil para comprobar la disponibilidad de los equipos locales, pero no siempre para los equipos fuera de su propia red.

> Muchos administradores usan el comando ping con un nombre de host y, si eso falla, use la dirección IP para ver si la falla está en resolver el nombre de host del dispositivo. Usar el nombre de host primero ahorra tiempo; si ese comando ping tiene éxito, hay una resolución de nombres adecuada y la dirección IP también funciona correctamente.
### 14.6.5 El comando netstat
El comando netstat es una herramienta poderosa que proporciona una gran cantidad de información de red. Se puede utilizar para mostrar información sobre las conexiones de red, así como para mostrar la tabla de ruteo de forma similar al comando route.

Por ejemplo, para mostrar estadísticas sobre el tráfico de red, utilice la opción -i para el comando netstat:

```
root@localhost:~# netstat -i                                                  
Kernel Interface table                                                        
Iface   MTU Met   RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
eth0       1500 0       137      0      4 0        12      0      0      0 BMRU
lo        65536 0        18      0      0 0        18      0      0      0 LRU
```

Las estadísticas más importantes de la salida anterior son TX-OK y TX-ERR. Un alto porcentaje de TX-ERR puede indicar un problema en la red, como demasiado tráfico de red.

Para utilizar el comando netstat para mostrar información de enrutamiento, utilice la opción -r:

```
root@localhost:~# netstat -r                                                  
Kernel IP routing table                                                       
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
192.168.1.0     *               255.255.255.0   U         0 0          0 eth0 
default         192.168.1.1     0.0.0.0        UG         0 0          0 eth0 
```

El comando netstat también se usa comúnmente para mostrar _puertos abiertos_. Un puerto es un número único que está asociado con un servicio proporcionado por un host. Si el puerto está abierto, el servicio está disponible para otros hosts.

Por ejemplo, puede iniciar sesión en un host desde otro host mediante un servicio llamado _SSH_. Al servicio SSH se le asigna el puerto #22. Por lo tanto, si el puerto #22 está abierto, entonces el servicio está disponible para otros hosts.

Es importante tener en cuenta que el host también debe tener los servicios ejecutándose por sí mismo; esto significa que el servicio (en este caso, el demonio ssh) que permite a los usuarios remotos iniciar sesión debe iniciarse (lo que suele suceder en la mayoría de las distribuciones de Linux).

Para ver una lista de todos los puertos abiertos actualmente, utilice el siguiente comando:

```
root@localhost:~# netstat -tln                                                
Active Internet connections (only servers)                                    
Proto Recv-Q Send-Q Local Address           Foreign Address         State     
tcp        0      0 192.168.1.2:53          0.0.0.0:*               LISTEN    
tcp        0      0 127.0.0.1:53            0.0.0.0:*               LISTEN    
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN    
tcp        0      0 127.0.0.1:953           0.0.0.0:*               LISTEN    
tcp6       0      0 :::53                   :::*                    LISTEN    
tcp6       0      0 :::22                  :::*                    LISTEN   
tcp6       0      0 ::1:953                 :::*                    LISTEN  
```

Como puede ver en el resultado anterior, el puerto # 22 está _escuchando_, lo que significa que está abierto.

En el ejemplo anterior, -t representa _TCP_ (recuerde este protocolo de anteriormente en este capítulo), -l representa _escuchar_ (qué puertos están escuchando) y -n representa _mostrar números, no nombres_.

A veces, mostrar los nombres puede ser más útil. Esto se puede lograr eliminando la opción -n:

```
root@localhost:~# netstat -tl                                                   
Active Internet connections (only servers)                                      
Proto Recv-Q Send-Q Local Address           Foreign Address          State     
tcp        0      0 cserver.example.:domain *:*                     LISTEN    
tcp        0      0 localhost:domain        *:*                     LISTEN    
tcp        0      0 *:ssh                   *:*                     LISTEN    
tcp        0      0 localhost:953           *:*                     LISTEN    
tcp6       0      0 [::]:domain             [::]:*                  LISTEN    
tcp6       0      0 [::]:ssh                [::]:*                  LISTEN    
tcp6       0      0 localhost:953           [::]:*                  LISTEN    
```

En algunas distribuciones puede ver el siguiente mensaje en la página de manual del comando netstat:

```
NOTE
     This program is obsolete. Replacement for netstat is ss. Replacement for 
     netstat -r is ip route. Replacement for netstat -i is ip -s link. 
     Replacement for netstat -g is ip maddr.
```

Si bien no se está realizando ningún desarrollo adicional en el comando netstat, sigue siendo una excelente herramienta para mostrar información de red. El objetivo es eventualmente reemplazar el comando netstat con comandos como los comandos ss e ip. Sin embargo, es importante tener en cuenta que esto puede llevar algún tiempo.
### 14.6.6 El comando ss
El comando ss está diseñado para mostrar estadísticas de socket y admite todos los principales tipos de paquetes y sockets. Destinado a ser un reemplazo y ser similar en función al comando netstat, también muestra mucha más información y tiene más características.

La razón principal por la que un usuario usaría el comando ss es para ver qué conexiones están establecidas actualmente entre su máquina local y las máquinas remotas, estadísticas sobre esas conexiones, etc.

De manera similar al comando netstat, puede obtener una gran cantidad de información útil del comando ss por sí mismo, como se muestra en el ejemplo a continuación.

```
root@localhost:~# ss
Netid  State      Recv-Q Send-Q   	Local Address:Port       	   Peer Address:Port
u_str  ESTAB      0      0                    * 104741                     * 104740 
u_str  ESTAB      0      0      /var/run/dbus/system_bus_socket 14623      * 14606  
u_str  ESTAB      0      0      /var/run/dbus/system_bus_socket 13582      * 13581  
u_str  ESTAB      0      0      /var/run/dbus/system_bus_socket 16243      * 16242  
u_str  ESTAB      0      0                    * 16009                      * 16010  
u_str  ESTAB      0      0      /var/run/dbus/system_bus_socket 10910      * 10909  
u_str  ESTAB      0      0      @/tmp/dbus-LoJW0hGFkV 15706                * 15705  
u_str  ESTAB      0      0                    * 24997                 	   * 24998  
u_str  ESTAB      0      0                    * 16242                      * 16243  
u_str  ESTAB      0      0      	@/tmp/dbus-opsTQoGE 15471          * 15470 
```

La salida es muy similar a la salida del comando netstat sin opciones. Las columnas anteriores son:

| Netid                     | El tipo de socket y el protocolo de transporte                      |
| ------------------------- | ------------------------------------------------------------------- |
| Estado                    | Conectado o desconectado, según el protocolo                        |
| Recv-Q                    | Cantidad de datos en cola para ser procesados que se han recibido   |
| Enviar-Q                  | Cantidad de datos en cola para ser enviados a otro host             |
| Dirección local           | La dirección y el puerto de la parte de la conexión del host local  |
| Dirección del mismo nivel | La dirección y el puerto de la parte de la conexión del host remoto |

El formato de la salida del comando ss puede cambiar drásticamente, dadas las opciones especificadas, como el uso de la opción -s, que muestra principalmente los tipos de sockets, estadísticas sobre su existencia y el número de paquetes reales enviados y recibidos a través de cada tipo de socket, como se muestra a continuación:

> El comando ss suele mostrar muchas filas de datos, y puede ser algo desalentador tratar de encontrar lo que desea en toda esa salida. Considere la posibilidad de enviar la salida al comando less para que la salida sea más manejable. Los paginadores permiten al usuario desplazarse hacia arriba y hacia abajo, realizar búsquedas y muchas otras funciones útiles dentro de los parámetros del comando less.

Si bien el comando ss ofrece muchas opciones diferentes para recopilar y mostrar información, los ejemplos anteriores son los más comunes, y cualquier otra cosa estaría fuera del alcance de los objetivos del examen en este nivel.
### 14.6.7 El comando dig
Puede haber ocasiones en las que necesite probar la funcionalidad del servidor DNS que está utilizando su host. Una forma de hacerlo es utilizar el comando dig, que realiza consultas en el servidor DNS para determinar si la información necesaria está disponible en el servidor.

En el siguiente ejemplo, el comando dig se utiliza para determinar la dirección IP del host example.com:

```
root@localhost:~# dig example.com                                              
; <<>> DiG 9.8.1-P1 <<>> example.com                                          
;; global options: +cmd                                                       
;; Got answer:                                                                
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 45155                     
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 0       
                                                                              
;; QUESTION SECTION:                                                          
;example.com.                   IN      A                                     
                                                                              
;; ANSWER SECTION:                                                            
example.com.            86400   IN      A       192.168.1.2                  
;; AUTHORITY SECTION:                                                         
example.com.            86400   IN      NS      example.com.                  
                                                                              
;; Query time: 0 msec                                                         
;; SERVER: 127.0.0.1#53(127.0.0.1)                                            
;; WHEN: Tue Dec  8 17:54:41 2015                                             
;; MSG SIZE  rcvd: 59    
```

Tenga en cuenta que la respuesta incluía la dirección IP de 192.168.1.2, lo que significa que el servidor DNS tiene la información de traducción de la dirección IP al nombre de host en su base de datos.

Si el servidor DNS no tiene la información solicitada, está configurado para preguntar a otros servidores DNS. Si ninguno de ellos tiene la información solicitada, aparece un mensaje de error:

```
root@localhost:~# dig sample.com                                            
; <<>> DiG 9.8.1-P1 <<>> sample.com                                           
;; global options: +cmd 
;; connection timed out; no servers could be reached
```
### 14.6.8 El comando host
En su forma más simple, el comando host funciona con DNS para asociar un nombre de host con una dirección IP. Como se usó en un ejemplo anterior, example.com está asociado con la dirección IP de 192.168.1.2:

```
root@localhost:~# host example.com                                            
example.com has address 192.168.1.2  
```

El comando host también se puede utilizar a la inversa si se conoce una dirección IP, pero no el nombre de dominio.

```
root@localhost:~# host 192.168.1.2                                            
2.1.168.192.in-addr.arpa domain name pointer example.com.                     
2.1.168.192.in-addr.arpa domain name pointer cserver.example.com.  
```

Existen otras opciones para consultar los diversos aspectos de un DNS, como un _nombre canónico CNAME -alias_:

```
root@localhost:~# host -t CNAME example.com                                   
example.com has no CNAME record  
```

Dado que muchos servidores DNS almacenan una copia de example.com, _los registros de inicio de autoridad de_ SOA  indican el servidor principal del dominio:

```
root@localhost:~# host -t SOA example.com                                     
example.com has SOA record example.com. cserver.example.com. 2 604800 86400 2419200 604800
```

Se puede encontrar una lista completa de información de DNS con respecto a example.com utilizando la opción -a _all_:

```
root@localhost:~# host -a example.com                                         
Trying "example.com"                                                          
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 3549                      
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1       
                                                                              
;; QUESTION SECTION:                                                          
;example.com.                   IN      ANY                                   
                                                                              
;; ANSWER SECTION:                                                            
example.com.            86400   IN      SOA     example.com. cserver.example.com. 2 604800 86400 2419200 604800                                              
example.com.            86400   IN      NS      example.com.                  
example.com.            86400   IN      A       192.168.1.2                   
                                                                              
;; ADDITIONAL SECTION:                                                        
example.com.            86400   IN      A       192.168.1.2                   
                                                                              
Received 119 bytes from 127.0.0.1#53 in 0 ms
```
### 14.6.9 El comando ssh
El comando ssh le permite conectarse a otra máquina a través de la red, iniciar sesión y luego realizar tareas en la máquina remota.

Si solo proporciona un nombre de máquina o una dirección IP para iniciar sesión, el comando ssh asume que desea iniciar sesión con el mismo nombre de usuario con el que ha iniciado sesión actualmente. Para usar un nombre de usuario diferente, use la sintaxis:

```
username@hostname
```

```
root@localhost:~# ssh bob@test
The authenticity of host ‘test (127.0.0.1)’ can’t be established.
RSA key fingerprint is c2:0d:ff:27:4c:f8:69:a9:c6:3e:13:da:2f:47:e4:c9.
Are you sure you want to continue connection (yes/no)? yes
Warning: Permanently added ‘test’ (RSA) to the list of known hosts.
bob@test’s password:
bob@test:~$ date
Fri Oct   4 16:14:43 CDT 2013  
```

Para volver al equipo local, utilice el comando exit:

```
bob@test:~$ exit
logout
Connection to test closed.
root@localhost:~#
```

> Tenga cuidado, si usa el comando exit demasiadas veces, cerrará la ventana del terminal en la que está trabajando.
#### 14.6.9.1 Huella digital de la clave RSA
Al usar el comando ssh, el primer mensaje le pide que verifique la identidad de la máquina en la que está iniciando sesión. En la mayoría de los casos, vas a querer responder que sí. Aunque puede consultar con el administrador del equipo remoto para asegurarse de que la huella digital de la clave RSA es correcta, este no es el propósito de esta consulta. Está diseñado para futuros intentos de inicio de sesión.

Después de responder afirmativamente, la huella digital de la clave RSA del equipo remoto se almacena en su sistema local. Cuando intente ssh a esta misma máquina en el futuro, la huella digital de la clave RSA proporcionada por la máquina remota se compara con la copia almacenada en la máquina local. Si coinciden, aparecerá el mensaje de nombre de usuario. Si no coinciden, se muestra un error como el siguiente:

```
sysadmin@localhost:~$ ssh bob@test
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@   WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!   @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that the RSA host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
c2:0d:ff:27:4c:f8:69:a9:c6:3e:13:da:2f:47:e4:c9.
Please contact your system administrator.
Add correct host key in /home/sysadmin/.ssh/known_hosts to get rid of this message.
Offending key in /home/sysadmin/.ssh/known_hosts:1
RSA host key for test has changed and you have requested strict checking.
Host key verification failed.
```

Este error podría indicar que un host no autorizado ha reemplazado al host correcto. Consulte con el administrador del sistema remoto. Si el sistema se reinstalara recientemente, tendría una nueva clave RSA y eso estaría causando este error.

En el caso de que este mensaje de error se deba a una reinstalación de una máquina remota, puede eliminar el archivo ~/.ssh/known_hosts de su sistema local (o simplemente eliminar la entrada de esa máquina) e intentar conectarse de nuevo:

```
sysadmin@localhost:~$ cat ~/.ssh/known_hosts
test ssh-rsa AAAAB3NzaC1yc2EAAAAmIwAAAQEAklOUpkDHrfHY17SbrmTIp/RZ0V4DTxgq9wzd+ohy006SWDSGPA+nafzlHDPOW7vdI4mZ5ew18KL4JW9jbhUFrviQzM7xlELEVf4h9lFX5QVkbPppSrg0cda3Pbv7kOdJ/MTyBlWXFCRH+Cv3FXRitBqxiX1nKhXpHAZsMciLq8V6RjsNAQwdsdMFvSlVK/7BA
t5FaiKoAfncM1Q8x3+2V0Ww71/eIFmb1zuUFljHYTprrX88XypNDvjYNby6vw/Pb0rwprz/Tn
mZAW3UX+PnTPI89ZPmNBLuxyrD2cE86Z/il8b+gw3r3+1nJotmIkjn2so1d01QraTlMqVSsbx
NrRFi9wrf+ghw==
sysadmin@localhost:~$ rm ~/.ssh/known_hosts
sysadmin@localhost:~$ ssh bob@test
The authenticity of host ‘test (127.0.0.1)’ can’t be established.
RSA key fingerprint is c2:0d:ff:27:4c:f8:69:a9:c6:3e:13:da:2f:47:e4:c9.
Are you sure you want to continue connection (yes/no)? yes
Warning: Permanently added ‘test’ (RSA) to the list of known hosts.
bob@test’s password:
Last login: Fri Oct   4 16:14:39 CDT 2013  from localhost
```