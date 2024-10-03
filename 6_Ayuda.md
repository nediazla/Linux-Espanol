# 6. Ayuda
## 6.1 Introducción
Hay miles de comandos disponibles con muchas opciones, lo que convierte a la línea de comandos en una herramienta poderosa. Sin embargo, con este poder viene la complejidad. La complejidad, a su vez, puede crear confusión. Como resultado, saber cómo encontrar ayuda mientras se trabaja en Linux es una habilidad esencial para cualquier usuario. Consultar la ayuda proporciona un recordatorio rápido de cómo funciona un comando, además de ser un recurso de información al aprender nuevos comandos.
## 6.2 Páginas de manual
UNIX es el sistema operativo en el que se basó Linux. Los desarrolladores de UNIX crearon documentos de ayuda llamados _páginas de manual_ (abreviatura  de _páginas de manual_).

Las páginas del manual se utilizan para describir las características de los comandos. Proporcionan una descripción básica del propósito del comando, así como detalles sobre las opciones disponibles.
### 6.2.1 Visualización de páginas de manual
Para ver una página de comando man para un comando, use el comando man:

```
man command
```

Por ejemplo, a continuación se muestra la página del comando man para el comando ls:

```
sysadmin@localhost:~$ man ls
```

Navegue por el documento con las teclas de flecha:

```
LS(1)                            User Commands                           LS(1)  
                                                                                
NAME                                                                            
       ls - list directory contents                                             
                                                                                
SYNOPSIS                                                                        
       ls [OPTION]... [FILE]...                                                 
                                                                                
DESCRIPTION                                                                     
       List  information  about  the FILEs (the current directory by default).  
       Sort entries alphabetically if none of -cftuvSUX nor --sort  is  speci-  
       fied.                                                                    
                                                                                
       Mandatory  arguments  to  long  options are mandatory for short options  
       too.                                                                     
                                                                                
       -a, --all                                                                
              do not ignore entries starting with .  
```

Para salir de la visualización de una página del comando man, utilice la tecla **Q**.

> El comando man utiliza un _localizador_ para mostrar documentos. Por lo general, este paginador es el comando less, pero en algunas distribuciones, puede ser el comando moren. Ambos son muy similares en su rendimiento.
> Para ver los distintos comandos de movimiento disponibles, utilice la tecla **H** mientras visualiza una página del comando man. Aparecerá una página de ayuda.
> Los buscapersonas se discutirán con más detalle más adelante en el curso.
### 6.2.2 Secciones dentro de las páginas de manual
Las páginas del manual se dividen en secciones. Cada sección está diseñada para proporcionar información específica sobre un comando. Si bien hay secciones comunes que se ven en la mayoría de las páginas de manual, algunos desarrolladores también crean secciones que solo están disponibles en páginas de manual específicas.

A continuación se describen algunas de las secciones más comunes que se encuentran en las páginas de manual:

**NOMBRE**

Proporciona el nombre del comando y una descripción muy breve.

```
NAME                                                                            
       ls - list directory contents   
```

**SINOPSIS**

Proporciona ejemplos de cómo se ejecuta el comando.

```
SYNOPSIS                                                                        
       ls [OPTION]... [FILE]...   
```

La sección SINOPSIS de una página del manual puede ser difícil de entender, pero es muy importante porque proporciona un ejemplo conciso de cómo usar el comando. Por ejemplo, considere la SINOPSIS de la página del manual para el comando cal:

```
SYNOPSIS 
     cal [-31jy] [-A number] [-B number] [-d yyyy-mm] [[month] year] 
```

Los corchetes [ ] se utilizan para indicar que esta función no es necesaria para ejecutar el comando. Por ejemplo, [-31jy] significa que las opciones -3, -1, -j o -y están disponibles, pero no son necesarias para que el comando cal funcione correctamente.

El último conjunto de corchetes [[mes] año] muestra otra característica; Significa que un año se puede especificar por sí mismo, pero para especificar un mes también se debe especificar el año.

Otro componente de la SINOPSIS que puede causar cierta confusión se puede ver en la página del manual del comando date:

```
SYNOPSIS                                                              
       date [OPTION]... [+FORMAT]                                      
       date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]
```

En esta SINOPSIS hay dos sintaxis para el comando date. El primero se utiliza para mostrar la fecha en el sistema, mientras que el segundo se utiliza para establecer la fecha.

Los puntos suspensivos ... después de [OPTION] indica que se pueden utilizar uno o más de los elementos anteriores.

Además, la notación [-u|--utc|--universal] significa que se puede usar la opción -u o la opción --utc o la opción --universal. Por lo general, esto significa que las tres opciones realmente hacen lo mismo, pero a veces el uso de la variable | se usa para indicar que las opciones no se pueden usar en combinación, como un "o" lógico.

**DESCRIPCIÓN**

Proporciona una descripción más detallada del comando.

```
DESCRIPTION  
       List  information  about  the FILEs (the current directory by default).  
       Sort entries alphabetically if none of -cftuvSUX nor --sort  is  speci-  
       fied.   
```

**OPCIONES**

Enumera las opciones del comando, así como una descripción de cómo se utilizan. A menudo, esta información se encuentra en la sección DESCRIPCIÓN y no en una sección separada de OPCIONES.

```
       -a, --all                                                                
              do not ignore entries starting with .  
 
       -A, --almost-all                                                         
              do not list implied . and ..                                      
                                                                                
       --author                                                                 
              with -l, print the author of each file                            
                                                                                
       -b, --escape                                                             
              print C-style escapes for nongraphic characters                   
                                                                                
       --block-size=SIZE                                                        
              scale sizes by SIZE before printing them; e.g., '--block-size=M'  
              prints sizes in units of 1,048,576 bytes; see SIZE format below  
```

**ARCHIVOS**

Enumera los archivos asociados al comando, así como una descripción de cómo se utilizan. Estos archivos se pueden utilizar para configurar las funciones más avanzadas del comando. A menudo, esta información se encuentra en la sección DESCRIPCIÓN y no en una sección separada de ARCHIVOS.

**AUTOR**

Proporciona el nombre de la persona que creó la página del comando man y (a veces) cómo ponerse en contacto con la persona.

```
AUTHOR                                                                          
       Written by Richard M. Stallman and David MacKenzie.
```

**INFORMAR DE ERRORES**

Proporciona detalles sobre cómo notificar problemas con el comando.

```
REPORTING BUGS                                                                  
       GNU coreutils online help: <http://www.gnu.org/software/coreutils/>      
       Report ls translation bugs to <http://translationproject.org/team/>
```

**DERECHOS DE AUTOR**

Proporciona información básica sobre derechos de autor.

```
COPYRIGHT                                                                       
       Copyright (C) 2017 Free Software Foundation, Inc.  License GPLv3+:  GNU  
       GPL version 3 or later <http://gnu.org/licenses/gpl.html>.               
       This  is  free  software:  you  are free to change and redistribute it.  
       There is NO WARRANTY, to the extent permitted by law. 
```

**VÉASE TAMBIÉN**

Le proporciona una idea de dónde puede encontrar información adicional. Esto a menudo incluye otros comandos que están relacionados con este comando.

```
SEE ALSO                                                                        
       Full documentation at: <http://www.gnu.org/software/coreutils/ls>        
       or available locally via: info '(coreutils) ls invocation' 
```
### 6.2.3 Búsqueda de páginas de manual
Para buscar un término en una página de manual, escriba el carácter / seguido de un término de búsqueda y, a continuación, pulse la tecla **Intro**. El programa busca desde la ubicación actual hacia la parte inferior de la página para tratar de localizar y resaltar el término.

Si se encuentra una coincidencia, se resaltará. Para pasar a la siguiente coincidencia del término, presione **n**. Para volver a una coincidencia anterior del término, presione **Mayús+N**. Si no se encuentra el término, o al llegar al final de los partidos, el programa informará de Patrón no encontrado (presione Retorno).

```
MAN(1)                        Manual pager utils                        MAN(1)  
                                                                                
NAME                                                                            
       man - an interface to the on-line reference manuals                      
                                                                                
SYNOPSIS                                                                        
       man  [-C  file]  [-d]  [-D]  [--warnings[=warnings]]  [-R encoding] [-L  
       locale] [-m system[,...]] [-M path] [-S list]  [-e  extension]  [-i|-I]  
       [--regex|--wildcard]   [--names-only]  [-a]  [-u]  [--no-subpages]  [-P  
       pager] [-r prompt] [-7] [-E encoding] [--no-hyphenation] [--no-justifi-  
       cation]  [-p  string]  [-t]  [-T[device]]  [-H[browser]] [-X[dpi]] [-Z]  
       [[section] page[.section] ...] ...                                       
       man -k [apropos options] regexp ...                                      
       man -K [-w|-W] [-S list] [-i|-I] [--regex] [section] term ...            
       man -f [whatis options] page ...                                         
       man -l [-C file] [-d] [-D] [--warnings[=warnings]]  [-R  encoding]  [-L  
       locale]  [-P  pager]  [-r  prompt]  [-7] [-E encoding] [-p string] [-t]  
       [-T[device]] [-H[browser]] [-X[dpi]] [-Z] file ...                       
       man -w|-W [-C file] [-d] [-D] page ...                                   
       man -c [-C file] [-d] [-D] page ...                                      
       man [-?V]                                                                
                                                                                
DESCRIPTION                                                                     

```
### 6.2.4 Páginas de manual categorizadas por secciones
Hasta ahora, hemos estado mostrando páginas de manual para comandos. Sin embargo, hay varios tipos diferentes de comandos (comandos de usuario, comandos del sistema y comandos de administración), archivos de configuración y otras características, como bibliotecas y componentes del kernel, que requieren documentación.

Como resultado, hay miles de páginas de manual en una distribución típica de Linux. Para organizar todas estas páginas del manual, se clasifican por secciones.

De forma predeterminada, hay nueve secciones de páginas de manual:

1. Comandos Generales
2. Llamadas al sistema
3. Convocatorias a la biblioteca
4. Archivos especiales
5. Formatos de archivo y convenciones
6. Juegos
7. Misceláneo
8. Comandos de administración del sistema
9. Rutinas del kernel

El comando man busca en cada una de estas secciones en orden hasta que encuentra la primera coincidencia. Por ejemplo, si ejecuta el comando man cal, en la primera sección (Comandos generales) se busca una página de manual llamada cal. Si no se encuentra, se busca en la segunda sección. Si no se encuentra ninguna página de manual después de buscar en todas las secciones, se devuelve un mensaje de error:

```
sysadmin@localhost:~$ man zed                                          
No manual entry for zed      
```

Para determinar a qué sección pertenece una página de manual específica, observe el valor numérico de la primera línea de la salida de la página de manual. Por ejemplo, el comando cal pertenece a la primera sección de las páginas del manual:

```
CAL(1)                    BSD General Commands Manual             CAL(1)
```

A veces hay páginas de manual con el mismo nombre en diferentes secciones. En estos casos, puede ser necesario especificar la sección de la página de manual correcta.

Por ejemplo, hay un comando llamado passwd que le permite cambiar su contraseña. También hay un archivo llamado passwd que almacena la información de la cuenta. Tanto el comando como el archivo tienen una página de manual.

El comando passwd es un comando de usuario, por lo que la página del manual asociada se encuentra en la primera sección. El comando man muestra la página man para el comando passwd de forma predeterminada:

```
sysadmin@localhost:~$ man passwd
```

```
PASSWD(1)                        User Commands                 PASSWD(1)
```

Entonces, ¿cómo se muestra la página de manual para el archivo passwd? En primer lugar, determine en qué sección se encuentra la página del manual. Para buscar páginas de manual por nombre, utilice la opción -f del comando man. Muestra las páginas del manual que coinciden, o coinciden parcialmente, con un nombre específico y proporciona el número de sección y una breve descripción de cada página del manual del manual:

```
sysadmin@localhost:~$ man -f passwd                                    
passwd (5)           - the password file                              
passwd (1)           - change user password                           
passwd (1ssl)        - compute password hashes    
```

> En la mayoría de las distribuciones de Linux, el comando whatis hace lo mismo que man -f. En esas distribuciones, ambos producirán la misma salida.

Para especificar una sección diferente, proporcione el número de la sección como primer argumento del comando man. El siguiente comando muestra la página del comando man passwd ubicada en la sección 5, que está asociada con el archivo passwd:

```
sysadmin@localhost:~$ man 5 passwd
```

```
PASSWD(5)                File Formats and Conversions          PASSWD(5)
```

Desafortunadamente, no siempre recordará el nombre exacto de la página del comando man que desea ver. Afortunadamente, cada página del manual tiene una breve descripción asociada. La opción -k del comando man busca una palabra clave tanto en los nombres como en las descripciones de las páginas del manual.

Por ejemplo, para encontrar una página de manual que muestre cómo copiar directorios, busque la palabra clave copy:

```
sysadmin@localhost:~$ man -k copy                                               
cp (1)               - copy files and directories                               
cpgr (8)             - copy with locking the given file to the password or gr...
cpio (1)             - copy files to and from archives                          
cppw (8)             - copy with locking the given file to the password or gr...
dd (1)               - convert and copy a file                                  
debconf-copydb (1)   - copy a debconf database                                  
install (1)          - copy files and set attributes                            
scp (1)              - secure copy (remote file copy program)                   
ssh-copy-id (1)      - use locally available keys to authorize logins on a re...
```

Recuerde que hay miles de páginas de manual, por lo que cuando busque una palabra clave, sea lo más específico posible. El uso de una palabra genérica, como "el", podría dar lugar a cientos o incluso miles de resultados.

> En la mayoría de las distribuciones de Linux, el comando apropos hace lo mismo que man -k. En esas distribuciones, ambos producen la misma salida.
## 6.3 Búsqueda de comandos y documentación
El comando whatis (o man -f) devuelve la sección en la que se almacena una página de manual. En ocasiones, este comando devuelve resultados inusuales, como los siguientes:

```
sysadmin@localhost:~$ whatis ls                                              
ls (1)               - list directory contents 
ls (lp)              - list directory contents
```

> El ejemplo anterior está diseñado para demostrar un escenario en el que dos comandos enumeran el contenido del directorio. Es posible que la salida del terminal de ejemplo anterior no coincida con la salida de la máquina virtual.

En función de este resultado, hay dos comandos ls que enumeran el contenido del directorio. La sencilla razón de esto es que UNIX tenía dos variantes principales, lo que dio lugar a que algunos comandos se desarrollaran "en paralelo" y, por lo tanto, se comportaran de manera diferente en diferentes variantes de UNIX. Muchas distribuciones modernas de Linux incluyen comandos de ambas variantes de UNIX.

Sin embargo, esto plantea un pequeño problema: cuando se escribe el comando ls, ¿qué comando se ejecuta?
### 6.3.1 ¿Dónde se encuentran estos comandos?
Para buscar la ubicación de un comando o las páginas del comando man de un comando, utilice el comando whereis. Este comando busca comandos, archivos de código fuente y páginas de manual en ubicaciones específicas donde normalmente se almacenan estos archivos:

```
sysadmin@localhost:~$ whereis ls 
ls: /bin/ls /usr/share/man/man1p/ls.1.gz /usr/share/man/man1/ls.1.gz
```

Las páginas de manual se distinguen fácilmente de los comandos, ya que normalmente se comprimen con un programa llamado gzip, lo que da como resultado un nombre de archivo que termina en .gz. Observe que hay dos páginas de manual enumeradas anteriormente, pero solo un comando: /bin/ls. Esto se debe a que el comando ls se puede utilizar con las opciones/características descritas por cualquiera de las páginas del manual. Por lo tanto, al aprender lo que se puede hacer con el comando ls, puede ser interesante explorar ambas páginas de manual. Afortunadamente, esto es más bien una excepción, ya que la mayoría de los comandos solo tienen una página de manual.
### 6.3.2 Buscar cualquier archivo o directorio
El comando whereis está diseñado específicamente para buscar comandos y páginas de comando man. Si bien esto es útil, a menudo es necesario encontrar un archivo o directorio, no solo archivos que sean comandos o páginas de manual.

Para buscar cualquier archivo o directorio, utilice el comando localice. Este comando busca en una base de datos todos los archivos y directorios que estaban en el sistema cuando se creó la base de datos. Normalmente, el comando para generar esta base de datos se ejecuta todas las noches.

```
sysadmin@localhost:~$ locate gshadow                                   
/etc/gshadow                                                           
/etc/gshadow-                                                          
/usr/include/gshadow.h                                                
/usr/share/man/cs/man5/gshadow.5.gz                                   
/usr/share/man/da/man5/gshadow.5.gz                                    
/usr/share/man/de/man5/gshadow.5.gz                                    
/usr/share/man/fr/man5/gshadow.5.gz                                    
/usr/share/man/it/man5/gshadow.5.gz                                    
/usr/share/man/man5/gshadow.5.gz                                       
/usr/share/man/ru/man5/gshadow.5.gz                                   
/usr/share/man/sv/man5/gshadow.5.gz                                    
/usr/share/man/zh_CN/man5/gshadow.5.gz       
```

Los archivos creados hoy no se podrán buscar con el comando localizar. Si el acceso root está disponible, es posible actualizar la base de datos de localización manualmente ejecutando el comando updatedb. Los usuarios normales no pueden actualizar el archivo de base de datos.

También tenga en cuenta que cuando se utiliza el comando locate como usuario normal, la salida puede estar limitada debido a los permisos de archivo. Si el usuario que ha iniciado sesión no tiene acceso a un archivo o directorio en el sistema de archivos debido a los permisos, el comando locate no devolverá esos nombres. Esta característica de seguridad está diseñada para evitar que los usuarios "exploren" el sistema de archivos mediante el uso de la base de datos de localización. El usuario root puede buscar cualquier archivo en la base de datos de localización.

La salida del comando locate puede ser bastante grande. Al buscar un nombre de archivo, como passwd, el comando locate genera todos los archivos que contienen la cadena passwd, no solo los archivos denominados passwd.

En muchos casos, es útil empezar por averiguar cuántos archivos coinciden. Para ello, utilice la opción -c del comando localice:

```
sysadmin@localhost:~$ locate -c passwd                                 
98
```

Para limitar la salida producida por el comando localice, utilice la opción -b. Esta opción solo incluye listados que tienen el término de búsqueda en el nombre base del nombre del archivo. El _nombre_ base es la parte del nombre de archivo que no incluye los nombres de directorio.

```
sysadmin@localhost:~$ locate -c -b passwd                              
83 
```

Como puede ver en la salida anterior, todavía habrá muchos resultados cuando se use la opción -b. Para limitar aún más la salida, coloque un carácter \ delante del término de búsqueda. Este carácter limita la salida a nombres de archivo que coincidan exactamente con el término:

```
sysadmin@localhost:~$ locate -b "\passwd"                              
/etc/passwd                                                                     
/etc/pam.d/passwd                                                               
/usr/bin/passwd                                                                 
/usr/share/doc/passwd                                                           
/usr/share/lintian/overrides/passwd
```

## 6.4 Documentación de información
Las páginas de manual son excelentes fuentes de información, pero tienden a tener algunas desventajas. Un ejemplo es que cada página de manual es un documento independiente, no relacionado con ninguna otra página de manual. Si bien algunas páginas de manual tienen una sección VÉASE TAMBIÉN que puede hacer referencia a otras páginas de manual, tienden a ser fuentes independientes de documentación.

El comando info también proporciona documentación sobre los comandos y las funciones del sistema operativo. El objetivo es ligeramente diferente al de las páginas de manual: proporcionar un recurso de documentación que proporcione una estructura organizativa lógica, facilitando la lectura de la documentación.

Toda la documentación se fusiona en un solo "libro" que representa toda la documentación disponible. Dentro de los documentos de información, la información se divide en categorías que funcionan de manera muy similar a una tabla de contenido en un libro. Los hipervínculos se proporcionan a páginas con información sobre temas individuales para un comando o característica específica.

Otra ventaja de la información sobre las páginas de manual es que el estilo de escritura de los documentos de información suele ser más propicio para el aprendizaje de un tema. Considere que las páginas de manual son más un recurso de referencia y los documentos de información son más una guía de aprendizaje.
### 6.4.1 Visualización de la documentación de información
Para mostrar la documentación info de un comando, utilice el comando info.

```
info command
```

Por ejemplo, para mostrar la página de información del comando ls:

```
sysadmin@localhost:~$ info ls     
```

Navegue por el documento con las teclas de flecha:

```
Next: dir invocation,  Up: Directory listing

10.1 `ls': List directory contents                                     
==================================  

    The `ls' program lists information about files (of any type, including directories).  Options and file arguments can be intermixed arbitrarily, as usual. 

    For non-option command-line arguments that are directories, by    
default `ls' lists the contents of directories, not recursively, and   
omitting files with names beginning with `.'.  For other non-option    
arguments, by default `ls' lists just the file name.  If no non-option 
argument is specified, `ls' operates on the current directory, acting  
as if it had been invoked with a single argument of `.'.                        
                                                                       
    By default, the output is sorted alphabetically, according to the  
locale settings in effect.(1) If standard output is a terminal, the    
output is in columns (sorted vertically) and control characters are    
output as question marks; otherwise, the output is listed one per line 
and control characters are output as-is.

-----Info: (coreutils)ls invocation, 57 lines --Top-----------------------------
Welcome to Info version 6.5.  Type H for help, h for tutorial. 
```

Esta documentación se divide en nodos y, en el ejemplo anterior, la línea resaltada en blanco muestra que actualmente se encuentra en el nodo de invocación ls. La primera línea proporciona información de índice sobre la ubicación actual dentro del documento. El siguiente nodo, como el siguiente capítulo de un libro, sería el nodo de invocación de directorios. Subiendo un nivel se encuentra el nodo de listado de directorios.

Al desplazarse por el documento, observe el menú del comando ls:

```
* Menu:                                                                         
                           
* Which files are listed::                     
* What information is listed::                 
* Sorting the output::                          
* Details about version sort::                   
* General output formatting::                    
* Formatting file timestamps::                    
* Formatting the file names::                                                   
                                         
   ---------- Footnotes ----------                                              
          
 (1) If you use a non-POSIX locale (e.g., by setting `LC_ALL' to 
`en_US'), then `ls' may produce output that is sorted differently than
you're accustomed to.  In that case, set the `LC_ALL' environment     
variable to `C'.                     
                                           
-----Info: (coreutils)ls invocation, 57 lines --Bot-----------------------------
```

Los elementos del menú son hipervínculos que enlazan con nodos que describen más sobre el comando ls. Por ejemplo, si se coloca el cursor en la línea * Ordenando la salida:: y pulsando la tecla **Enter**, se accede a un nodo que describe la ordenación de la salida del comando ls:

```
Next: Details about version sort,  Prev: What information is listed,  Up: ls in\
vocation                                                                        
                                                                                
10.1.3 Sorting the output                                                       
-------------------------                                                       
                                                                                
These options change the order in which 'ls' sorts the information it           
outputs.  By default, sorting is done by character code (e.g., ASCII            
order).                                                                         
                                                                                
'-c'                                                                            
'--time=ctime'                                                                  
'--time=status'                                                                 
     If the long listing format (e.g., '-l', '-o') is being used, print         
     the status change timestamp (the ctime) instead of the mtime.  When        
     explicitly sorting by time ('--sort=time' or '-t') or when not             
     using a long listing format, sort according to the ctime.  *Note           
     File timestamps::.                                                         
                                                                                
'-f'                                                                            
     Primarily, like '-U'--do not sort; list the files in whatever order        
     they are stored in the directory.  But also enable '-a' (list all          
-----Info: (coreutils)Sorting the output, 69 lines --Top------------------------
```

Tenga en cuenta que al entrar en el nodo sobre la clasificación se accede a un subnodo del original. Para volver al nodo anterior, utilice la tecla **U**. Mientras que **U** conduce al inicio del nodo un nivel más arriba, la tecla **L** regresa a la misma ubicación que antes de ingresar al nodo de clasificación.
### 6.4.2 Navegación por documentos info
Al igual que el comando man, una lista de comandos de movimiento está disponible presionando la tecla **Shift + H** mientras lee la documentación de información:

```
Basic Info command keys                                                         
                                                                                
l           Close this help window.                                             
q           Quit Info altogether.                                               
h           Invoke the Info tutorial.                                           
                                                                                
Up          Move up one line.                                                   
Down        Move down one line.                                                 
PgUp        Scroll backward one screenful.                                      
PgDn        Scroll forward one screenful.                                       
Home        Go to the beginning of this node.                                   
End         Go to the end of this node.                                         
                                                                                
TAB         Skip to the next hypertext link.                                    
RET         Follow the hypertext link under the cursor.                         
l           Go back to the last node seen in this window.                       
                                                                                
[           Go to the previous node in the document.                            
]           Go to the next node in the document.                                
p           Go to the previous node on this level.                              
n           Go to the next node on this level.                                  
u           Go up one level.                                                    
-----Info: *Info Help*, 302 lines -- Top--------------------------------------
```

Tenga en cuenta que para cerrar la pantalla de ayuda, escriba la tecla **L**, que devuelve el documento actual. Para salir por completo, use la tecla **Q**.
### 6.4.3 Exploración de la documentación de información
En lugar de usar la documentación de información para buscar información sobre un comando o característica específica, considere explorar las capacidades de Linux leyendo la documentación de información. Ejecute el comando info sin ningún argumento para llevarlo al nivel superior de la documentación. Este es un buen punto de partida para explorar muchas de las características que se ofrecen:

```
File: dir,      Node: Top,      This is the top of the INFO tree.               
                                                                                
This is the Info main menu (aka directory node).                                
A few useful Info commands:                                                     
                                                                                
  'q' quits;                                                                    
  'H' lists all Info commands;                                                  
  'h' starts the Info tutorial;                                                 
  'mTexinfo RET' visits the Texinfo manual, etc.                                
                                                                                
* Menu:                                                                         
                                                                                
Basics                                                                          
* Common options: (coreutils)Common options.                                    
* Coreutils: (coreutils).       Core GNU (file, text, shell) utilities.         
* Date input formats: (coreutils)Date input formats.                            
* File permissions: (coreutils)File permissions.                                
                                Access modes.                                   
* Finding files: (find).        Operating on files matching certain criteria.   
                                                                                
C++ libraries                                                                   
* autosprintf: (autosprintf).   Support for printf format strings in C++.       
-----Info: (dir)Top, 211 lines --Top--------------------------------------------
Welcome to Info version 6.5.  Type H for help, h for tutorial. 
```
## 6.5 Fuentes adicionales de ayuda
En muchos casos, las páginas de manual o la documentación de información proporcionan la información necesaria de manera eficiente. Sin embargo, hay otras fuentes de ayuda a tener en cuenta.
### 6.5.1 Uso de la opción de ayuda
Muchos comandos proporcionarán información básica, muy similar a la SINOPSIS que se encuentra en las páginas de manual, simplemente usando la opción --help al comando. Esta opción es útil para aprender rápidamente el uso básico de un comando sin salir de la línea de comandos:

```
sysadmin@localhost:~$  cat --help                                                
Usage: cat [OPTION]... [FILE]...                                                
Concatenate FILE(s) to standard output.                                         
                                                                                
With no FILE, or when FILE is -, read standard input.                           
                                                                                
  -A, --show-all           equivalent to -vET                                   
  -b, --number-nonblank    number nonempty output lines, overrides -n           
  -e                       equivalent to -vE                                    
  -E, --show-ends          display $ at end of each line                        
  -n, --number             number all output lines                              
  -s, --squeeze-blank      suppress repeated empty output lines                 
  -t                       equivalent to -vT                                    
  -T, --show-tabs          display TAB characters as ^I                         
  -u                       (ignored)                                            
  -v, --show-nonprinting   use ^ and M- notation, except for LFD and TAB        
      --help     display this help and exit                                     
      --version  output version information and exit                            
                                                                                
Examples:                                                                       
  cat f - g  Output f's contents, then standard input, then g's contents.       
  cat        Copy standard input to standard output.                            
                                                                                
GNU coreutils online help: <http://www.gnu.org/software/coreutils/>             
Report cat translation bugs to <http://translationproject.org/team/>            
Full documentation at: <http://www.gnu.org/software/coreutils/cat>              
or available locally via: info '(coreutils) cat invocation'  
```
### 6.5.2 Documentación adicional del sistema
En la mayoría de los sistemas, hay un directorio donde se encuentra documentación adicional (como archivos de documentación almacenados por proveedores de software de terceros).

Estos archivos de documentación a menudo se denominan _archivos Léame_, ya que los archivos suelen tener nombres como README o readme.txt. La ubicación de estos archivos puede variar en función de la distribución que esté utilizando. Las ubicaciones típicas incluyen /usr/share/doc y /usr/doc.

Por lo general, este directorio es donde los administradores de sistemas acuden para aprender a configurar servicios de software más complejos. Sin embargo, a veces los usuarios habituales también encuentran útil esta documentación.