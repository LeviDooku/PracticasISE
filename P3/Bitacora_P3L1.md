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

Lo primero es configurar Zabbix en Debian, para ello, se consulta en la web que versión interesa (la última para Debian13) y se descarga usando `wget`. Además, en la página oficial proporciona informacińo exacta sobre lo que se está instalando y los comandos:

```
# Repositorio de Zabbix

wget https://repo.zabbix.com/zabbix/7.4/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.4+debian13_all.deb
dpkg -i zabbix-release_latest_7.4+debian13_all.deb
apt update

# Servidor, frontend y agente

apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```

Ahora, se indica que el usuario cree una base de datos inicial. Antes hay que comprobar y tener configurado MariaDB (no se profundiza en esto ya que se ha tratado en la práctica anterior).

```sql
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'password';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
quit;
```

Lo próximo que dice en la web es que se debe importar el esquema inicial y datos.

```
zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```

Ahora se desactiva el log_bin_trust_function_creators

```sql
set global log_bin_trust_function_creators = 0;
```

Ahora, se configura una contraseña para la base de datos, editando la línea `DBPassword` del archivo `/etc/zabbix/zabbix_server.config`

Para terminar, se activa el servicio tanto de Zabbix como de Apache:

```
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```

Finalmente, desde un navegador, se comprueba que todo es correcto haciendo `http://IP/zabbix`

![zabbix](../img/P3L1/P3L1_zabbix.png)
