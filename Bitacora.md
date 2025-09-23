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

El término RAID, cuyas siglas significan "redundant array of independent disks" es una tecnología de virtualización del almacenamiento, en el que varios dispositivos de almacenamiento físico se "virtualizan" en una unidad lógica, con el fin de crear redundancia de datos. El RAID1 consiste en copiar los datos exactamente iguales en dos o más discos.

La gestión con LVM (logical volume manager), se refiere a un software que añade una capa de abstracción entre los discos físicos y el sistema de archivos.
