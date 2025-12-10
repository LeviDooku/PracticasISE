# Bitacora P3L1

## Enunciado

Realice una instalación de Zabbix 7.4 en su servidor con Debian 13 y configure para que
se monitorice a él mismo y para que monitorice a la máquina con Alma Linux.

Puede configurar varios parámetros para monitorizar, uso de CPU, memoria, etc. pero
debe configurar de manera obligatoria, como mínimo, la monitorización de los servicios
SSH y HTTP.

Es recomendable, para reforzar el aprendizaje, que documente el proceso de instalación
y configuración indicando las referencias que ha utilizado así como los problemas que ha
encontrado.

## Introducción y conceptos

Lo que se pretende en este ejercicio es configurar una máquina Debian13 como "servidor de monitorización" instalando Zabbix.

Zabbix es un programa de monitorización de código abierto, usada para supervisar el estado de la red, los servidores, las máquinas virtuales y las aplicaciones en tiempo real.

## Diseño

Lo que se pide es que Debian se monitorice a sí mismo y además a la máquina de Almalinux. Se configurará primero los parámetros obligatorios y después algunos de los opcionales. 

Se trabaja con dos máquinas virtuales con los SO's mencionados anteriormente.

## Implementación del sistema