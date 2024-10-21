
# Proyecto DNS - sistema.test

## Descripción General

Este proyecto implementa un sistema **DNS** con una zona directa e inversa utilizando servidores **BIND** en un entorno virtualizado con **Vagrant**. Se configura un servidor **DNS maestro (tierra)** y un **DNS esclavo (venus)**. La zona DNS incluye alias y registros adicionales, y las transferencias de zona entre los servidores se configuran y verifican.

El objetivo es desplegar un entorno DNS funcional, realizar configuraciones avanzadas como la transferencia de zonas entre servidores y comprobar que todo funcione correctamente utilizando herramientas como `dig` o `nslookup`.

## Estructura del Repositorio

Este repositorio incluye los siguientes archivos y carpetas clave:

- **`.gitignore`**: Excluye del control de versiones los directorios innecesarios como `.vagrant` (donde tenemos las máquinas creadas) y archivos de backup.
- **`Vagrantfile`**: Define la configuración de las máquinas virtuales que simulan el servidor dns y el esclavo.
- **`README.md`**: Este archivo, que explica el proyecto y los pasos para ejecutarlo.
- **`LICENSE`**: Archivo con la licencia del proyecto.
- **Archivos de configuración de BIND**: Aquí se encuentran los archivos de configuración del servidor DNS maestro y esclavo.

## Requisitos Previos

Antes de comenzar, asegúrate de tener instalado:

- **Vagrant**
- **VirtualBox**
- **Git**

## Instalación

1. **Clona este repositorio** en tu máquina local:
   ```bash
   git clone https://github.com/antoniohr4/dns-practice.git
   cd dns-practice
   ```

2. **Configura y levanta las máquinas virtuales**:
   ```bash
   vagrant up
   ```

   Esto levantará dos máquinas virtuales, una con el rol de **servidor maestro** y otra con el rol de **servidor esclavo**.

## Configuración DNS

### 1. Configuración de la Red

Las máquinas están configuradas en la red `192.168.57.0/24`. Los nombres de los equipos y sus direcciones IP son las siguientes:

| Equipo                | FQDN                   | IP             |
|-----------------------|------------------------|----------------|
| Debian Maestro (tierra) | tierra.sistema.test     | 192.168.57.103 |
| Debian Esclavo (venus) | venus.sistema.test      | 192.168.57.102 |

### 2. Configuración de BIND

- El **servidor maestro** tiene autoridad sobre las zonas directa e inversa de `sistema.test` y su subred correspondiente.
- El **servidor esclavo** realiza transferencias de zona desde el maestro.
- Se han configurado alias para los nombres `ns1.sistema.test` y `ns2.sistema.test` que apuntan a `tierra.sistema.test` y `venus.sistema.test`, respectivamente.

### 3. Opciones de Seguridad y Forwarding

- El servidor DNS solo escucha en **IPv4** (`listen-on { any; };` y `listen-on-v6 { none; };`).
- La validación **DNSSEC** está activada con la opción `dnssec-validation yes`.
- Se permiten consultas recursivas solo desde las redes **127.0.0.0/8** y **192.168.57.0/24**.
- Las consultas no autorizadas se reenvían al servidor **OpenDNS** (`208.67.222.222`).

### 4. Transferencia de Zonas

La transferencia de zonas está configurada para permitir que el servidor esclavo (venus.sistema.test) se sincronice con el maestro (tierra.sistema.test). Los registros AXFR se pueden consultar para verificar las transferencias.

### 5. Comprobación del DNS

Utiliza las siguientes herramientas para verificar el correcto funcionamiento:

- **Consulta de registros A**:
   ```bash
   dig @192.168.57.103 tierra.sistema.test A
   ```

- **Consulta inversa** (PTR):
   ```bash
   dig @192.168.57.103 -x 192.168.57.103
   ```

- **Alias `ns1.sistema.test`**:
   ```bash
   dig @192.168.57.103 ns1.sistema.test
   ```

- **Transferencia de zona**:
   ```bash
   dig @192.168.57.103 sistema.test AXFR
   ```

### Capturas

- **Transferencia de Zona**: Captura de la ejecución del comando `dig sistema.test AXFR` mostrando la transferencia de zona entre el servidor maestro y esclavo.
- **Consulta de registros**: Resultados de la consulta `nslookup` para `ns1.sistema.test` y `ns2.sistema.test`.

## Decisiones en GitHub

- **Commits graduales**: Los cambios en el repositorio se realizan de manera gradual, subiendo las configuraciones paso a paso para mostrar el progreso y facilitar la revisión. Esto es importante para que el trabajo sea transparente y se vea el proceso de desarrollo de la práctica.
- **Uso de `.gitignore`**: Se incluye un archivo `.gitignore` para evitar que directorios como `.vagrant` (generado por Vagrant) y archivos de backup se suban al repositorio. Estos archivos no son relevantes para el control de versiones, ya que son generados automáticamente o específicos de la máquina local.
- **Vagrant y Virtualización**: Se ha optado por usar Vagrant para automatizar la creación de las máquinas virtuales. Esto permite replicar el entorno fácilmente y asegura que el mismo entorno se ejecute en cualquier máquina donde se clonen los archivos del repositorio.
