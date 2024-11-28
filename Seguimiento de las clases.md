***Control del flujo stderr-stdout, operadores y procesos en segundo plano***
	Se escribe por consola *echo $?* para mostrar el código de estado del ultimo comando que se ha ejecutado.
	<span style="color:rgb(255, 192, 0)">stder</span> Errores en los archivos (Se escribe por consola el numero 2 para identificarlos). 
	<span style="color:rgb(255, 192, 0)">stdout</span> Output de los comandos que se muestra por consola (Se identifica con un numero 1).
	<span style="color:rgb(255, 192, 0)">(>)</span> redirección de salidas de comandos (comando + > al lugar donde quieras redirigir tanto el output como lo errores del programa)
	<span style="color:rgb(255, 192, 0)">2>&1</span> manera de convertir los errores *(2)* como si fueran output *(1)* de comandos. Se escribe de la siguiente manera:
	![[Pasted image 20230612200045.png]]
	<span style="color:rgb(255, 192, 0)">&></span> se refiere a todo el output de los comandos tanto errores (stderr) como el output (stdout) se envían a donde se indique:
**Correr aplicaciones en segundo plano**
		Esto pasa porque los procesos son hijos de una consola, por lo que seguirá estando atado el proceso a la consola. Se hace de la siguiente manera: 
		![[Pasted image 20230615215748.png]]
		*Para correr una aplicación que aun depende de la consola:* 
			Aplicación + & (Para correrlos en en el fondo dependiendo dela consola en la que se corrió el comando. 
		*Para correr una aplicación que no depende de la consola y es totalmente independiente*
			comando + & + > + ruta de destino + disown 
			![[Pasted image 20230615221022.png]]
**Descriptores de archivo**
	<span style="color:rgb(255, 0, 0)">exec</span> se usa para utilizar un descriptor de archivos.
	<span style="color:rgb(255, 0, 0)">exec 3&lt;&gt; file</span> Así se crea un descriptor de archivos con permisos de lectura (**<**) y de <span style="color:rgb(255, 0, 0)">escritura (**>**)</span> que usará el numero 3 redireccionando todos los comandos al archivo con el nombre "file" (*No usar el numero 1 ni el 2, porque son la manera de identificar el output y los errores que tenga un comando*) 
	<span style="color:rgb(255, 0, 0)">**exec 3>&**</span> manera de cerrar el descriptor de archivos que creaste antes. <span style="color:rgb(255, 192, 0)">*RECUERDA USAR EL NUMERO CON EL QUE CREASTE EL DESCRIPTOR, en este caso es el 3*</span>
	<span style="color:rgb(255, 192, 0)">&lt</span> Permiso de solo lectura en el archivo que esta creando. 
	<span style="color:rgb(255, 192, 0)">></span> Permiso de escritura en el archivo que se esta creando. 
	<span style="color:rgb(255, 0, 0)">whoami >&3</span> Así se mandan los output de los comandos al archivo previamente creado que esta usando el descriptor como lugar de destino. Se usa el numero que se le dio al descriptor, en este caso es el 3.
	![[Pasted image 20230615222701.png]]
<span style="color:rgb(0, 112, 192)">Lectura e interpretación de permisos 1</span>
	<span style="color:rgb(255, 0, 0)">Touch</span> se usa para crear archivos. (touch + nombre del archivo +  extensión .txt .pdf)
	<span style="color:rgb(255, 0, 0)">whoami > file.txt</span> Envía el output al archivo file.txt. 
	<span style="color:rgb(255, 0, 0)">whoami >> file.txt</span> Envía el output al archivo file.txt y si tiene ya contenido el archivo le hace un salto de línea sin eliminar el anterior contenido del archivo. 
	![[Pasted image 20230720185506.png]]
	**d** Es un directorio.
	**.** Es un archivo. 
	*p (Letras dibujadas)* Es el grupo de permisos para el propietario del archivo
	*G (Letras dibujadas)* Es el grupo de permisos para el grupo. 
	*O (Letras dibujadas)* Es el grupo de permisos para otros fuera tanto del propietario como del grupo. 
	**R** Permisos de lectura (Read)
	**W** Permiso de escritura (Wirite)
	**X** Permiso de ejecución  (execution)
**Lectura e interpretación de permisos 2**
	**|** Para aplicar un filtro dentro de un directorio
	**grep** Va en conjunto con el "|" para filtrar por lo que quieras buscar usando el grepeo de los directorios o archivos. 
	**grep "Lo que sea"** Para buscar por lo que se coloque dentro de las comillas. 
	**grep "ENCRYPT_METHOD"** Reportar el método de encriptación en el que las contraseñas se muestran en el /etc/shadow. 
**Asignación de permisos**
	**chmod** Comando para asignar privilegios. 
	**rm -r** * Sirve para eliminar de forma recursiva directorios dentro de otros directorios y archivos dentro de este mismo directorio. 
	**chgrp** Cambio de grupos a los directorios. 
	![[Pasted image 20230720194224.png]]
	**usermod** modificar todo conforme a un usuario.
	**touch** Para crear archivos
	*creación de usuarios* #users 
	**useradd +nombre del usuario +** 
		Primero que todo, *se crea el directorio que usará* este nuevo usuario, preferiblemente en la ruta /home/(nombre del usuario a agregar).
		 **useradd +nombre del usuario +** Para crear un nuevo usuario. **-s** Para darle la Shell al usuario que se esta creando. **-d** para darle el directorio personal del usuario. Se tienen que colocar la ruta absoluta, tanto para la Shell como para el directorio personal. 
		*rutas absolutas para Shell*
		*/bin/bash* para una bash.
		*/bin/zsh* para una zsh. 
	**passwd** Agregar contraseña al usuario. 
		*passwd* + (nombre del usuario) enter. luego escribes la contraseña. 
	**chgrp** para cambiar el grupo al que pertenece un directorio. 
		*chgrp* + nombre del directorio + nombre del usuario a colocar. Ejem: *(chgrp l4mb l4mb)*
	**chown +nombre de propietario a cambiar +:+nombre del grupo a cambiar** opciones para cambiar tanto grupo como propietario a un directorio. Ejem *(chown l4mb:root l4mb)*
	**groupadd + (nombre del grupo)** se usa para crear nuevos grupos ejem (groupadd alumnos)
	**usermod -a -G +nombre del grupo +nombre del usuario** Para añadir a un usuario a un grupo existente. ejem (usermod -a -G alumnos l4mb)
	**cat /etc/group** Para poder ver los grupos creados y que existen. Se puede aplicar filtros para una respuesta mas concreta a lo buscado. 
	**userdel -f -r +nombre del usuario** Comando para poder eliminar usuarios del sistema. 
<span style="color:rgb(0, 112, 192)">Notación octal de permisos</span>
	![[Pasted image 20230730163037.png]]
	*equivalencias de números a permisos*
		<span style="color:rgb(255, 192, 0)">4</span> =*Read* = <span style="color:rgb(255, 192, 0)">r</span>.
		<span style="color:rgb(255, 192, 0)">2</span> = *Wirite* = <span style="color:rgb(255, 192, 0)">w</span>.
		<span style="color:rgb(255, 192, 0)">1</span> = *Execution* = <span style="color:rgb(255, 192, 0)">x</span>. 
		Las sumas de estos determinaran los permisos que se quieren añadir:
		<span style="color:rgb(255, 192, 0)">7</span> todos los permisos. 
		<span style="color:rgb(255, 192, 0)">6</span> r + w. 
		<span style="color:rgb(255, 192, 0)">5</span> r + x. 
		<span style="color:rgb(255, 192, 0)">4</span> r.
		<span style="color:rgb(255, 192, 0)">3</span> w + x.
		<span style="color:rgb(255, 192, 0)">2</span> w.
		<span style="color:rgb(255, 192, 0)">1</span> x. 
		
<span style="color:rgb(0, 112, 192)">Permisos especiales - Sticky Bit</span>
	Los permisos sobre los directorios son los permisos que se usarán sobre los archivos dentro del directorio. Así que es mejor modificar los permisos del directorio. 
	**chmod +t + nombre del directorio** Se usa para poner un permiso especial llamado Sticky Bit, donde solo el usuario creador del archivo pueda *eliminarlo* o *renombrarlo*. Funciona en directorios donde todos los usuarios tienen permiso de lectura y escritura. 
<span style="color:rgb(0, 112, 192)">Control de atributos de ficheros en Linux - Chattr y Lsattr</span>
	<span style="color:rgb(255, 0, 0)">lsattr</span> Es un comando para listar las flags especiales de 
	permisos diferentes a <span style="color:rgb(255, 192, 0)">rwx</span>. 
	*parámetros mas usados, se colocan después de un solo guion y pegados*
		<span style="color:rgb(255, 192, 0)">-a</span> Mostrar los ficheros ocultos.
		<span style="color:rgb(255, 192, 0)">-R</span> Mostrar de manera recursiva los ficheros.
		<span style="color:rgb(255, 192, 0)">-V</span> Verbose. Mostrar por consola todos los pasos de manara descriptiva.
		<span style="color:rgb(255, 192, 0)">-f</span> Suprimir los errores.
		<span style="color:rgb(255, 192, 0)">-v</span> Versión del archivo.
		<span style="color:rgb(255, 192, 0)">-d</span> Mostrar atributos del directorio, no de los archivos dentro del directorio.
		<span style="color:rgb(255, 192, 0)">mode</span> Establezca los atributos ocultos del archivo, el formato es+-=[acdeijstuACDST]. 
	<span style="color:rgb(255, 0, 0)">chattr</span> Comando para añadir flags especiales de permisos. 
		*Parámetros para añadir flags a los ficheros*
			<span style="color:rgb(255, 192, 0)">A</span> Deshabilitar la modificación de la fecha de acceso al fichero. 
			<span style="color:rgb(255, 192, 0)">a</span> Añadir contenido al fichero, pero no eliminarlo. 
			<span style="color:rgb(255, 192, 0)">c</span> Comprimir automáticamente el fichero en el disco.
			<span style="color:rgb(255, 192, 0)">i</span> Inmutable. Bloquear la modificación o borrado de un archivo.
			<span style="color:rgb(255, 192, 0)">u</span> Permitir recuperación de archivo aunque se haya eliminado. Se guardan el contenido del archivo a pesar de que se haya eliminado para así poder recuperarlo. 
			<span style="color:rgb(255, 192, 0)">e</span> Al eliminar un archivo, sobrescribir con cero todos sus bloques. 
			<span style="color:rgb(255, 192, 0)">S</span> Escribir de forma sincrónica a disco cambios en el fichero. De forma directa. 
			<span style="color:rgb(255, 192, 0)">d</span> Eso no es un volcado, el archivo de configuración no puede ser el objetivo de respaldo del programa de volcado.
			<span style="color:rgb(255, 192, 0)">j</span> Es decir, diario, establezca este parámetro para que cuando el sistema de archivos se monte con el parámetro de montaje "datos = ordenados" o "datos = reescritura", el archivo se grabará primero cuando se escriba (en el diario). Si el parámetro del sistema de archivos se establece en data = journal, el parámetro es automáticamente inválido.
			*Se pueden asignar todos a la vez*
			![[Pasted image 20230731131627.png]]
**Permisos especiales SUID y SGID**
	<span style="color:rgb(255, 0, 0)">which</span> comando para ver la ruta absoluta de algún binario o programa. 
	<span style="color:rgb(255, 0, 0)">xargs</span> Manera de poder realizar dos comandos de manera paralela o al mismo tiempo. Se coloca después del comando colocando un | y luego xargs, después de eso se coloca el comando que quieres ejecutar paralelamente. 
	<span style="color:rgb(255, 0, 0)">suid</span> Es la manera de poder correr un programa como el propietario del archivo. 
	![[Pasted image 20230814204549.png]]
	<span style="color:rgb(255, 0, 0)">SGID</span> Es la manera de entrar a un directorio como si hiciéramos parte del grupo al que pertenece el directorio. 
	 ![[Pasted image 20230814204506.png]]

<span style="color:rgb(0, 112, 192)">Permisos especiales y Capabilities</span>
	<span style="color:rgb(255, 0, 0)">getcap</span> Comando para encontrar todas las capabilities en el sistema. 
		*uso* <span style="color:rgb(255, 0, 0)">getcap -r / 2>/dev/null</span> 
	<span style="color:rgb(255, 192, 0)">Setear capabilities en el sistema</span> <span style="font-style:italic; color:rgb(255, 0, 0)">setcap</span> (capabilitie a setear) (ruta del archivo)
	<span style="color:rgb(255, 0, 0)">find</span> Se usa para buscar lo que sea por el sistema.
	se puede usar de diferentes maneras, las que he visto en este curso son:
	ejem: <span style="color:rgb(255, 0, 0)">find -type f -perm -4000</span> (para buscar los archivos que tienen capabilities en el sistema) 
<span style="color:rgb(0, 112, 192)">Directorios</span>
	*<span style="color:rgb(255, 0, 0)">bin</span>* Almacena los ejecutables de usuario. 
	*<span style="color:rgb(255, 0, 0)">Sbin</span>* hace lo mismo pero para tareas del sistema operativo.
	*boot* Directorio estatico que contiene todos los archivos necesarios para arrancar el sistema. Aquí se encuentra el gestor de arranque *GRUB.*
	*dev* Todos los dispositivos de almacenamiento en forma de archivo. Para que el sistema pueda entenderlos como sistema de almacenamiento. 
	*etc* Archivos de configuración del sistema operativo como tal, como los programas instalados después. No debe contener preferiblemente binarios. 
	*home* Directorios de los usuarios estándar.
	*lib* Contiene todas las librerías necesarias para correr todas las aplicaciones que tiene el sistema montadas. 
	*lib64* Contiene también las líberias para aplicaciones de 64 bits. 
	*media* El punto de montaje de todos los volúmenes lógicos que se montan. (Disco duro, etc)
	*opt* Contiene archivos de programas autocontenidos, como la carpeta archivos x86 de windows. 
	*proc* Procesos y aplicaciones que se están ejecutando en el sistema. Este directorio no guarda nada, debido a que son archivos virtuales, por lo que el contenido de este directorio es nulo. Contiene listas que se generan al momento de acceder a ellos. 
	*root* Este es la carpeta del usuario root. Esta esta en su propia carpeta desde la raíz del sistema operativo.
	*srv* Almacenar archivos de directorios relativos que estén en un servidor instalado en tu sistema.
	*sys* Archivos virtuales de eventos del sistema operativo. Estos se distribuyen de manera jerárquica. 
	*tmp* Almacena archivos temporales del sistema o aplicaciones que estén corriendo. Este esta diseñado para almacenar archivos de corta duración, y cada vez que se reinicia la maquina se borra el contenido de este directorio.
	*usr* Archivos de solo lectura incluyendo todos los paquetes del software instalados en el sistema. User System Resourses. En este directorio se almacena por lo tanto todos los recursos del usuario que están instalados en el sistema.
	*var* Actua como un registro del sistema. Contiene almacenados los registros del sistema, los correos, logs, información almacenada en la cache, bases de datos. 
<span style="color:rgb(0, 112, 192)">Uso de tmux</span>
	*ctrl + b + ,* = renombrar ventana.
	*ctrl + b + shift + "* = crea nuevo panel horizontalmente.
	*ctrl + b +shift + %* = crea un nuevo panel verticalmente. 
	*ctrl + b + o* = cambia entre paneles. 
	*ctrl + b + x* = cerrar un panel. 
	*ctrl + b + $* = cambiar el nombre de la sesión tmux.
	*ctrl + b + 1* = cambiar de ventana de la sesión. (dependiendo de las ventanas que hayan abiertas)
	*ctrl + -* = disminuir el tamaño de la letra de la Shell.
	*ctrl + +* = aumentar el tamaño de la letra de la Shell.
	*ctrl + b + m* = cambiar al modo mouse.
	*ctrl + b + w* = cambiar al modo directory (para ver todas las sesiones y ventanas que hay activas).
<span style="color:rgb(0, 112, 192)">Dumpeos hexadecimales en archivos y descompresión de archivos continuamente comprimidos</span>
	Se copia el contendió del del archivo que contiene el dumpeo hexadecimal:
	![[Pasted image 20241013174805.png]]
		<span style="color:rgb(255, 0, 0)">xxd</span> Este comando se usa con el fin de convertir y hacer todo tipo de operaciones con hexadecimales tanto de conversión como de reversear contenidos ya en hexadecimal.
			Parámetros:
			<span style="color:rgb(255, 192, 0)">xxd [file]</span> Muestra el contenido de un archivo en formato hexadecimal y ASCII.
			<span style="color:rgb(255, 192, 0)">xxd -r [file]</span> Este realiza el reverseo de información en decimal.
			<span style="color:rgb(255, 192, 0)">xxd -l [file]</span> Muestra solo los primeros `length` bytes del archivo.
			<span style="color:rgb(255, 192, 0)">xxd -c [file]</span> Define cuántos bytes se deben mostrar por línea (por defecto son 16).
			<span style="color:rgb(255, 192, 0)">xxd -g [file]</span> Agrupa los bytes en unidades de tamaño `bytes` (por ejemplo, `-g 1` agrupa de a un byte).
			<span style="color:rgb(255, 192, 0)">xxd -s [file]</span> Comienza la visualización a partir del byte número `seek` del archivo.
			<span style="color:rgb(255, 192, 0)">xxd -p [file]</span> Muestra solo los valores hexadecimales en una línea continua, sin direcciones ni ASCII.
			<span style="color:rgb(255, 192, 0)">xxd -i [file]</span> Genera un arreglo en lenguaje C que contiene los bytes del archivo.
			<span style="color:rgb(255, 192, 0)">xxd -u [file]</span> Muestra los números hexadecimales en mayúsculas.
			<span style="color:rgb(255, 192, 0)">xxd -b [file]</span> Muestra el contenido en formato binario, en lugar de hexadecimal.
			<span style="color:rgb(255, 192, 0)">xxd -e [file]</span> Cambia el orden de los bytes para reflejar el cambio en endianness (little endian a big endian o viceversa).
			<span style="color:rgb(255, 192, 0)">xxd -v</span> Muestra la versión de `xxd`.
		con xxd reverseamos todo el contenido del archivo que contiene el dumpeo hexadecimal. Ejemplo:
			<span style="color:rgb(255, 192, 0)">xxd -r [file] | sponge [file]</span> (redireccionamos el output del comando al mismo archivo con el comando sponge) ejemplo: 
			![[Pasted image 20241013190517.png]]
	pasamos el comando <span style="color:rgb(255, 192, 0)">file</span> al archivo con el fin de ver que tipo de archivo es:
		![[Pasted image 20241013190701.png]]
	Cambiamos el nombre del archivo según lo que nos reporte el comando file. 
	Posteriormente se realiza el extractor de archivos con el siguiente codigo:
```bash
#!/bin/bash

#Ctrl+C
function ctrl_c(){
    echo -e "\n\n [+] Saliendo...\n"
    exit 1
}
    trap ctrl_c INT
    
    primer_nombre="data.gz"
    archivoDescomprimido="$(7z l data.gz | tail -n 3 | head -n 1 | awk 'NF {print $NF}')"
    7z x $primer_nombre &>/dev/null
    
while [ $archivoDescomprimido ]; do
    echo -e "\n[+] Se ha descomprimido un nuevo archivo: $archivoDescomprimido"
    7z x $archivoDescomprimido &>/dev/null
    archivoDescomprimido="$(7z l $archivoDescomprimido 2>/dev/null | tail -n 3 | head -n 1 | awk 'NF {print $NF}')"
done
```
Esto nos dará el ultimo archivo descomprimido el cual contiene la contraseña del siguiente nivel.

---
	<span style="color:rgb(0, 112, 192)">Uso del comando ssh para conexiones seguras con open SSH</span>
	 <span style="color:rgb(255, 0, 0)">ssh -i "nombreDelArchivo" user@dondeSeQuieraConectar</span> Este comando nos permite conectarnos a una maquina proporcionando un archivo que contiene una llave privada. Ejemplo:![[Pasted image 20240728131932.png]]
	 
---
<span style="color:rgb(0, 112, 192)">Comandos para listar protocolos y puertos en la maquina</span> 
<span style="color:rgb(255, 0, 0)">lsof -i:puertoARevisar</span> Este comando nos permite revisar el servicio que esta corriendo por un puerto en especifico.
<span style="color:rgb(255, 0, 0)">ss -nltp</span> Este comando nos permite ver los puertos y protocolos que están corriendo. 
<span style="color:rgb(255, 0, 0)">cat /proc/net/tcp</span> Este archivo almacena un listado de los puertos en hexadecimal.

---
<span style="color:rgb(0, 112, 192)">Uso del comando netcat (nc)</span>
<span style="color:rgb(255, 0, 0)">nc ipoConexión puerto</span> Este comando nos permite conectarnos a maquinas tanto remotas como locales por el puerto especifico que se deseé. Ejemplo:
![[Pasted image 20240728135426.png]]
Posteriormente si se proporciona la contraseña se podrá acceder a dicha maquina. 

---
<span style="color:rgb(0, 112, 192)">Conexiones usando cifrado ssl con openssl</span>
	SSL es un tipo de conexión segura que nos permite conectarnos a maquinas remotas de manera cifrada, por lo que no se pueden realizar robos de información de ninguna manera.
	Comandos:
	<span style="color:rgb(255, 0, 0)">openssl s_client  -connect dondeSeQuiereConectar:puerto</span> Este comando nos permite conectarnos a la maquina usando openssl siempre que esté instalado en nuestro equipo.

---
<span style="color:rgb(0, 112, 192)">PortScan para puertos abiertos con una bash</span>
	Con el fin de escalar a bandit17 se realiza un script de escaneo de puertos para identificación de puertos abiertos usando la función de la bash <span style="color:rgb(255, 192, 0)">/dev/tcp/127.0.0.1/port</span> con el fin de realizar redirecciones a esta función y así poder identificar que puertos están abiertos.
```java
#!/bin/bash


function Ctrl_c(){
        echo -e "\n\n[*] Saliendo... \n"
        exit 1
}

trap Ctrl_c INT
for port in $(seq 31000 32000); do
        (echo '' > /dev/tcp/127.0.0.1/$port) 2> /dev/null && echo -e "\n\n [+] $port - OPEN"
done
```
<span style="color:rgb(255, 0, 0)">Explicación</span>
	- <span style="color:rgb(255, 192, 0)">function Ctrl_c</span>
		Esta función realiza la captura de las teclas <span style="color:rgb(255, 0, 0)">Ctrl + c</span> con el fin de salir en cualquier momento de la ejecución del script. Adicionalmente, el <span style="color:rgb(255, 0, 0)">exit 1</span> nos permite configurar el código de estado. En este caso el <span style="color:rgb(255, 0, 0)">1</span> es un error y el <span style="color:rgb(255, 0, 0)">0</span> es ok.
	- <span style="color:rgb(255, 192, 0)">trap Ctrl_c INT</span> 
		Esta función nos permite capturar en cualquier momento la combinación de teclas y redirigirla a la función de salida del código. 
	- <span style="color:rgb(255, 192, 0)">for port in $(seq 31000 32000); do</span>
		Esta función nos permite realizar un bucle entre una secuencia deseada con el comando <span style="color:rgb(255, 0, 0)">seq</span> donde pasándole dos parámetros con un  espacio entre medias realizamos una secuencia empezando por el primer parámetro y terminando en el segundo. 
	- <span style="color:rgb(255, 192, 0)">(echo '' > /dev/tcp/127.0.0.1/$port) 2> /dev/null && echo -e "\n\n [+] $port - OPEN</span>
		Este código nos permite enviar una cadena vacía a la función <span style="color:rgb(255, 0, 0)">/dev/tcp/</span> del mismo sistema en el que estamos. Con la variable <span style="color:rgb(255, 0, 0)">$port</span> nos encargamos de que el valor que vaya tomando se coloque donde está colocada esta variable para indicar el puerto que queremos analizar si está abierto. Con <span style="color:rgb(255, 0, 0)">&&</span> hacemos que si todo el código anterior (<span style="color:rgb(255, 0, 0)">echo '' > /dev/tcp/127.0.0.1/$port) 2> /dev/null</span>) tiene una salida con estado correcto (<span style="color:rgb(255, 0, 0)">0</span>) todo lo demás se interprete. De lo contrario, como el código de estado es errado (<span style="color:rgb(255, 0, 0)">1</span>) no se interpretará. Con <span style="color:rgb(255, 0, 0)">echo -e "\n\n [+] $port - OPEN</span> imprimimos un mensaje por consola para mostrar los puertos que están abiertos.
Posteriormente usaremos netcat con la opción de usar ssl para conectarnos a los diferentes puertos que nos reporte este script de la siguiente forma: 
<span style="color:rgb(255, 0, 0)">Explicación</span>
	![[Pasted image 20241014172650.png]]
	- <span style="color:rgb(255, 192, 0)">nc --ssl localhost port</span>
	Este comando nos permite conectarnos usando ssl con netcat. Se debe proporcionar la ip y el puerto al que se desea conectar. 
	- Al conectarnos al localhost por el puerto que estemos probando y demos la clave de bandit16, el servidor nos dará una llave con la cual conectarnos al siguiente nivel. Guardando esta clave en un archivo <span style="color:rgb(255, 0, 0)">id_rsa</span> y proporcionándolo con ssh podremos conectarnos como bandit17.

---
<span style="color:rgb(0, 112, 192)">Encontrando diferencias con el comando diff</span>
	Con el comando diff podemos listar las líneas diferentes pasándole como parámetros dos archivos que queramos comparar de la siguiente forma:
	<span style="color:rgb(255, 0, 0)">diff archivo1 archivo2</span> Esto mostrará las diferencias entre `archivo1` y `archivo2`. Las líneas que son diferentes se mostrarán con un prefijo indicando si se debe agregar (`>`) o eliminar (`<`).

---
<span style="color:rgb(0, 112, 192)">Listar procesos del sistema y comandos</span>
<span style="color:rgb(255, 0, 0)">ps -faux</span> Este comando nos permite listar todos los procesos que están corriendo en la maquina.
<span style="color:rgb(255, 0, 0)">ps -eo command</span> Este comando nos permite listar todos los comandos que se están ejecutando en la maquina actual.

---
<span style="color:rgb(0, 112, 192)">Decimal a hexadecimal</span>
Aquí hay un ejemplo de como convertir una lista de palabras de hexadecimal a decimal.
![[Pasted image 20240728143649.png]]


