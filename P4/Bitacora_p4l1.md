# Bitacora P4L1

## Enunciado

Una vez que haya indagado sobre los benchmarks disponibles, seleccione como mínimo uno de ellos y proceda a ejecutarlos en Debian y Alma Linux. Comente las diferencias de los resultados de ejecución.

## Introducción y conceptos

Este ejercicio se propone en el contexto de Phoronix, la cual es una suite que permite ejecutar un conjunto de benchmarks. 

Un benchmark es un programa que sirve como prueba de rendimiento para una máquina, un componente, un servicio... En definitiva es una prueba usada para la comparación de rendimientos. Es esencial medir las necesidades y objetivos de lanzar un programa de benchmark en un ordenador, teniendo claras cuestiones como las métricas que se usarán, bajo que situación etc.

## Diseño

El ejercicio pide indagar sobre los benchmarks disponibles en Phoronix y seleccionar como mínimo dos, posteriormente ejecutarlos en las máquinas Debian y Almalinux.

Para ver los benchmarks disponibles se puede consultar openbenchmarking.org, también listandolos desde la terminal.

Los test que se eligen son fáciles y rápidos:

1. Cpu: para pruebas sintéticas de cpu
2. 7zip: para compresión y descompresión de archivos

## Implementación del sistema

El primer paso es instalarse la suit en ambos sistemas. Para ello, se clona el repositorio oficial y se ejecuta el script install-sh que incluye.

```
git clone git@github.com:phoronix-test-suite/phoronix-test-suite.git
chmod +x install-sh
sudo ./install-sh

phoronix-test-suite version # Comprobar que todo ok
```

Para mirar los test disponibles

```
phoronix-test-suite list-test
phoronix-test-suite list-recommended-tests # Recomendados para el SO 
```

![recomendados](../img/P4L1/P4L1_test.png)


