# Bitácora P1L2

## Enunciado: 

En esta ocasión, en la empresa en la que le acaban de contratar tenían adquirido un
servidor y su predecesor había realizado la instalación del S.O. Alma Linux, según le
han comentado los compañeros, él solía hacer instalaciones por defecto y luego aplicar
scripts de configuración. 

Sin más información, nuestro jefe nos informa que esa máquina
va a alojar unos cursos con vídeos de alta calidad y relativamente largos. Por tanto,
viendo la configuración del sistemas, prevemos que /var necesitará más espacio, incluso
es conveniente asignarle un LV exclusivamente. Para ello, incluiremos un nuevo disco y
configuraremos LVM para que /var se monte en el nuevo VL que crearemos para él.

## Memoria

### Introducción y conceptos

Lo primero que se debe hacer es crear una máquina virtual de AlmaLinux con su configuración por defecto.
En este caso, se le asignan 10 GB de disco a la máquina y se procede con la instalación automática, sin tocar ningún tema de almacenamiento. Eso sí, creando un root con su contraseña y un usuario. Una vez completada la instalación verificamos con el comando `lsblsk` que todo está correcto. La instalación ha creado un disco sda con 3 particiones, la primera (de 1MB) para el arranque de la BIOS, la segunda para /boot y la tercera es un PV de LVM con espacio para root y swap.
