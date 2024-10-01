# 5.1 Laboratorio Capitulo 5
## 5.1 Introducción
Este es el laboratorio 5: Habilidades de línea de comandos. Al realizar este laboratorio, los estudiantes aprenderán a utilizar las características básicas del shell.

En este laboratorio, realizará las siguientes tareas:
- Explorar las características de Bash
- Usar variables del shell
- Ser capaz de utilizar comillas
## 5.2 Archivos y directorios
En esta tarea, accederemos a la interfaz de línea de comandos (CLI) para Linux para explorar cómo ejecutar comandos básicos y qué afecta a la forma en que se pueden ejecutar.

La mayoría de los usuarios probablemente estén más familiarizados con la forma en que se ejecutan los comandos mediante una interfaz gráfica de usuario (GUI). Por lo tanto, es probable que esta tarea le presente algunos conceptos nuevos si no ha trabajado anteriormente con una CLI. Para usar una CLI, deberá escribir el comando que desea ejecutar.

La ventana en la que escribirá el comando se conoce como aplicación emuladora de terminal. Dentro de la ventana de la Terminal, el sistema muestra un mensaje, que actualmente contiene un mensaje seguido de un cursor parpadeante:

```
**sysadmin@localhost:~$**
```

```
Es posible que tenga que pulsar Intro en la ventana para mostrar el mensaje.
```

El mensaje le indica que es administrador del sistema del usuario; el host o la computadora que está utilizando: localhost; y el directorio en el que se encuentra: ~, que representa su directorio de inicio.

Cuando escriba un comando, aparecerá en el cursor de texto. Puede utilizar teclas como las **teclas** de *home*, *end*, *Backspace* y las flechas para editar el comando que está escribiendo.

Igualmente importante es la sintaxis de la línea de comandos, que es el orden en el que el comando, las opciones y los argumentos deben introducirse en el símbolo del sistema para que el shell reconozca cómo ejecutar correctamente el comando. La sintaxis correcta de la línea de comandos es similar a la siguiente:

```
command [options] [arguments]
```

Escriba el comando ls en el símbolo del sistema, que enumerará los archivos y directorios contenidos en su directorio de trabajo actual. En este ejemplo, no se utilizarán opciones ni argumentos.

Una vez que haya escrito el comando correctamente, presione **Enter** para ejecutarlo.

```
sysadmin@localhost:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```
### 5.2.1 Paso 1
El comando ls se utiliza para enumerar información sobre directorios y archivos y, de forma predeterminada, muestra información para el directorio actual. Utilice la opción -l para mostrar esta información en el formato largo, que proporciona información adicional sobre los archivos ubicados en el directorio de trabajo actual:

```
ls -l
```

El resultado debe ser similar al siguiente:

```
sysadmin@localhost:~$ ls -l                                                     
total 32                                                                        
drwxr-xr-x 2 sysadmin sysadmin 4096 Oct 31 19:52 Desktop                        
drwxr-xr-x 4 sysadmin sysadmin 4096 Oct 31 19:52 Documents                      
drwxr-xr-x 2 sysadmin sysadmin 4096 Oct 31 19:52 Downloads                      
drwxr-xr-x 2 sysadmin sysadmin 4096 Oct 31 19:52 Music                          
drwxr-xr-x 2 sysadmin sysadmin 4096 Oct 31 19:52 Pictures                       
drwxr-xr-x 2 sysadmin sysadmin 4096 Oct 31 19:52 Public                         
drwxr-xr-x 2 sysadmin sysadmin 4096 Oct 31 19:52 Templates                      
drwxr-xr-x 2 sysadmin sysadmin 4096 Oct 31 19:52 Videos
```

Tenga en cuenta que los directorios se consideran un tipo de archivo en el sistema de archivos de Linux.
### 5.2.2 Paso 2

También se pueden agregar argumentos a los comandos. Al agregar la ubicación de un directorio específico al comando ls, se mostrará la información de ese directorio. Utilice el argumento /home para mostrar información detallada sobre los archivos en el directorio /home.

```
ls -l /home
```

El resultado debe ser similar al siguiente:

```
sysadmin@localhost:~$ ls -l /home                                                
total 4                                                                         
drwxr-xr-x 1 sysadmin sysadmin 4096 Aug  8 17:46 sysadmin
```

Al usar la opción -l y el argumento /home, ahora podemos ver que el directorio /home tiene un directorio llamado sysadmin ubicado dentro de él.
### 5.2.3 Paso 3
El siguiente comando mostrará la misma información que ve en la primera parte del mensaje. Asegúrese de haber seleccionado (hecho clic en) primero la  ventana **Terminal** y luego escriba el siguiente comando seguido de la tecla **Enter**:

```
whoami
```

El resultado debe ser similar al siguiente:

```
sysadmin@localhost:~$ whoami                            
sysadmin                                                
```

La salida del comando whoami, sysadmin, muestra el nombre de usuario del usuario actual. Aunque en este caso su nombre de usuario se muestra en el símbolo del sistema, este comando podría usarse para obtener esta información en una situación en la que el mensaje no contenía esta información.
### 5.2.4 Paso 4
El siguiente comando muestra información sobre el sistema actual. Para poder ver el nombre del kernel que está utilizando, escriba el siguiente comando en la terminal:

```
uname
```

El resultado debe ser similar al siguiente:

```
sysadmin@localhost:~$ uname                             
Linux
```

Muchos comandos que se ejecutan producen una salida de texto como esta. Puede cambiar la salida generada por un comando mediante  opciones después del nombre del comando.

Las opciones de un comando se pueden especificar de varias maneras. Tradicionalmente en UNIX, las opciones se expresaban mediante un guión seguido de otro carácter; Por ejemplo: -n.

En Linux, las opciones a veces también pueden ser dadas por dos caracteres de guión seguidos de una palabra, o palabra con guión; Por ejemplo: --nodename.

Vuelva a ejecutar el comando uname dos veces en el terminal, una vez con la opción -n y otra vez con la opción --nodename. Esto mostrará el nombre de host del nodo de red, que también se encuentra en el símbolo del sistema.

```
uname -n
uname --nodename
```

El resultado debe ser similar al siguiente:

```
sysadmin@localhost:~$ uname -n                          
localhost                                               
sysadmin@localhost:~$ uname --nodename                  
localhost
```

