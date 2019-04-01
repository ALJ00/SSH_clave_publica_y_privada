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
al servidro sin necesidad de recordar contraseñas largas.

## Windows
Antes de comenzar con el tutorial hay que tener en cuenta que el uso de del par de claves pública y privada implica que 
la privada sólo va a permanecer en el cliente mientras que la pública permanece en el servidor, en el archivo ***` /home/usuario/.ssh/authorized_keys`***
por defecto, aunque se puede modificar en el archivo ***` /etc/ssh/sshd_config`*** .

# Generar las claves
En esta práctica vamos a utilizar como servidor Ubuntu Server por 