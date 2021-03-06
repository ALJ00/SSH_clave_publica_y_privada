
![](capturas/logo.png)

# SSH clave pública y privada

SSH (o Secure SHell) es el nombre de un protocolo y del programa que lo implementa cuya principal función es el acceso 
remoto a un servidor por medio de un canal seguro en el que toda la información está cifrada. Además de la conexión a 
otros dispositivos, SSH permite copiar datos de forma segura (tanto archivos sueltos como simular sesiones FTP cifradas),
gestionar claves RSA para no escribir contraseñas al conectar a los dispositivos y pasar los datos de cualquier otra 
aplicación por un canal seguro tunelizado mediante SSH y también puede redirigir el tráfico del (Sistema de Ventanas X) 
para poder ejecutar programas gráficos remotamente. El protocolo TCP asignado es el 22. [Ver más información, Wikipedia.](https://es.wikipedia.org/wiki/Secure_Shell)

Si configuramos este protocolo de conexión utilizando clave pública y privada, la autenticación de SSH tendrá un "plus" 
en lo que se refiere a la seguridad del servidor. El uso de estas claves en SSH implica un cifrado [criptográfico asimétrico](https://es.wikipedia.org/wiki/Criptograf%C3%ADa_asim%C3%A9trica)
que una simple password no es capaz de ofrecer. Además de esto, hace posible que los usuarios puedan realizar conexiones
al servidor sin necesidad de recordar contraseñas largas.

## Windows

En esta práctica vamos a utilizar como servidor Ubuntu Server y como cliente Windows 10 Pro y para comenzar descargaremos 
[putty](https://www.ssh.com/ssh/putty/download#sec-Download-PuTTY-installation-package-for-Windows) ,cliente SSH entre 
otros con licencia libre actualmente disponible en varias plataformas Unix. [Más información PuTTY.](https://www.google.com/search?q=putty&oq=putty++&aqs=chrome..69i57j0j35i39l2j0l2.3415j1j8&sourceid=chrome&ie=UTF-8)

![](capturas/puttyGem.PNG)

### Generar las claves

Primeramente generaremos las claves pulsando el botón generate. Guardaremos tanto la clave pública y la privada 
en una ubicación del ordenador y habrá que copiar posteriormente la clave pública para pegarla en un archivo llamado
authorized_keys que habrá que crear en el servidor. 

![](capturas/generarKey.PNG)

Generadas y guardadas la claves [descargaremos winSCP](https://winscp.net/eng/download.php) que es una aplicación de 
software libre que funciona como cliente SFTP gráfico para Windows y que emplea SSH. [Ver más información.](https://es.wikipedia.org/wiki/WinSCP)

![](capturas/windscp.PNG)

Instalado el programa habrá que introducir la IP del servidor y su correspondiente contraseña para que estén conectados
ambos equipos. Hecho esto podremos visualizar los paneles de ambos como se puede apreciar en la imagen superior. 
Ayudándonos de winSCP crearemos en nuestro directorio de usuario del panel del servidor el archivo ***`.ssh/authorized_keys`*** 
en el que copiaremos la clave pública generada anteriormente. No hay que olvidarse de acceder en el equipo
servidor al archivo sshd_config mediante el comando ***`sudo nano sshd_config`*** para descomentar la línea 
***#Authorizedkeysfile...*** .

![](capturas/descomentarSshconfig.PNG)

### Comprobar conexión

Hecho esto es hora de comprobar la conexión por lo que iremos a Putty para ejecutarlo. En la sección Session introduciremos los
datos del servidor remoto: IP o host (campo “Host Name (or IP address)“) y puerto (campo “Port“).

![](capturas/conexionEjecPutty.PNG)

En la sección Connection -> Data introduciremos el nombre de usuario (campo “Auto-login username”) en el servidor remoto para 
el que estamos configurando las claves.

![](capturas/conecctiondata.PNG)

Posteriormente iremos a  la sección Connection -> SSH -> Auth y le indicamos la ubicación de la clave privada (extensión .ppk) 
creada previamente.

![](capturas/ubicacionClavePrivada.PNG)

Hecho esto guardaremos la sesión con un nombre determinado, que en este caso será la IP del servidor al que queremos 
conectar, ya que es un servidor de la red local y lo identificamos fácilmente. Para ello vamos a la sección Session, seleccionamos
una sesión existente (o creamos una nueva introduciendo el nombre en el campo “Saved Sessions“) y hacemos clic en el botón “Save“.

![](capturas/saveSession.PNG)

Con esto ya estaría todo configurado para conectarnos al servidor. Para ello bastará con hacer doble clic sobre la sesión 
guardada o bien pulsar el botón open.

![](capturas/comprobarConexion.PNG)

## Ubuntu

Para la siguiente práctica utilizaremos dos equipos Ubuntu, Desktop como cliente y Server como equipo servidor. 

### Generar claves

Ayudándonos del comando ***`ssh-keygen`*** creamos la clave pública y privada en el equipo cliente.

![](capturas/ubuntuKeys.PNG)

Tal como se muestra en la imagen superior se crean las dos claves e indica dónde se crean.

### Copiar la clave pública en el equipo servidor

Generadas la claves el siguiente paso va a ser copiar la clave pública ssh al servidor remoto. Para ello utilizamos el comando
***`scp /home/sr/.ssh/id_rsa.pub usuario@ip:home`*** . Aquí estamos indicando de qué archivo del cliente tenemos que enviar
la clave y dónde la queremos dejar en el servidor  ***`usuario@ip:home`*** .

![](capturas/envioClaveUbuntu.PNG)

Ejecutado el comando nos pedirá usuario y contraseña y el envío se habrá realizado como se aprecia en la imagen superior.
Posteriormente ya podremos conectarnos con el servidor sin necesidad de introducir ninguna clave. Podemos comprobarlo mediante
el comando ***`ssh usuarioservidor@ip_servidor`*** 

![](capturas/conexionSin.PNG)

Como se aprecia en la imagen Ubuntuserver nos da la vienbenida en vez de pedirnos alguna clave y podemos notar que ya estamos
en el equipo remoto.

Por último quedaría probar en el servidor que la clave se ha enviado y que se ha copiado correctamente.

![](capturas/comprobacionServidor.PNG)

Como se puede apreciar en la imagen observamos que al ejecutar el comando de copia en el cliente ***`scp /home/sr/.ssh/id_rsa.pub usuario@ip:home`***
al poner en el comando ***:home*** estamos indicando que en el servidor queremos que se copie la clave en ese archivo. 
Por lo tanto en el servidor moveremos el archivo ayudándonos del comando ***`cat`*** , el cual nos permite realizar varias 
acciones:
* Mostrar archivos de texto.
* Copiar archivos de texto en un documento nuevo.
* Agregar el contenido de un archivo de texto al final de otro archivo de texto, combinándolos.
* Mostrar archivos de texto.

Como vemos hemos ido al directorio home creado donde se encuentra la clave ***`cat home`*** y hemos copiado el archivo y
a posteriori hemos copiado la clave en el archivo authorized_keys ***`cat home > .ssh/authorized_keys`*** . 





