# Bitacora P1 - L1

## Enunciado: 

Usted está trabajando en una empresa proveedora de servicios y recibe la solicitud de
un cliente que sea tener un servidor para la implantación de un comercio electrónico
mediante un CMS.  

Sin tener más detalles por parte del cliente, le pregunta a un compañero qué configuración
se suele aplicar en estos casos. Este le remite a su jefa de Dpto. que le recomienda la
configuración de un RAID1 gestionado con LVM, cifrando toda la información para cumplir con la legislación vigente. También le recomienda crear al menos 3 VL (hogar, raiz y swap) y una partición para el arranque.  

Nota: El usuario que crearemos debe ser su primer nombre de pila acompañado de las
iniciales de sus apellidos en mayúscula. Por ejemplo, Jose Manuel Fernan Pese ->JoseFP  

## Memoria

### Introducción y conceptos

Lo primero que se debe hacer al realizar este ejercicio es leer y entender el enunciado propuesto para crear un diseño adecuado para el sistema que se implementará.

Los conceptos a tener en cuenta son la recomendación de implementar un RAID1 gestionado con LVM, cifrando la información y creando al menos 3 volúmenes lógicos (LV) y una partición para el arranque.

El término RAID, cuyas siglas significan "redundant array of independent disks" es una tecnología de virtualización del almacenamiento, en el que varios dispositivos de almacenamiento físico se "virtualizan" en una unidad lógica, con el fin de crear redundancia de datos. El RAID1 consiste en copiar los datos exactamente iguales en dos o más discos. Existen dos formas de implementar la tecnología RAID: software y hardware. La primera (la que se usa en este caso), la gestiona el SO. Es una solución barata y flexible, aunque añade carga a la CPU. La implementación por hardware necesita de una tarjeta RAID, y el SO ve un único dispositivo lógico.

La gestión con LVM se refiere a Logical Volume Manager, que añade una capa de abstracción sobre el RAID que permite crear y redimensionar volúmenes (LV) con flexibilidad. Además, es una herramienta muy útil para crear snapshots. En este caso, el LVM irá cifrado. 

### Diseño

Teniendo en cuenta los conceptos anteriores se puede proponer un diseño para el sistema con las siguientes características:  

- Se tendrán dos discos físicos (sda y sdb) sobre el que se montará el RAID1
- Cada disco se particionará en tres:
	- 1a: para el sector de arranque (GRUB) de 1MB
	- 2a: para el /boot, de aproximadamente 400MB. Se elige este tamaño porque, dependiendo de los módulos, una imagen del kernel puede llegar hasta los 200MB (de forma orientativa), para dar pie a actualizaciones, se escoge darle como mínimo el doble del máximo.
	- 3a: el resto del almacenamiento (unos 9.5GB) se usará para implementar el resto del sistema, que se gestionará con un LVM, con los 3 LV /, /home y /swap. Además, como se indica en el enunciado, el LVM irá cifrado.  

De forma esquemática, el diseño queda de esta forma:  

![DiseñoP1L1](../img/P1L1.png)