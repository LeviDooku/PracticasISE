# Bitácora P2L1

## Enunciado: 

Usted está trabajando en una empresa proveedora de servicios y recibe la solicitud de un cliente que desea tener un servidor para la implantación de un comercio electrónico. Una vez diseñado y configurado el almacenamiento, debe enviarle las credenciales de acceso para que el equipo de desarrollo pueda desplegar la aplicación. Dado que FTP no es un protocolo seguro, decide configurar SSH para que el usuario sea capaz de acceder a la consola de manera segura y asegurada (secure and hardened). Para ello, configurará el servicio de modo que el usuario root no pueda tener acceso, que sea posible la autenticación por llave pública, restringirá el usuario que puede acceder acceso y limitará los ataques por fuerza bruta. Además cambiará el puerto por defecto para que no esté en la lista de barrido rápido de nmap.

### Introducción y conceptos

En este ejercicio se debe montar un servicio SSH. Antes de entrar en más detalles sobre el enunciado, se debe definir lo que es SSH y el porqué del protocolo:

- SSH (Secure Shell): un protocolo seguro que proporciona autenticación, encriptación de los datos y control sobre la integridad de los datos que viajan. Es una evolución a protocolos como el FTP que enviaban datos en texto plano, fácilmente vulnerables a ataques de sniffing.

El uso sigue un modelo cliente - servidor, en el que el servidor (lo que se pide configurar), tendrá siempre escuchando un servicio `ssh_d`  (de viene de daemon) y un cliente invocará el servicio ssh para que el servidor le brinde el servicio. Además un servidor puede a su vez ser cliente.

### Diseño

Lo que se hará será configurar los archivos /etc/ssh/sshd_config y el /etc/ssh/ssh_config, para configurar tanto el servicio como el cliente.  

Como máquina se usa una con Almalinux.

### Implementación del sistema

Lo primero es comprobar si se tiene ssh instalado en la máquina, en el caso de Almalinux, sí. Ahora se debe ver si el servicio ssh está activo, para ello se puede usar el comando `ps -Af | grep sshd`, en el caso de esta máquina, sí aparece, esto es un comportamiento deseable. Se puede ahora probar a conectarse al localhost usando `ssh localhost`.

Una vez hecho esto, se creará en el home un directorio .ssh con un archivo know_host, al consultarlo, se puede ver el fingerprint de los host conocidos.

Ahora, se configura el archivo de configuración del servicio, para evitar que el usuario root pueda acceder mediante ssh:

```
sudo vi /etc/ssh/sshd_config
```

Dentro hay que encontrar la opción PermitRootLogin y cambiarla a "no". Posteriormente se reinicia el servicio mediante

```
sudo systemctl restart sshd
```

Ahora se puede comprobar si se puede acceder como root a la máquina haciendo, desde una terminal en nuestro sistema anfitrión:

```
ssh -l root <ip>
```

![con](../img/P2L1/P2L1_con.png)  

NOTA: en la imagen no se accede como root, pero al intentarlo, pide la contraseña y aunque sea correcta, no deja acceder.

Se pueden verbosear la interacción añadiento el switch -v al comando. Esto es útil para hacer debug.

Otra buena práctica (pedida además por el enunciado) es cambiar el puerto por defecto del servicio, el cual es el 22.

Para esto se edita el archivo de configuración ya conocido y se busca la directiva Port. Se escoge un puerto mayor al 100 (los que escanea `nmap`) y se reinicia el servicio de nuevo. 

En este punto dará un error, se puede consultar exactamente el problema con `journalctl -xe`. El problema reside en que tenemos que indicar al SO de este cambio. De hecho, en la cabecera del propio archivo de configuración, aparece este detalle. Para realizar este cambio se usa el comando `semanage` que NO viene instalado por defecto en Almalinux. Después de instalarlo, se ejecuta:

```
sudo semanage port -l | grep ssh #Listar los tipos de puerto relacionados con ssh
sudo semanage port -a -t ssh_port_t -p tcp 22022
sudo semanage port -l | grep ssh #Comprobar que aparece el puerto 22022
```

![semanage](../img/P2L1/P2L1_semanage.png)  

Al reiniciar el servicio ahora, no da errores.

Pero sigue sin acceder desde la máquina anfitrión. Esto es debido al Firewall, que no permite acceso a este puerto. Al menos, esto tiene fácil solución, se debe añadir un puerto al Firewall para que permita el acceso, esto se hace mediante `firewall-cmd` una interfaz para la configuración. Previamente es recomendable consultar el manual.

```
sudo firewall-cmd --add-port 22022/tcp --permanent #Añadir el puerto de manera permanente, pero no abre el puerto por defecto
sudo firewall-cmd --add-port 22022/tcp #Agregar el puerto
```

Ahora, sí que permite el acceso en el puerto 22022:

![fin](../img/P2L1/P2L1_fin.png)  
