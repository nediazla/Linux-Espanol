# 12. Comprender el Hardware de la Computadora

## 12.1 Introducción
Las computadoras comienzan como un dispositivo de hardware, pero pueden crecer hasta abarcar máquinas virtuales que están completamente construidas a partir de software y parecen ser a todos los efectos una máquina remota a la que los usuarios pueden acceder a través de protocolos y métodos remotos.

Teniendo en cuenta las máquinas virtuales, es imperativo comprender de qué se compone el hardware físico que compone una computadora. Sin una comprensión de los componentes que componen una computadora, cómo se integran y se comunican entre sí, y cómo _debe funcionar una computadora_  , no puede instalar, configurar, administrar, proteger o solucionar problemas correctamente de un sistema.

Hay cientos de tipos específicos de computadoras en servicio, desde decodificadores hasta firewalls dedicados, computadoras portátiles, servidores y muchos dispositivos de propósito especial, pero todos cuentan con al menos algunos de los mismos componentes básicos que realmente los convierten en una computadora. Este capítulo presenta y explica los conceptos básicos de lo que compone una computadora y se expande a algunos elementos opcionales que son bastante comunes.
## 12.2 Placas base
La placa base, o placa del sistema, es la placa de hardware principal de la computadora a través de la cual  _se conectan la unidad central de procesamiento (CPU),_  la _memoria de acceso aleatorio (RAM)_ y otros componentes. Dependiendo del tipo de computadora (portátil, computadora de escritorio, servidor), algunos dispositivos, como los procesadores y la memoria RAM, se conectan directamente a la placa base, mientras que otros dispositivos, como las tarjetas complementarias, se conectan a través de un bus.
## 12.3 Procesadores
Una _unidad central de procesamiento_ (CPU o procesador) es uno de los componentes de hardware más críticos de una computadora. El procesador es el cerebro de la computadora, donde se lleva a cabo la ejecución del código y donde se realizan la mayoría de los cálculos. Está conectado (soldado) directamente a la placa base, ya que las placas base suelen estar configuradas para funcionar con tipos específicos de procesadores.

Si un sistema de hardware tiene más de un procesador, el sistema se denomina multiprocesador. Si se combina más de un procesador en un solo chip de procesador general, entonces se denomina multinúcleo.

Aunque el soporte está disponible para más tipos de procesadores en Linux que en cualquier otro sistema operativo, principalmente hay solo dos tipos de procesadores utilizados en computadoras de escritorio y servidores: _x86 y x86_64_. En un sistema x86, el sistema procesa datos de 32 bits a la vez; En un x86_64 el sistema procesa datos 64 bits a la vez. Un sistema x86_64 también es capaz de procesar datos de 32 bits a la vez en un modo compatible con versiones anteriores. Una de las principales ventajas de un sistema de 64 bits es la capacidad de trabajar con más memoria, mientras que otras ventajas incluyen una mayor eficiencia de procesamiento y una mayor seguridad.

**Intel** originó la familia de procesadores x86 en 1978 con el lanzamiento del procesador 8086. Desde entonces, Intel ha producido muchos otros procesadores que son mejoras del 8086 original; Se conocen genéricamente como procesadores x86. Estos procesadores incluyen el 80386 (i386), el 80486 (i486), la serie Pentium (i586) y la serie Pentium Pro (i686). Además de Intel, otras compañías como AMD y Cyrix también han producido procesadores compatibles con x86. Si bien Linux es capaz de soportar procesadores de la generación i386, muchas distribuciones (especialmente las que cuentan con soporte corporativo, como SUSE, Red Hat y Canonical) limitan su soporte a i686 o posterior.

La x86_64 familia de procesadores, incluidos los procesadores de 64 bits de Intel y AMD, han estado en producción desde aproximadamente el año 2000. Como resultado, casi todos los procesadores modernos que se envían hoy en día son del tipo x86_64. Si bien el hardware ha estado disponible durante más de una década, el software para soportar esta familia de procesadores ha sido mucho más lento de desarrollar. Aunque estamos en 2018 en el momento de esta actualización, sigue habiendo una serie de paquetes que todavía están disponibles para la arquitectura x86.

Para ver a qué familia pertenece la CPU del sistema actual, utilice el comando arch:

```
sysadmin@localhost:~$ arch
x86_64
```

Para obtener más información sobre la CPU, utilice el comando lscpu:

```
sysadmin@localhost:~$ lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                4
On-line CPU(s) list:   0-3                                                    
Thread(s) per core:    1                                                      
Core(s) per socket:    4                                                      
Socket(s):             1
NUMA node(s):          1                                                      
Vendor ID:             GenuineIntel 
```

La primera línea de este resultado muestra que la CPU se está utilizando en un modo de 64 bits, ya que la arquitectura informada es x86_64. La segunda línea de salida muestra que la CPU es capaz de funcionar en modo de 32 o 64 bits, por lo tanto, en realidad es una CPU de 64 bits.
## 12.4 Memoria de acceso aleatorio
La placa base suele tener ranuras donde  se puede conectar la _memoria de acceso aleatorio (RAM)_ al sistema. Los sistemas de arquitectura de 32 bits pueden usar hasta 4 gigabytes (GB) de RAM, mientras que las arquitecturas de 64 bits son capaces de direccionar y usar mucha más RAM.

En algunos casos, es posible que la RAM que tiene un sistema no sea suficiente para manejar todos los requisitos del sistema operativo. Una vez invocados, los programas se cargan en la memoria RAM junto con los datos que necesitan almacenar, mientras que las instrucciones se envían al procesador cuando se ejecutan.

Cuando el sistema se está quedando sin RAM, la memoria virtualizada llamada _espacio de intercambio_ se utiliza para almacenar temporalmente los datos en el disco. Los datos almacenados en la memoria RAM que no se han utilizado recientemente se trasladan a la sección del disco duro designada como intercambio, liberando más RAM para su uso por parte de los programas actualmente activos. Si es necesario, estos datos intercambiados se pueden volver a mover a la RAM en un momento posterior. El sistema hace todo esto automáticamente siempre que el intercambio esté configurado.

Para ver la cantidad de RAM en su sistema, incluido el espacio de intercambio, ejecute el comando free. El comando free tiene una opción -m para forzar que la salida se redondee al megabyte (MB) más cercano y una opción -g para forzar que la salida se redondee al gigabyte (GB) más cercano:

```
sysadmin@localhost:~$ free -m                                                
             total       used       free     shared    buffers     cached     
Mem:          1894        356       1537          0          25       177     
-/+ buffers/cache:        153       1741                                      
Swap:         4063          0       4063
```

El resultado muestra que este sistema tiene un total de 1.894 MB y actualmente utiliza 356 MB.

La cantidad de intercambio parece ser de aproximadamente 4 GB, aunque ninguno de ellos parece estar en uso. Esto tiene sentido porque gran parte de la RAM física está libre, por lo que no es necesario en este momento utilizar RAM virtual (espacio de intercambio).
## 12.5 Buses
Un _bus_ es una conexión de alta velocidad que permite la comunicación entre computadoras o los componentes dentro de una computadora. La placa base tiene buses que permiten que varios dispositivos se conecten al sistema, incluida la _interconexión de componentes periféricos (PCI)_ y  el _bus serie universal (USB)._ La placa base también tiene conectores para monitores, teclados y ratones.

Los diferentes tipos de sistemas utilizarán un bus de manera diferente para conectar componentes. En una computadora de escritorio o servidor, hay una placa base con el procesador, la RAM y otros componentes conectados directamente, y luego un bus de alta velocidad que permite conectar componentes adicionales a través de ranuras para tarjetas, como video, red y otros dispositivos periféricos.

Cada vez más, en el caso de los portátiles y los ordenadores ligeros/delgados/pequeños, la mayoría de los componentes del ordenador pueden estar conectados directamente a la placa base, incluido el procesador habitual, la memoria RAM, así como componentes adicionales como la tarjeta de red. En esta configuración, el bus sigue existiendo, pero efectivamente uno de los principales factores de conveniencia de poder intercambiar o actualizar dispositivos ya no es una posibilidad.

Dispositivos periféricos

_Los dispositivos periféricos_ son componentes conectados a un ordenador que permiten la entrada, salida o almacenamiento de datos. Los teclados, ratones, monitores, impresoras y discos duros se consideran periféricos y el sistema accede a ellos para realizar funciones fuera del procesamiento central. Para ver todos los dispositivos conectados por el bus PCI, el usuario puede ejecutar el comando lspci. A continuación se muestra un ejemplo de salida de este comando. Las secciones resaltadas muestran que este sistema tiene un controlador VGA (un conector de monitor), un controlador de almacenamiento SCSI (un tipo de disco duro) y un controlador Ethernet (un conector de red):

```
sysadmin@localhost:~$ lspci                                                   
00:00.0 Host bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX Host bridge (rev 01)                                                                      
00:01.0 PCI bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX AGP bridge (rev 01)                                                                        
00:07.0 ISA bridge: Intel Corporation 82371AB/EB/MB PIIX4 ISA (rev 08)        
00:07.1 IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)     
00:07.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 08)           
00:07.7 System peripheral: VMware Virtual Machine Communication Interface (rev 10)                                                                           
00:0f.0 VGA compatible controller: VMware SVGA II Adapter     
03:00.0 Serial Attached SCSI controller: VMware PVSCSI SCSI Controller (rev 02
0b:00.0 Ethernet controller: VMware VMXNET3 Ethernet Controller (rev 01)
```

Si bien el bus PCI se utiliza para muchos dispositivos internos, como tarjetas de sonido y de red, un número cada vez mayor de dispositivos externos (o periféricos) están conectados a la computadora a través de USB.

Los dispositivos conectados internamente suelen estar _enchufados en frío_, lo que significa que el sistema debe apagarse para conectar o desconectar un dispositivo. Los dispositivos USB son _de conexión en caliente_, lo que significa que se pueden conectar o desconectar mientras el sistema está funcionando.

Aunque los dispositivos USB son de conexión en caliente por diseño, es importante asegurarse de que los sistemas de archivos montados estén correctamente desmontados, o puede producirse la pérdida de datos y la corrupción del sistema de archivos.

Para mostrar los dispositivos conectados al sistema a través de USB, ejecute el comando lsusb:

```
sysadmin@localhost:~$ lsusb
Bus 001 Device 002: ID 0e0f:000b VMware, Inc. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 004: ID 0e0f:0008 VMware, Inc. 
Bus 002 Device 003: ID 0e0f:0002 VMware, Inc. Virtual USB Hub
Bus 002 Device 002: ID 0e0f:0003 VMware, Inc. Virtual Mouse
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
```
## 12.6 Discos duros
_Los discos duros_, también llamados discos duros, dispositivos de disco o dispositivos de almacenamiento, se pueden conectar al sistema de varias maneras; el controlador puede estar integrado en la placa base, en una tarjeta PCI o como un dispositivo USB. Para los propósitos de la mayoría de los sistemas Linux, un disco duro generalmente se puede definir como cualquier dispositivo de almacenamiento electromecánico o electrónico que contiene datos a los que el sistema puede acceder.

Los discos duros se dividen en una o más _particiones_. Una partición es una división lógica de un disco duro, diseñada para ocupar una gran cantidad de espacio de almacenamiento disponible y dividirlo en áreas más pequeñas. Si bien es común en Microsoft Windows tener una sola partición para cada disco duro, en las distribuciones de Linux, es común tener varias particiones por disco duro.

Algunos discos duros hacen uso de una tecnología de partición llamada _Master Boot Record (MBR),_ mientras que otros hacen uso de un tipo de partición llamado _GUID Partitioning Table (GPT)._ El tipo de partición MBR se ha utilizado desde los primeros días de la computadora personal (PC), y el tipo GPT ha estado disponible desde el año 2000.

Un término antiguo utilizado para describir un disco duro interno es _disco fijo_, ya que el disco es fijo (no extraíble). Este término dio lugar a varios nombres de comandos: los comandos fdisk, cfdisk y sfdisk, que son herramientas para trabajar con los discos particionados MBR.

Los discos GPT utilizan un tipo más nuevo de partición, que permite al usuario dividir el disco en más particiones de las que admite MBR. GPT también permite tener particiones que pueden tener más de dos terabytes (MBR no). Las herramientas para administrar discos GPT se denominan de manera similar a sus contrapartes fdisk: gdisk, cgdisk y sgdisk.

También hay una familia de herramientas que intentan soportar discos de tipo MBR y GPT. Este conjunto de herramientas incluye el comando parted y la herramienta gparted gráfica.

> Muchas distribuciones de Linux utilizan la herramienta gparted en la rutina de instalación, ya que proporciona una interfaz gráfica a las potentes opciones disponibles para crear nuevas particiones, cambiar el tamaño de las existentes (como las particiones de Windows) y muchas otras tareas avanzadas.

Los discos duros están asociados con nombres de archivo (llamados archivos de dispositivo) que se almacenan en el directorio /dev. Cada nombre de archivo de dispositivo se compone de varias partes.

- **Tipo de archivo**

El nombre del archivo tiene un prefijo en función de los diferentes tipos de discos duros. _Los discos duros IDE (Intelligent Drive Electronics_) comienzan con hd, mientras que los  discos _duros USB, SATA (Serial Advanced Technology Attachment)_ y _SCSI (Small Computer System Interface)_ comienzan con sd.

- **Orden de dispositivos**

A cada disco duro se le asigna una letra que sigue al prefijo. Por ejemplo, el primer disco duro IDE se llamaría /dev/hda y el segundo sería /dev/hdb, y así sucesivamente.

- **Partición**

Por último, a cada partición de un disco se le asigna un indicador numérico único. Por ejemplo, si un disco duro USB tiene dos particiones, podrían estar asociadas con los archivos de dispositivo /dev/sda1 y /dev/sda2.

En el ejemplo siguiente se muestra un sistema que tiene tres dispositivos sd: /dev/sda, /dev/sdb y /dev/sdc. Además, hay dos particiones en el primer dispositivo, como lo demuestran los archivos /dev/sda1 y /dev/sda2, y una partición en el segundo dispositivo, como lo demuestra el archivo /dev/sdb1:

```
root@localhost:~$ ls /dev/sd*                                               
/dev/sda /dev/sda1  /dev/sda2  /dev/sdb  /dev/sdb1  /dev/sdc
```

El comando fdisk se puede utilizar para mostrar más información sobre las particiones:

El siguiente comando requiere acceso root.

```
root@localhost:~$ fdisk -l /dev/sda                                             
Disk /dev/sda: 21.5 GB, 21474836480 bytes                                       
255 heads, 63 sectors/track, 2610 cylinders, total 41943040 sectors             
Units = sectors of 1 * 512 = 512 bytes                                          
Sector size (logical/physical): 512 bytes / 512 bytes                           
I/O size (minimum/optimal): 512 bytes / 512 bytes                               
Disk identifier: 0x000571a2                                                     
                                                                                
⁠⁠​ ⁠
   Device Boot      Start         End      Blocks   Id  System                  
/dev/sda1   *        2048    39845887    19921920   83  Linux                   
/dev/sda2        39847934    41940991     1046529    5  Extended                
/dev/sda5        39847936    41940991     1046528   82  Linux swap / Solaris
```
## 12.7 Discos de estado sólido
Si bien la frase _disco duro_ generalmente se considera que abarca los dispositivos de disco giratorio tradicionales, también puede referirse a las unidades o discos de estado sólido más nuevos y muy diferentes.

Considere la diferencia entre un disco duro de plato giratorio tradicional y una unidad de memoria USB. El primero tiene literalmente platos de discos giratorios que son leídos por los cabezales de las unidades, y los discos giratorios están dispuestos para aprovechar la naturaleza giratoria del dispositivo. Los datos se escriben (y leen) en largas cadenas de bloques secuenciales que un cabezal de unidad encuentra a medida que el plato gira.

_Los discos de estado sólido_ son esencialmente unidades de memoria USB de mayor capacidad en construcción y función. Como no hay partes móviles, ni discos giratorios, solo ubicaciones de memoria para ser leídas por el controlador, un disco de estado sólido es medible y visiblemente más rápido en acceder a la información almacenada en sus chips de memoria.

Un disco de estado sólido está controlado por un procesador integrado que toma las decisiones sobre dónde y cómo se escriben y leen los datos de los chips de memoria cuando se solicitan.

Las ventajas de un disco de estado sólido incluyen un menor uso de energía, ahorro de tiempo en el arranque del sistema, cargas de programas más rápidas y menos calor y vibración sin partes móviles. Las desventajas incluyen costos más altos en comparación con los discos duros giratorios, menor capacidad debido al mayor costo y, si se suelda directamente en la placa base / placa base, no se puede actualizar intercambiando la unidad.

> A los efectos de este curso, y para su comprensión, nos referiremos a todos los discos, giratorios o de estado sólido, como discos duros, a menos que haya una distinción importante que nos obligue a diferenciar entre los dos.
## 12.8 Unidades ópticas
_Las unidades_ ópticas, a menudo denominadas CD-ROM, DVD o Blu-Ray, son medios de almacenamiento extraíbles. Mientras que algunos dispositivos utilizados con discos ópticos son de solo lectura, otros son capaces de grabar (escribir en) discos, cuando se utiliza un tipo de disco grabable. Existen varios estándares para discos grabables y regrabables, como CD-R, CD+R, DVD+RW y DVD-RW.

El lugar donde se montan estos discos extraíbles en el sistema de archivos es una consideración importante para un administrador de Linux. Las distribuciones modernas suelen montar los discos en la carpeta /media, mientras que las distribuciones más antiguas suelen montarlos en la carpeta /mnt. Por ejemplo, se puede montar una unidad de memoria USB en la ruta /media/usbthumb.

Tras el montaje, la mayoría de las interfaces GUI solicitan al usuario que realice alguna acción, como abrir el contenido del disco en un explorador de archivos o iniciar un programa multimedia. Cuando el usuario haya terminado de usar el disco, es necesario desmontarlo usando un menú o el comando umount.
## 12.9 Gestión de dispositivos
Linux, por naturaleza, tiene una serie de distribuciones o versiones, una gran parte de las cuales se centran en el hardware convencional, mientras que otras se ejecutan en tipos o modelos de hardware relativamente oscuros. Es seguro decir que hay una distribución de Linux diseñada específicamente para casi todas las plataformas de hardware modernas, y también para muchas plataformas históricas.

Cada una de estas plataformas de hardware tiene una gran variedad en los componentes que están disponibles. Además de varios tipos diferentes de discos duros, hay muchas tarjetas gráficas, monitores e impresoras diferentes. Con la popularidad de los dispositivos USB, como los dispositivos de almacenamiento USB, las cámaras y los teléfonos móviles, el número de dispositivos disponibles que querría conectar a un sistema Linux puede ascender a miles.

La gran cantidad de dispositivos diferentes plantea problemas, ya que estos dispositivos de hardware suelen necesitar _controladores_, software que les permite comunicarse con el sistema operativo instalado.

El controlador puede compilarse como parte del kernel de Linux, cargarse en el kernel como un módulo o cargarse mediante un comando de usuario o una aplicación. Los dispositivos externos, como escáneres e impresoras, suelen tener sus controladores cargados por una aplicación y estos controladores, a su vez, se comunican a través del dispositivo a través del kernel a través de una interfaz como USB.

Los fabricantes de hardware a menudo proporcionan este software, pero generalmente para Microsoft Windows, no para Linux. Esto ha ido cambiando con la creciente popularidad de Linux en general, pero la cuota de mercado de escritorio de Linux sigue siendo un tercero muy distante con respecto a los sistemas de Microsoft y Apple.

En parte debido a que el soporte de los proveedores es relativamente escaso, hay una gran cantidad de apoyo de la comunidad por parte de los desarrolladores que se esfuerzan por proporcionar controladores para los sistemas Linux. Aunque no todo el hardware tiene los controladores necesarios, hay una cantidad decente que los tiene, lo que hace que sea un desafío para los usuarios y administradores de Linux encontrar los controladores correctos o elegir hardware que tenga algún nivel de soporte en Linux.

Para tener éxito en la habilitación de dispositivos en Linux, es mejor verificar la distribución de Linux para ver si el dispositivo está certificado para funcionar con esa distribución. Las distribuciones comerciales como Red Hat y SUSE tienen páginas web dedicadas a enumerar el hardware que está certificado o aprobado para trabajar con su software.

Consejos adicionales para tener éxito con la conexión de sus dispositivos: evite los dispositivos nuevos o altamente especializados y consulte con el proveedor del dispositivo para ver si es compatible con Linux antes de realizar una compra.
## 12.10 Dispositivos de visualización de video
Para mostrar la salida al monitor, el sistema informático debe tener un dispositivo de _visualización de video_ (tarjeta de video) y un monitor. Los dispositivos de visualización de video a menudo están integrados o conectados directamente a la placa base, aunque también se pueden conectar a través de las ranuras de bus PCI en la placa base.

Con la gran variedad de proveedores de dispositivos de video, cada dispositivo de visualización de video generalmente requiere un controlador propietario proporcionado por el proveedor. Todos los controladores, pero especialmente los controladores de dispositivos de video, deben escribirse para el sistema operativo específico, algo que se hace comúnmente para Microsoft Windows, pero no siempre para Linux. Afortunadamente, la mayoría de los proveedores de pantallas de video más grandes ahora brindan al menos algún nivel de soporte para Linux. La comunidad Linux ha realizado una cantidad increíble de trabajo desarrollando controladores estándar para tarjetas de video.

> Los problemas de soporte de video que ha experimentado Linux se centran principalmente en el mercado de escritorio para Linux. La cuota de mercado de los servidores no se ve afectada en su mayoría por la GUI, ya que la gran mayoría de las implementaciones de servidores Linux son en modo texto.

Hay tres tipos de cables de video comúnmente utilizados:

- el  cable analógico de matriz de _gráficos de video (VGA) de_ 15 pines, más antiguo pero que aún se usaba
- la  interfaz _digital Visual Interface (DVI)_ de 29 pines más reciente
- la interfaz multimedia de alta definición (HDMI), muy utilizada, que admite resoluciones de hasta 4K o Ultra HD, y tiene 19 o 29 pines
- la interfaz de pantalla digital más nueva, el DisplayPort (DP) de 20 pines. También su contraparte miniaturizada, el Mini DisplayPort, utilizado principalmente para productos de Apple.

Para que los monitores funcionen correctamente con los dispositivos de visualización de vídeo, deben ser capaces de soportar la misma resolución que el dispositivo de visualización de vídeo. Normalmente, el software que controla el dispositivo de visualización de video puede detectar automáticamente la resolución máxima que la pantalla y el monitor de video pueden admitir y establecer la resolución de la pantalla en ese valor.
## 12.11 Fuentes de alimentación
_Las fuentes de alimentación_ son los dispositivos que convierten la corriente alterna (120v, 240v) en corriente continua, que el ordenador utiliza a varios voltajes (3,3v, 5v, 12v). Las fuentes de alimentación generalmente no son programables; sin embargo, su correcto funcionamiento tiene un gran impacto en el resto del sistema.

Aunque normalmente no están diseñados como supresores de sobretensiones, estos dispositivos a menudo protegen la computadora de las fluctuaciones de voltaje provenientes de la fuente de alimentación. Es aconsejable que un administrador de red elija una fuente de alimentación en función de la calidad y no del precio, ya que una fuente de alimentación defectuosa puede provocar daños significativos en un sistema informático.

Los sistemas de escritorio, torre de servidores, rack y autónomos son más vulnerables a las fluctuaciones de energía que los sistemas portátiles. Si la energía fluctúa, puede causar muchos estragos en los sistemas que no están basados en baterías, mientras que una computadora portátil simplemente sigue extrayendo energía de su batería interna hasta que se agota.