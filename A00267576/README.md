### Taller 3
**Universidad ICESI**  
**Curso:** Sistemas Operativos  
**Docente:** Daniel Barragán C.  
**Tema:** Llamadas al sistema (syscalls)  
**Correo:** daniel.barragan at correo.icesi.edu.co  

**Estudiante:** Óscar Daniel Molano Torres  
**Código:** A00267576  
**Link:** https://github.com/odm95/so-workshop3/tree/A00267576/add-solutions  


### Objetivos
* Conocer y explicar la función de las llamadas al sistema en un sistema operativo

### Descripción
El tercer taller del curso sistemas operativos trata sobre las llamadas al sistema y su importancia para el sistema operativo

### Actividades  
1. Empleando el aplicativo **strace** obtenga 5 llamadas al sistema para uno o varios comandos de linux. Explique por qué los comandos seleccionados emplean las llamadas al sistema encontradas, para ello debe emplear los manuales de Linux en Internet o del sistema operativo (comando **man**). Debe incluir la explicación de los parámetros que reciben las llamadas al sistema encontradas. Consigne capturas de pantalla donde muestre las llamadas al sistema obtenidas (sugerencia: emplear -etrace para filtrar los resultados).

**Comando ejecutado:** strace ls  

![][1]  
![][2]  
![][3]  

### 1.mmap:  

**Comando:** mmap(NULL, 19180, PROT_READ, MAP_PRIVATE, 3, 0)  
**Descripción:** Ubica, elimina ficheros o dispositivos en memoria. Proyecta el contenido de un archivo en memoria.  
**Método:** void *mmap (void *start, size_t length, int prot, int flags, int fd, off_t offset);  

**Parametros:**  

start  
Dirección para alojar la proyección. Si NULL el sistema elige.  

length  
Cantidad del fichero a mapear.  

prot  
Describe la protección de memoria deseada:  
PROT_EXEC = Se permite ejecución  
PROT_READ = Se permite leer.  
PROT_WRITE = Se permite escribir.  
PROT_NONE = Las páginas no pueden ser accedidas.  
Nota: Estas opciones se pueden combinar separándolas por “|”.  

flags  
Modificadores de acceso:  
MAP_FIXED No seleccionar una dirección diferente a la especificada. Si la dirección especificada no puede ser utilizada, mmap fallará.  
MAP_SHARED Comparte este área con todos los otros procesos que señalan a este objeto.  
MAP_PRIVATE Crear un área privada "copy-on-write".  
MAP_ANON, MAPDENY_WRITE, MAP_LOCKED.  

fd  
Descriptor del fichero a usar. Previamente debe haber sido abierto.  

offset  
Desplazamiento del comienzo del mapeo en el archivo.  

### 2. open:  

**Comando:** open("/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3  
**Descripción:** Se utiliza para transformar una ruta de archivo a un descriptor de arhivo (un valor entero no negativo que se utiliza para identificar al archivo abierto).  
**Método:** int open(const char *ruta, int flags, mode_t modo);  

**Parametros:**  

ruta  
Es un arreglo que almacena la ruta del archivo por abrir.

flags  
Es una bandera que le indica a open el modo de apertura, puede ser:  
O_RDONLY, O_WRONLY, O_RDWR, O_APPEND, etc.  

mode_t modo  
Es una bandera que le indica a open los permisos que se le asignarán al archivo si este no existia y se le indicó a open crearlo con la bandera O_CREAT.  

**Retorno:**  

Open regresa un entero el cual es el descriptor de archivo o -1 en caso de error.  


### 3. close:  

**Comando:** close(3)  
**Descripción:** Cierra un descriptor de archivo de manera que pueda ser reutilizado por el sistema. Close no garantiza que los datos del archivo se hayan escrito en el disco por lo que se recomienda el uso de fsync.  
**Método:** int close (int fd);  

**Parametros:**  

Un Descriptor de archivo abierto.

**Retorno:**  

0 en caso de éxito y 1 en caso de error.  


### 4. read:  

**Comando:** read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0 \26\0\0\0\0\0\0"..., 832) = 832    
**Descripción:** Almacena en buf los nbytes leidos del archivo referenciado por (fd).  
**Método:** ssize_t read(int fd, void *buf, size_t nbytes);  

**Parametros:**  

fd  
Es un descriptor al archivo que se intenta leer con read.  

buf  
Es la localidad en memoria donde se almacenaran los bytes leidos.  

nbytes  
Es el numero de bytes que se intentaran leer del archivo.  

**Retorno:**  

read regresa un valor de tipo ssize_t, un entero que indica el numero de bytes leidos ó -1 en caso de error.  


### 5. write:  

**Comando:** write(1, "anaconda-ks.cfg  bin  etc  repos"..., 40anaconda-ks.cfg  bin  etc  repositorios) = 40  
**Descripción:** Escribe hasta nbytes de la localidad de memoria referenciada por buf en elarchivo referenciado por fd.  
**Método:** ssize_t write(int fd, const void *buf, size_t nbytes);  

**Parametros:**  

fd  
Es un descriptor al archivo que se intenta escribir con write.  

buf  
Es la localidad de memoria que almacena la informacion que se va escribir por write.  

nbytes  
es el numero maximo de bytes que se van a escribir al archivo.  

**Retorno:**  

-1 en caso de error o un valor de tipo ssize_t que indica el numero de bytes escritos.  






2. Realice la compilación del código fuente adjunto y su ejecución empleando el aplicativo **strace**. Identifique las llamadas al sistema encargadas de enviar y recibir datos a través de la red. A partir de los manuales de Linux en Internet o del sistema operativo explique las llamadas al sistema encontradas y sus parámetros.  


![][4]  

### 1.sendto:  

**Descripción:** Se utiliza para transmitir un mensaje a otro socket.  
**Metodo:** sendto (sockfd, buf, len, flags, NULL, 0);  

**Parametros:**  

sockfd  
Es el descriptor de archivo del socket de envío.  

buf  
Es la localidad de memoria que almacena la informacion que se va a enviar.  

len  
Cantidad del fichero a mapear.  

flags  
argumento es el operador OR de cero o más de los siguientes banderas:  
MSG_CONFIRM, MSG_DONTROUTE, MSG_DONTWAIT, MSG_EOR, MSG_MORE, MSG_NOSIGNAL, MSG_OOB.  


**Retrorno:**  

En caso de éxito, esta llamada devuelve el número de bytes enviados. En caso de error, -1 se devuelve, y errno se establece correctamente.  


### 2. recvfrom:  

**Descripción:** Recibe un mensaje desde un socket.  
**Metodo:** ssize_t recvfrom(int socket, void *restrict buffer, size_t length, int flags, struct sockaddr *restrict address, socklen_t *restrict address_len);  

**Parametros:**  

socket  
Especifica el descriptor del archivo de socket.  


buffer  
Señala el búfer en el que debe guardarse el mensaje.  

length  
Especifica la longitud en bytes del buffer apuntado por el argumento del búfer.  

flags  
Especifica el tipo de recepción de mensajes. Valores de este argumento se forman por OR'ing lógicamente cero o más de los siguientes valores:  
MSG_PEEK, MSG_OOB, MSG_WAITALL. 

address  
UN puntero nulo, o apunta a una sockaddr estructura en la que se va a almacenar la dirección de envío. La longitud y formato de la dirección dependen de la familia de direcciones del enchufe.  

address_len  
Un puntero nulo, si la dirección es un puntero nulo o un puntero a un objeto socklen_t que especifica en la entrada la longitud de la estructura sockaddr suministrada , y output especifica la longitud de la dirección almacenada.  


**Retrorno:**  

Una vez finalizada con éxito, recvfrom () devolverá la longitud del mensaje en bytes. Si no hay mensajes disponibles para ser recibidos y el par ha realizado un cierre ordenado, recvfrom () devolverá 0. De lo contrario, la función devolverá -1 y establecerá errno para indicar el error.  


## Referencias

* http://man7.org/linux/man-pages/man2/syscalls.2.html  
* https://jvns.ca/blog/2014/09/18/you-can-be-a-kernel-hacker/  
* http://systemcalls.blogspot.com.co/2010/03/mmap.html  
* http://systemcalls.blogspot.com.co/2010/03/open-abre-y-o-crea-un-archivo.html  

[1]: Imagenes/Screen1.PNG  
[2]: Imagenes/Screen2.PNG  
[3]: Imagenes/Screen3.PNG  
[4]: Imagenes/Screen4.PNG  

