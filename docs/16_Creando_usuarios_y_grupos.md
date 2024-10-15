# 16. Creando Usuarios y Grupos
## 16.1 Introducción
Durante el proceso de instalación, la mayoría de los instaladores crean un usuario normal y le otorgan a este usuario el permiso para ejecutar comandos administrativos con sudo o requieren que se configure la contraseña de la cuenta de usuario raíz como parte del proceso de instalación. La mayoría de los sistemas Linux están configurados para permitir que un usuario sin privilegios (no root) inicie sesión, así como para tener la capacidad de ejecutar comandos de manera efectiva como usuario root, ya sea directa o indirectamente.

Si la computadora va a ser utilizada por una sola persona, entonces tener solo una cuenta de usuario regular podría ser suficiente. Sin embargo, si una computadora debe ser compartida por varias personas, entonces es deseable tener una cuenta separada para cada persona que la use. Hay varias ventajas para las personas que tienen sus propias cuentas separadas:

Las cuentas se pueden usar para otorgar acceso selectivo a archivos o servicios. Por ejemplo, el usuario de cada cuenta tiene un directorio de inicio independiente al que, por lo general, no pueden acceder los demás usuarios.

El comando sudo se puede configurar para otorgar la capacidad de ejecutar comandos administrativos seleccionados. Si se requiere que los usuarios utilicen el comando sudo para realizar comandos administrativos, el sistema registra cuándo los usuarios realizan estos comandos.

Cada cuenta puede tener membresías de grupos y derechos asociados, lo que permite una mayor flexibilidad de administración.

En algunas distribuciones, la creación de una nueva cuenta de usuario también crea automáticamente una cuenta de grupo para el usuario, denominada _Grupo Privado de Usuario (UPG)._ En estos sistemas, el grupo y el nombre de usuario serían los mismos, y el único miembro de este nuevo grupo sería el nuevo usuario.

En el caso de las distribuciones que no crean un UPG, a los nuevos usuarios se les suele asignar el grupo de usuarios como grupo principal. El administrador puede crear manualmente cuentas de grupo que sean privadas para el usuario, pero es más común que el administrador cree grupos para varios usuarios que necesitan colaborar. Las cuentas de usuario se pueden modificar en cualquier momento para agregarlas o eliminarlas de las membresías de cuentas de grupo, pero los usuarios deben pertenecer al menos a un grupo para usarlas como su grupo principal.

Antes de empezar a crear usuarios, debe planear cómo usar los grupos. Los usuarios se pueden crear con membresías en grupos que ya existen, o los usuarios existentes se pueden modificar para tener membresías en grupos existentes.

Si ya ha planeado qué usuarios y grupos desea, es más eficiente crear primero sus grupos y crear sus usuarios con sus membresías de grupos. De lo contrario, si primero crea los usuarios y, a continuación, los grupos, deberá realizar un paso adicional para modificar los usuarios y convertirlos en miembros de los grupos.
## 16.2 Grupos
La razón más común para crear un grupo es proporcionar una forma para que los usuarios compartan archivos. Por ejemplo, si varias personas trabajan juntas en el mismo proyecto y necesitan poder colaborar en documentos almacenados en archivos para el proyecto. En este escenario, el administrador puede hacer que estas personas sean miembros de un grupo común, cambiar la propiedad del directorio al nuevo grupo y establecer permisos en el directorio que permitan a los miembros del grupo acceder a los archivos.

Después de crear o modificar un grupo, puede verificar los cambios consultando la información de configuración del grupo en el archivo /etc/group con el comando grep. Si trabaja con servicios de autenticación basados en red, el comando getent puede mostrarle grupos locales y basados en red.

```
grep pattern filename
getent database record
```

Para uso local, estos comandos muestran el mismo resultado, en este caso para el grupo raíz:

```
root@localhost:~# grep root /etc/group
root:x:0:
root@localhost:~# getent group root
root:x:0:
```
### 16.2.1 Creación de un grupo
El usuario root puede ejecutar el comando groupadd para crear un nuevo grupo. El comando solo requiere el nombre del grupo que se va a crear. La opción -g se puede utilizar para especificar un id de grupo para el nuevo grupo:

```
root@localhost:~# groupadd -g 1005 research
root@localhost:~# grep research /etc/group
research:x:1005:
```

Si no se proporciona la opción -g, el comando groupadd proporcionará automáticamente un GID para el nuevo grupo. Para lograr esto, el comando groupadd examina el archivo /etc/group y utiliza un número que es un valor superior al número GID más alto actual. La ejecución de los siguientes comandos ilustra esto:

```
root@localhost:~# groupadd development
root@localhost:~# grep development /etc/group
development:x:1006:
```
#### 16.2.1.1 Consideraciones sobre el ID de grupo
En algunas distribuciones de Linux, particularmente aquellas basadas en Red Hat, cuando  _se crea un ID de usuario (UID),_ también se crea un grupo privado de usuario (UPG) con ese usuario como su único miembro. En estas distribuciones, se supone que el UID y el ID del UPG deben coincidir (ser el mismo número).

Por lo tanto, debe evitar crear GID en los mismos rangos numéricos en los que espera crear UID, para evitar un conflicto entre un GID que cree y un número UPG que se crea para que coincida con un UID.

Los GIDs de menos de 500 (RedHat) o 1000 (Debian) están reservados para el uso del sistema. Puede haber ocasiones en las que desee asignar un valor de GID más bajo. Para lograr esto, use la opción -r que asigna al nuevo grupo un GID que es menor que el GID estándar más bajo:

```
root@localhost:~# groupadd -r sales
root@localhost:~# getent group sales
sales:x:999:
```
#### 16.2.1.2 Consideraciones sobre la nomenclatura de grupos
Seguir estas pautas para los nombres de grupo puede ayudar a seleccionar un nombre de grupo que sea portátil (que funcione correctamente con otros sistemas o servicios):

- El primer carácter del nombre debe ser un carácter _ de subrayado o un carácter alfabético de la A a la Z en minúsculas.
- Se permiten hasta 32 caracteres en la mayoría de las distribuciones de Linux, pero usar más de 16 puede ser problemático, ya que algunas distribuciones pueden no aceptar más de 16.
- Después del primer carácter, los caracteres restantes pueden ser alfanuméricos, un guión - o un carácter de subrayado _.
- El último carácter no debe ser un guión.

> Desafortunadamente, estas pautas no siempre se aplican. El problema no es que el comando groupadd no falle necesariamente, sino que es posible que otros comandos o servicios del sistema no funcionen correctamente.
### 16.2.2 Modificación de un grupo
El comando groupmod se puede usar para cambiar el nombre de un grupo con la opción -n o cambiar el GID para el grupo con la opción -g.

Cambiar el nombre del grupo puede confundir a los usuarios que estaban familiarizados con el nombre anterior y no han sido informados del nuevo nombre. Sin embargo, cambiar el nombre del grupo no causará ningún problema con el acceso a los archivos, ya que los archivos son propiedad de GIDs, no de nombres de grupo. Por ejemplo:

```
root@localhost:~# ls -l index.html
-rw-r-----. 1 root sales 0 Aug  1 13:21 index.html
root@localhost:~# groupmod -n clerks sales
root@localhost:~# ls -l index.html
-rw-r-----. 1 root clerks 0 Aug  1 13:21 index.html
```

> El archivo del ejemplo anterior no está disponible en el entorno de máquina virtual de este curso.

Después del comando groupmod anterior, el archivo index.html tiene un nombre de propietario de grupo diferente. Sin embargo, todos los usuarios que estaban en el grupo de ventas ahora están en el grupo de empleados, por lo que todos esos usuarios pueden seguir teniendo acceso al archivo index.html. De nuevo, esto se debe a que el sistema define el grupo por el GID, no por el nombre del grupo.

Por otro lado, si cambia el GID de un grupo, todos los archivos que estaban asociados a ese grupo ya no estarán asociados a ese grupo. De hecho, todos los archivos que estaban asociados a ese grupo ya no estarán asociados a _ningún_ nombre de grupo. En su lugar, estos archivos serán propiedad de un GID únicamente, como se muestra a continuación:

```
root@localhost:~# groupmod -g 10003 clerks
root@localhost:~# ls -l index.html
-rw-r-----. 1 root 491 13370 Aug  1 13:21 index.html
```

> Estos archivos sin nombre de grupo se denominan _archivos huérfanos_. Para buscar todos los archivos que son propiedad de un GID (no asociado con un nombre de grupo) use la opción -nogroup del comando find:

```
root@localhost:~# find / -nogroup 
/root/index.html
```
### 16.2.3 Eliminación de un grupo
Si decide eliminar un grupo con el comando groupdel, tenga en cuenta que todos los archivos que sean propiedad de ese grupo quedarán huérfanos.

Solo se pueden eliminar los grupos complementarios, por lo que si algún grupo es el grupo principal de cualquier usuario, no se puede eliminar. El administrador puede modificar qué grupo es el grupo principal de un usuario, por lo que un grupo que se estaba utilizando como grupo principal se puede convertir en un grupo complementario y, a continuación, se puede eliminar.

Siempre que el grupo que se va a eliminar no sea el grupo principal de un usuario, la eliminación del grupo se realiza mediante el comando groupdel junto con el nombre del grupo:

```
root@localhost:~# groupdel clerks
```
## 16.3 Usuarios
La información de la cuenta de usuario se almacena en el archivo /etc/passwd y la información de autenticación del usuario (datos de contraseña) se almacena en el archivo /etc/shadow. La creación de un nuevo usuario se puede lograr agregando manualmente una nueva línea a cada uno de estos archivos, pero esa no es generalmente la técnica recomendada.

Mediante el uso de un comando apropiado para agregar un nuevo usuario, estos archivos se pueden modificar de forma automática y segura. Si tuviera que modificar estos archivos manualmente, correría el riesgo de cometer un error que podría impedir que todos los usuarios puedan iniciar sesión normalmente.

Antes de comenzar a crear usuarios para el sistema, debe verificar o establecer valores prácticos que se utilizan de forma predeterminada con el comando useradd. Esta configuración se puede encontrar en los archivos de configuración que utiliza el comando useradd.

Asegurarse de que los valores de estos archivos de configuración sean razonables antes de agregar usuarios puede ayudarle a ahorrar el tiempo y la molestia de tener que corregir la configuración de la cuenta de usuario después de agregar los usuarios.
### 16.3.1 Archivos de configuración de usuario
La opción -D del comando useradd le permite ver o cambiar algunos de los valores predeterminados utilizados por el comando useradd. Los valores mostrados por useradd -D también se pueden ver o actualizar manipulando el archivo /etc/default/useradd:

```
root@localhost:~# useradd -D 
GROUP=100
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes
```

A continuación se describe cada uno de estos valores:

- **Grupo**

```
GROUP=100
```

En las distribuciones que no utilizan UPG, este es el grupo principal predeterminado para un nuevo usuario, si no se especifica uno con el comando useradd. Este suele ser el grupo de usuarios con un GID de 100.

Esta configuración afecta al  campo _de ID de grupo principal_ del archivo /etc/passwd resaltado a continuación:
![](img/20241011100338.png)

La opción -g del comando useradd le permite utilizar un grupo principal diferente al predeterminado al crear una nueva cuenta de usuario.

- **Hogar**

```
HOME=/home
```

El directorio /home es el _directorio base predeterminado_ en el que se crea el nuevo directorio principal del usuario. Esto significa que un usuario con un nombre de cuenta de bob tendría un directorio de inicio de /home/bob.

Esta configuración afecta al campo del directorio de inicio del archivo /etc/passwd resaltado a continuación:

![](img/20241011100418.png)

La opción -b del comando useradd le permite utilizar un grupo de directorios base diferente al predeterminado al crear una nueva cuenta de usuario.

- **Inactivo**

```
INACTIVE=-1
```

Este valor representa el número de días después de que caduque la contraseña que la cuenta está deshabilitada. Un valor de -1 significa que esta función no está habilitada de forma predeterminada y no se proporciona ningún valor "inactivo" para las cuentas nuevas de forma predeterminada.

Esta configuración afecta al  _campo inactivo_ del archivo /etc/shadow resaltado a continuación:

![](img/20241011100454.png)

La opción -f del comando useradd le permite utilizar un valor INACTIVE diferente al predeterminado al crear una nueva cuenta de usuario.

- **Expirar**

```
EXPIRE=
```

De forma predeterminada, no hay ningún valor establecido para la fecha de vencimiento. Por lo general, se establece una fecha de vencimiento en una cuenta individual, no en todas las cuentas de forma predeterminada.

Por ejemplo, si tuviera un contratista contratado para trabajar hasta el final del día el 1 de noviembre de 2019, podría asegurarse de que no pueda iniciar sesión después de esa fecha mediante el campo CADUCAR.

Esta configuración afecta al  _campo de caducidad_ del archivo /etc/shadow resaltado a continuación:

![](img/20241011100551.png)

La opción -e del comando useradd le permite utilizar un valor EXPIRE diferente al predeterminado al crear una nueva cuenta de usuario.

- **Shell**

```
SHELL=/bin/bash
```

La configuración de SHELL indica el shell predeterminado para un usuario cuando inicia sesión en el sistema.

Esta configuración afecta al  campo de shell del archivo /etc/passwd resaltado a continuación:

![](img/20241011100626.png)

La opción -s del comando useradd le permite utilizar un shell de inicio de sesión diferente al predeterminado al crear una nueva cuenta de usuario.

- **Directorio skeleton**

```
SKEL=/etc/skel
```

El valor SKEL determina qué _directorio esqueleto_ tiene su contenido copiado en el directorio principal del nuevo usuario. El contenido de este directorio se copia en el directorio principal del nuevo usuario y se le otorga al nuevo usuario la propiedad de los nuevos archivos.

Esta configuración proporciona a los administradores una manera fácil de rellenar una nueva cuenta de usuario con archivos de configuración de claves.

La opción -k del comando useradd le permite utilizar un directorio SKEL diferente al predeterminado al crear una nueva cuenta de usuario.

- **Crear cola de correo**

```
CREATE_MAIL_SPOOL=yes
```

Una cola de _correo_ es un archivo donde se coloca el correo electrónico entrante.

Actualmente, el valor para crear una cola de correo es sí, lo que significa que los usuarios están configurados de forma predeterminada con la capacidad de recibir y almacenar correo local. Si no planea utilizar el correo local, este valor podría cambiarse a no.

Para modificar uno de los valores predeterminados de useradd, el archivo /etc/default/useradd se puede editar con un editor de texto. Otra técnica (más segura) es usar el comando useradd -D.

Por ejemplo, si desea permitir que los usuarios tengan contraseñas caducadas con las que puedan iniciar sesión durante un máximo de treinta días, puede ejecutar:

```
root@localhost:~# useradd -D -f 30
root@localhost:~# useradd -D
GROUP=100
HOME=/home
INACTIVE=30
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes
```
### 16.3.2 Archivos de configuración de usuario
El archivo /etc/login.defs también contiene valores que se aplican de forma predeterminada a los nuevos usuarios que se crean con el comando useradd. A diferencia del archivo /etc/default/useradd, el archivo /etc/login.defs suele ser editado directamente por el administrador para alterar sus valores.

Este archivo contiene muchos comentarios y líneas en blanco, por lo que para ver solo las líneas que no son comentarios o líneas en blanco (los ajustes de configuración reales), puede usar el siguiente comando grep:

```
root@localhost:~#  grep -Ev '^#|^$' /etc/login.defs
MAIL_DIR	/var/mail/spool
PASS_MAX_DAYS	99999
PASS_MIN_DAYS	0
PASS_MIN_LEN	5
PASS_WARN_AGE	7
UID_MIN			  500
UID_MAX			60000
GID_MIN			  500
GID_MAX			60000
CREATE_HOME	yes
UMASK           077
USERGROUPS_ENAB yes
ENCRYPT_METHOD SHA512
MD5_CRYPT_ENAB no
```

El ejemplo anterior representa un archivo /etc/login.defs típico de la distribución de CentOS 6 con sus valores. A continuación se describe cada uno de estos valores:

- **Directorio de correo**

```
MAIL_DIR		/var/mail/spool
```

El directorio en el que se crea el archivo de cola de correo del usuario.

- **Contraseña Días máximos**

```
PASS_MAX_DAYS	99999
```

Esta configuración determina el número máximo de días que un usuario puede seguir usando la misma contraseña. Dado que el valor predeterminado es 99999 días (más de 200 años), significa que los usuarios nunca tienen que cambiar su contraseña.

Las organizaciones con políticas efectivas para mantener contraseñas seguras suelen cambiar este valor a 60 o 30 días.

Esta configuración afecta a la configuración predeterminada del archivo /etc/shadow resaltado a continuación:

![](img/20241011101025.png)

- **Contraseña Días Mínimos**

```
PASS_MIN_DAYS	0
```

Con esto establecido en un valor predeterminado de cero, el tiempo más corto que un usuario debe mantener una contraseña es cero días, lo que significa que puede cambiar inmediatamente una contraseña que acaba de establecer.

Si el valor PASS_MIN_DAYS se estableció en tres días, después de establecer una nueva contraseña, el usuario tendría que esperar tres días antes de poder cambiarla de nuevo.

Esta configuración afecta a la configuración predeterminada del archivo /etc/shadow resaltado a continuación:

![](img/20241011101048.png)

- **Longitud mínima de la contraseña**

```
PASS_MIN_LEN	5
```

Indica el número mínimo de caracteres que debe contener una contraseña.

- **Advertencia de contraseña**

```
PASS_WARN_AGE	7
```

Este es el valor predeterminado para el campo de advertencia. A medida que un usuario se acerca al número máximo de días que puede usar su contraseña, el sistema comprueba si es el momento de comenzar a advertir al usuario sobre el cambio de su contraseña al iniciar sesión.

Esta configuración afecta a la configuración predeterminada del archivo /etc/shadow resaltado a continuación:

![](img/20241011101138.png)

- **UID mínimo**

```
UID_MIN			  500
```

El UID_MIN determina el primer UID que se asigna a un usuario normal. Cualquier UID inferior a este valor sería para una cuenta del sistema o para la cuenta raíz.

- **UID máximo**

```
UID_MAX			60000
```

Técnicamente, un UID podría tener un valor de más de cuatro mil millones. Para obtener la máxima compatibilidad, se recomienda dejarlo en su valor predeterminado de 60000.

- **GID Mínimo**

```
GID_MIN			  500
```

El GID_MIN determina el primer GID que se asigna a un grupo ordinario. Cualquier grupo con un GID menor que este valor sería un grupo del sistema o el grupo raíz.

- **GID Máximo**

```
GID_MAX			60000
```

Un GID, al igual que un UID, podría tener un valor de más de cuatro mil millones. Cualquiera que sea el valor que use para su UID_MAX, debe usarse para GID_MAX para apoyar a UPG.

- **Directorio de inicio**

```
CREATE_HOME	yes
```

El valor de esto determina si se crea o no un nuevo directorio para el usuario cuando se crea su cuenta.

- **Umask**

```
UMASK           077
```

UMASK funciona en el momento en que se crea el directorio principal del usuario; Determina qué permisos predeterminados se colocan en este directorio. El uso del valor predeterminado de 077 para UMASK significa que solo el propietario del usuario tiene algún tipo de permiso para acceder a su directorio.

> El valor UMASK se tratará con más detalle en el capítulo sobre permisos.

- **UPG**

```
USERGROUPS_ENAB yes
```

En las distribuciones que cuentan con un grupo privado para cada usuario, como se muestra en este ejemplo de CentOS, el USERGROUPS_ENAB tendrá un valor de yes. Si no se utiliza UPG en la distribución, tendrá un valor de no.

- **Encriptación**

```
ENCRYPT_METHOD SHA512
```

El método de cifrado que se utiliza para cifrar las contraseñas de los usuarios en el archivo /etc/shadow. La configuración ENCRYPT_METHOD anula la configuración MD5_CRYPT_ENAB.

- **Cifrado (en desuso)**

```
MD5_CRYPT_ENAB no
```

Esta configuración obsoleta originalmente permitía al administrador especificar el uso del cifrado MD5 de contraseñas en lugar del cifrado DES original. Ha sido reemplazado por el entorno ENCRYPT_METHOD.
### 16.3.3 Consideraciones sobre la cuenta
La creación de una cuenta de usuario para su uso con un sistema Linux puede requerir que recopile varios fragmentos de información. Si bien todo lo que puede ser necesario es el nombre de la cuenta, también es posible que desee planificar el UID, el grupo principal, los grupos complementarios, el directorio principal, el directorio esqueleto y el shell que se va a utilizar. Al planear estos valores, tenga en cuenta lo siguiente:

**Nombre de usuario**

El único argumento requerido para el comando useradd es el nombre que desea que tenga la cuenta. El nombre de usuario debe seguir las mismas pautas que para los nombres de grupo. Seguir estas pautas puede ayudarlo a seleccionar un nombre de usuario que sea portable:

- El primer carácter del nombre debe ser un carácter _ de subrayado o un carácter alfabético de la A a la Z en minúsculas.
- Se permiten hasta 32 caracteres en la mayoría de las distribuciones de Linux, pero usar más de 16 puede ser problemático, ya que algunas distribuciones pueden no aceptar más de 16.
- Después del primer carácter, los caracteres restantes pueden ser alfanuméricos, un guión - o un carácter de subrayado _.
- El último carácter no debe ser un guión.

Si el usuario necesita acceder a varios sistemas, generalmente se recomienda que el nombre de la cuenta sea el mismo en esos sistemas. El nombre de la cuenta debe ser único para cada usuario.

```
root@localhost:~# useradd jane
```

**Identificador de usuario (UID)**

Una vez que se crea un usuario con un UID específico, el sistema generalmente incrementa el UID en uno para el siguiente usuario que cree. Si está conectado a una red con otros sistemas, es posible que desee asegurarse de que este UID sea el mismo en todos los sistemas para ayudar a proporcionar un acceso coherente.

Agregar la opción -u al comando useradd le permite especificar el número UID. Por lo general, los UID pueden oscilar entre cero y más de cuatro mil millones, pero para una mayor compatibilidad con sistemas más antiguos, el valor máximo recomendado de UID es de 60 000.

```
root@localhost:~# useradd -u 1000 jane
```

El usuario root tiene un UID de 0, lo que permite que esa cuenta tenga privilegios especiales. Cualquier cuenta con un UID de 0 podría actuar efectivamente como administrador.

Las cuentas del sistema se utilizan generalmente para ejecutar servicios en segundo plano denominados _demonios_. Al no tener servicios que se ejecuten como usuario raíz, la cantidad de daño que se puede causar con una cuenta de servicio comprometida es limitada. Las cuentas del sistema que usan los servicios generalmente usan UID que se encuentran en el  rango reservado. Una cuenta del sistema que es una excepción a esta regla es el usuario nfsnobody, que tiene un UID de 65534.

El intervalo reservado que se usa para las cuentas de servicio se ha expandido con el tiempo. Inicialmente, era para UID entre 1 y 99. Luego, se expandió a estar entre 1 y 499. La tendencia actual entre las distribuciones es que las cuentas del sistema son cualquier cuenta que tenga un UID entre 1 y 999, pero el rango 1-499 también se sigue utilizando comúnmente.

Al configurar un nuevo sistema, se recomienda iniciar UID no inferiores a 1000 para asegurarse de que haya suficientes UID disponibles para muchos servicios del sistema y darle la capacidad de crear muchos GID en el rango reservado.

**Grupo Primario**

En las distribuciones que incluyen UPG, este grupo se crea automáticamente con un GID y un nombre de grupo que coincide con el UID y el nombre de usuario de la cuenta de usuario recién creada. En las distribuciones que no utilizan UPG, el grupo principal suele ser el grupo de usuarios con un GID de 100.

Para especificar un grupo principal con el comando useradd, utilice la opción -g con el nombre o GID del grupo. Por ejemplo, para especificar usuarios como grupo principal:

```
root@localhost:~# useradd -g users jane
```

**Grupo Suplementario**

Para hacer que el usuario sea miembro de uno o más grupos suplementarios, la opción -G se puede utilizar para especificar una lista separada por comas de nombres de grupos o números. Por ejemplo, para especificar ventas e investigación como grupos complementarios:

```
root@localhost:~# useradd -G sales,research jane
```

**Directorio de inicio**

De forma predeterminada, la mayoría de las distribuciones crean el directorio principal del usuario con el mismo nombre que la cuenta de usuario debajo del directorio base especificado en la configuración HOME del archivo /etc/default/useradd, que normalmente especifica el directorio /home. Por ejemplo, si se crea una cuenta de usuario llamada jane, el nuevo directorio principal del usuario sería /home/jane.

```
root@localhost:~# useradd jane
root@localhost:~# grep '/home/jane' /etc/passwd
jane:x:1008:1010::/home/jane:/bin/sh
```

Hay varias opciones para el comando useradd que pueden afectar a la creación del directorio principal del usuario:

- Si CREATE_HOME se establece en no o esta configuración no está presente, el directorio no se creará automáticamente. De lo contrario, la opción -M se utiliza para especificar al comando useradd que no debe crear el directorio principal, incluso si CREATE_HOME establece en yes.

- Si la configuración de CREATE_HOME en el archivo /etc/login.defs se establece en yes, el directorio de inicio se crea automáticamente. De lo contrario, la opción -m se puede usar para _crear_ el directorio de inicio.

```
root@localhost:~# useradd -m jane
root@localhost:~# ls -ld /home/jane                                             
drwxr-xr-x 2 jane jane 4096 Dec 18 19:14 /home/jane
```

- La opción -b le permite especificar un  directorio _base diferente_  en el que se crea el directorio principal del usuario. Por ejemplo, a continuación se crea la cuenta de usuario jane con un /test/jane creado como directorio principal del usuario:

```
root@localhost:~# useradd -mb /test jane
root@localhost:~# ls -ld /test/Jane
drwxr-xr-x 2 jane jane 4096 Dec 18 19:16 /test/jane
```

- La opción -d le permite especificar un directorio existente o un nuevo directorio de inicio para crear para el usuario. Debe ser una _ruta de acceso completa_ para el directorio principal del usuario. Por ejemplo, a continuación se crea la cuenta de usuario jane con un /test/jane creado como directorio principal del usuario:

```
root@localhost:~# useradd -md /test/jane jane
root@localhost:~# ls -ld /test/jane
drwxr-xr-x 2 jane jane 4096 Dec 18 19:19 /test/jane
```

- La opción -k especifica un directorio esqueleto diferente. Cuando se utiliza la opción -k, se requiere la opción -m.

**Directorio skeleton**

De forma predeterminada, el contenido del directorio /etc/skel se copia en el directorio principal del nuevo usuario. Los archivos resultantes también son propiedad del nuevo usuario. Al usar la opción -k con el comando useradd, el contenido de un directorio diferente se puede usar para rellenar el directorio principal de un nuevo usuario. Al especificar el directorio esqueleto con la opción -k, se debe usar la opción -m o, de lo contrario, el comando useradd fallará con un error.

En el ejemplo siguiente se utiliza /home/sysadmin como directorio esqueleto:

```
root@localhost:~# useradd -mk /home/sysadmin jane
root@localhost:~# ls /home/jane
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```

**Shell**

Si bien el shell predeterminado se especifica en el archivo /etc/default/useradd, también se puede anular con el comando useradd usando la opción -s en el momento de la creación de la cuenta:

```
root@localhost:~# useradd -s /bin/bash jane
```

Es común especificar el shell /sbin/nologin para las cuentas que se van a utilizar como cuentas del sistema.

> El campo de comentarios, originalmente llamado campo General Electric Comprehensive Operating System (GECOS), se usa normalmente para contener el nombre completo del usuario. Muchos programas gráficos de inicio de sesión muestran el valor de este campo en lugar del nombre de la cuenta. La opción -c del comando useradd permite especificar el valor de este campo.

```
root@localhost:~# useradd -c 'Jane Doe' jane 
```
### 16.3.4 Creación de un usuario
Una vez que haya verificado los valores predeterminados que se van a utilizar y haya recopilado la información sobre el usuario, estará listo para crear una cuenta de usuario. Un ejemplo de un comando useradd que utiliza algunas opciones es el siguiente:

```
root@localhost:~# useradd -u 1009 -g users -G sales,research -m -c 'Jane Doe' jane 
```

En este ejemplo del comando useradd se crea un usuario con un UID de 1009, un grupo principal de usuarios, membresías complementarias en los grupos de ventas e investigación, un comentario de "Jane Doe" y un nombre de cuenta de jane.

Después de ejecutar el comando anterior, la información sobre la cuenta de usuario jane se agrega automáticamente a los archivos /etc/passwd y /etc/shadow, mientras que la información sobre el acceso a grupos suplementarios se agrega automáticamente a los archivos /etc/group y /etc/gshadow:

```
root@localhost:~# grep jane /etc/passwd
jane:x:1009:100:Jane Doe:/home/jane:/bin/sh
root@localhost:~# grep jane /etc/shadow
jane:!:17883:0:99999:7:30::
root@localhost:~# grep jane /etc/group
research:x:1005:jane
sales:x:999:jane
root@localhost:~# grep jane /etc/gshadow
research:!::jane
sales:!::jane
```

¡Ten en cuenta que la cuenta aún no tiene una contraseña válida!

Además, si el CREATE_MAIL_SPOOL se establece en yes, se crea el archivo de cola de correo /var/spool/mail/jane:

```
root@localhost:~# ls /var/spool/mail 
jane root rpc sysadmin
```

Finalmente, debido a que se usa la opción -m, el directorio /home/jane se crea con permisos que solo permiten el acceso del usuario jane, y el contenido del directorio /etc/skel se copiaría en el directorio:

```
root@localhost:~# ls /home
jane sysadmin
```
### 16.3.5 Contraseñas
Elegir una buena contraseña no es una tarea fácil, pero es fundamental que se haga correctamente o la seguridad de una cuenta (tal vez todo el sistema) podría verse comprometida. Elegir una buena contraseña es solo el comienzo; Debe tener mucho cuidado con su contraseña para que no se revele a otras personas. Nunca debe decirle a nadie su contraseña y nunca dejar que nadie le vea escribir su contraseña. Si decide anotar su contraseña, debe guardarla de forma segura en un lugar como una caja fuerte o una caja de seguridad.

¡Es fácil hacer una mala contraseña! Si utiliza cualquier información en su contraseña que esté relacionada con usted, esta información también puede ser conocida o descubierta por otros, lo que hace que su contraseña se vea comprometida fácilmente. Su contraseña nunca debe contener información sobre usted o cualquier persona que conozca, como su nombre, segundo nombre, apellido, fecha de nacimiento, teléfono, nombres de mascotas, licencia de conducir o número de seguro social.

Hay numerosos factores a tener en cuenta cuando se intenta elegir una contraseña para una cuenta:

- **Longitud**: El archivo /etc/login.defs permite al administrador especificar la longitud mínima de la contraseña. Si bien algunos creen que cuanto más larga sea la contraseña, mejor, esto no es realmente correcto. El problema con las contraseñas demasiado largas es que no se recuerdan fácilmente y, como resultado, a menudo se escriben en un lugar donde pueden encontrarse y verse comprometidas fácilmente.
- **Composición**: Una buena contraseña debe estar compuesta por una combinación de caracteres alfabéticos, numéricos y simbólicos.
- **Vida útil**: La cantidad máxima de tiempo que se puede usar una contraseña debe estar limitada por varias razones:

- Si una cuenta se ve comprometida y el tiempo que la contraseña es válida es limitado, el intruso finalmente perderá el acceso cuando la contraseña deje de ser válida.
- Si una cuenta no se está utilizando, se puede desactivar automáticamente cuando la contraseña ya no sea válida.
- Si los atacantes intentan un ataque de "fuerza bruta" probando todas las contraseñas posibles, entonces la contraseña se puede cambiar antes de que el ataque pueda tener éxito.

Sin embargo, exigir a un usuario que cambie su contraseña con demasiada frecuencia puede plantear problemas de seguridad, entre ellos:

- La calidad de la contraseña que elija el usuario puede verse afectada.
- El usuario puede comenzar a escribir su contraseña en papel, aumentando la posibilidad de que la contraseña pueda ser descubierta.
- Las cuentas de usuario que se usan con poca frecuencia pueden caducar y requerir atención administrativa para restablecerse.

Las opiniones varían sobre la frecuencia con la que se debe obligar a los usuarios a cambiar sus contraseñas. En el caso de las cuentas muy confidenciales, se recomienda cambiar las contraseñas con más frecuencia, por ejemplo, cada 30 días. Por otro lado, para las cuentas no críticas sin acceso a información confidencial, hay menos necesidad de cambios frecuentes. Para las cuentas con riesgo mínimo, tener una duración de 90 días se consideraría más razonable.
#### 16.3.5.1 Establecer una contraseña de usuario
Hay varias formas de cambiar una contraseña de usuario. El usuario puede ejecutar el comando passwd, el administrador puede ejecutar el comando passwd proporcionando el nombre de usuario como argumento, o también están disponibles herramientas gráficas.

El administrador puede usar el comando passwd para establecer la contraseña inicial o cambiar la contraseña de la cuenta. Por ejemplo, si el administrador ha creado la cuenta jane, la ejecución de passwd jane proporciona al administrador un mensaje para establecer la contraseña de la cuenta jane. Si se completa con éxito, el archivo /etc/shadow se actualizará con la nueva contraseña del usuario.

Mientras que los usuarios normales deben seguir muchas reglas de contraseña, el usuario root solo necesita seguir una regla: la contraseña no se puede dejar en blanco. Cuando el usuario root viola todas las demás reglas de contraseña que normalmente se aplican a los usuarios normales, se imprime una advertencia en la pantalla y la regla no se aplica:

```
root@localhost:~# passwd Jane
Enter new UNIX password:
BAD PASSWORD: it is WAY to short
BAD PASSWORD: is too simple
Retype new UNIX password:
```

Suponiendo que el administrador ha establecido una contraseña para una cuenta de usuario, el usuario puede iniciar sesión con ese nombre de cuenta y contraseña. Después de que el usuario abra un terminal, puede ejecutar el comando passwd sin argumentos para cambiar su propia contraseña. Se les solicita su contraseña actual y, a continuación, se les pide que introduzcan la nueva contraseña dos veces.

Como usuario común, puede ser difícil establecer una contraseña válida porque se deben seguir todas las reglas para la contraseña. Normalmente, al usuario se le permiten tres intentos para proporcionar una contraseña válida antes de que el comando passwd salga con un error.

Usando los privilegios del usuario root, las contraseñas cifradas y otra información relacionada con la contraseña se pueden ver viendo el archivo /etc/shadow. Recuerde que los usuarios normales no pueden ver el contenido de este archivo.
#### 16.3.5.2 Administrar la antigüedad de las contraseñas
El comando chage proporciona muchas opciones para administrar la información de antigüedad de la contraseña que se encuentra en el archivo /etc/shadow.

Este es un resumen de las opciones de pago:

| **Opción corta** | **Opción larga**         | **Descripción**                                                                                                               |
| ---------------- | ------------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| -l               | --lista                  | Enumerar la información de antigüedad de la cuenta                                                                            |
| -d _LAST_DAY_    | --último día _LAST_DAY_  | Establezca la fecha del último cambio de contraseña en LAST_DAY                                                               |
| -E _EXPIRE_DATE_ | --expirado _EXPIRE_DATE_ | Configurar la cuenta para que caduque el EXPIRE_DATE                                                                          |
| -h               | --Ayuda                  | Mostrar la ayuda para el comando chage                                                                                        |
| -I _INACTIVO_    | --inactivo _INACTIVO_    | Configurar la cuenta para permitir el inicio de sesión durante los días INACTIVOS después de que caduque la contraseña        |
| -m _MIN_DAYS_    | --mindays _MIN_DAYS_     | Establezca el número mínimo de días antes de que la contraseña se pueda cambiar a MIN_DAYS                                    |
| -M _MAX_DAYS_    | --maxdays _MAX_DAYS_     | Establezca el número máximo de días antes de que se cambie una contraseña a MAX_DAYS                                          |
| -W WARN_DAYS     | --warndays _WARN_DAYS_   | Establezca el número de días antes de que caduque una contraseña para que comience a mostrarse una advertencia para WARN_DAYS |

Un buen ejemplo del comando chage sería cambiar el número máximo de días que la contraseña de una persona es válida para que sea de 60 días:

```
root@localhost:~# chage -M 60 jane
root@localhost:~# grep jane /etc/shadow | cut -d: -f1,5
jane:60 
```
### 16.3.6 Modificación de un usuario
Antes de realizar cambios en una cuenta de usuario, tenga en cuenta que algunos comandos no modificarán correctamente una cuenta de usuario si el usuario ha iniciado sesión actualmente (como cambiar el nombre de inicio de sesión del usuario).

Otros cambios que pueda realizar no serán efectivos si el usuario ha iniciado sesión, pero entrarán en vigor tan pronto como el usuario cierre la sesión y vuelva a iniciarla. Por ejemplo, al modificar membresías de grupos, las nuevas membresías no estarán disponibles para el usuario hasta la próxima vez que inicie sesión.

En cualquier caso, es útil saber cómo usar los comandos who, w y last, para que pueda saber quién ha iniciado sesión en el sistema, ya que esto puede afectar los cambios que desea realizar en una cuenta de usuario.

Los comandos who y w muestran quién está conectado actualmente al sistema. El comando w es el más detallado de los dos, ya que muestra el tiempo de actividad del sistema y la información de carga, así como el proceso que está ejecutando cada usuario. El último comando se puede utilizar para determinar las sesiones de inicio de sesión actuales y anteriores, así como su fecha y hora específicas. Al proporcionar un nombre de usuario o un nombre tty (terminal) como argumento, el comando solo muestra los registros que coinciden con ese nombre.

El comando usermod ofrece muchas opciones para modificar una cuenta de usuario existente. Muchas de estas opciones también están disponibles con el comando useradd en el momento en que se crea la cuenta. El siguiente gráfico proporciona un resumen de las opciones de usermod:

| **Opción corta** | **Opción larga**           | **Descripción**                                                                                                        |
| ---------------- | -------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| -c               | _COMENTARIO_               | Establece el valor del campo GECOS o comentario en COMMENT.                                                            |
| -d _HOME_DIR_    | --casa _HOME_DIR_          | Establece HOME_DIR como un nuevo directorio principal para el usuario.                                                 |
| -e _EXPIRE_DATE_ | --expirado _EXPIRE_DATE_   | Establezca la fecha de vencimiento de la cuenta en EXPIRE_DATE.                                                        |
| -f _INACTIVO_    | --inactivo _INACTIVO_      | Configure la cuenta para permitir el inicio de sesión durante los días INACTIVOS después de que caduque la contraseña. |
| -g _GRUPO_       | --GRUPO gid                | Establezca GROUP como el grupo principal.                                                                              |
| -G _GRUPOS_      | --groups _GRUPOS_          | Establezca grupos suplementarios en una lista especificada en GROUPS.                                                  |
| -un              | --añadir                   | Anexe los grupos complementarios del usuario a los especificados por la opción -G.                                     |
| -h               | --Ayuda                    | Muestra la ayuda para el comando usermod.                                                                              |
| -l _NEW_LOGIN_   | --iniciar sesión NEW_LOGIN | Cambie el nombre de inicio de sesión del usuario.                                                                      |
| -L               | --cerradura                | Bloquear la cuenta de usuario.                                                                                         |
| -s _CÁSCARA_     | --shell _SHELL_            | Especifique el shell de inicio de sesión de la cuenta.                                                                 |
| -u _NEW_UID_     | --uid _NEW_UID_            | Especifique el UID del usuario que se NEW_UID.                                                                         |
| -U               | --abrir                    | Desbloquear la cuenta de usuario.                                                                                      |

Varias de estas opciones son dignas de discusión debido a cómo impactan en la gestión de usuarios. Puede ser muy problemático cambiar el UID del usuario con la opción -u, ya que los archivos que sean propiedad del usuario quedarán huérfanos. Por otro lado, especificar un nuevo nombre de inicio de sesión para el usuario con -l no hace que los archivos queden huérfanos.

La eliminación de un usuario con el comando userdel puede dejar huérfanos o eliminar los archivos del usuario en el sistema. En lugar de eliminar la cuenta, otra opción es bloquear la cuenta con la opción -L para el comando usermod. El bloqueo de una cuenta impide que se utilice, pero se mantiene la propiedad de los archivos.

Hay algunas cosas importantes que debe saber sobre la gestión de los grupos complementarios. Si utiliza la opción -G sin la opción -a, debe enumerar todos los grupos a los que pertenecería el usuario. El uso de la opción -G por sí sola puede provocar la eliminación accidental de un usuario de todos los grupos complementarios anteriores a los que pertenecía el usuario.

Si usas la opción -a con -G, entonces solo tienes que listar los nuevos grupos a los que pertenecería el usuario. Por ejemplo, si la usuaria jane pertenece actualmente a los grupos de ventas e investigación, para agregar su cuenta al grupo de desarrollo, ejecute el siguiente comando:

```
root@localhost:~# usermod -aG development jane
```
### 16.3.7 Eliminación de un usuario
El comando userdel se utiliza para eliminar usuarios. Al eliminar una cuenta de usuario, también debe decidir si desea eliminar el directorio principal del usuario. Los archivos del usuario pueden ser importantes para la organización, e incluso puede haber requisitos legales para conservar los datos durante un tiempo determinado, así que tenga cuidado de no tomar esta decisión a la ligera. Además, a menos que haya realizado copias de seguridad de los datos, una vez que haya ejecutado el comando para eliminar al usuario y sus archivos, no hay forma de revertir la acción.

Para eliminar el usuario jane sin eliminar el directorio principal del usuario /home/jane, ejecute:

```
root@localhost:~# userdel jane
```

Tenga en cuenta que eliminar un usuario sin eliminar su directorio principal significa que los archivos del directorio principal del usuario quedarán huérfanos y estos archivos serán propiedad únicamente de su UID y GID anteriores.

Para eliminar también el usuario, el directorio de inicio y la cola de correo, use la opción -r:

```
root@localhost:~# userdel -r jane
```

> El comando anterior solo eliminará los archivos del usuario en su directorio de inicio y su cola de correo. Si el usuario posee otros archivos fuera de su directorio principal, esos archivos seguirán existiendo como archivos huérfanos.