# Creación de un Servicio VOIP
### Curso 4º GITT Asignatura Laboratorio Telemática

En esta primera práctica de la asignatura Laboratorio de Telemática se nos pidió realizar el despliegue de una red VoIP accesible desde Internet basándonos en DDNS

## Capacidades adquiridas y/o reforzadas

- Capacidad de configuración y puesta en marcha operativa de encaminadores y otros
elementos de interconexión.
- Capacidad para diseñar, desplegar y mantener las redes empresariales, tanto a nivel de
intranet/extranet como de su conexión a Internet.
- Capacidad para instalar, configurar y mantener los servicios más importantes de
Internet, incluyendo servicios web, de correo electrónico, noticias, mensajería y otros.
- Ser capaz de seleccionar, diseñar, desplegar, integrar y gestionar redes e
infraestructuras de comunicaciones en una organización.

## Objetivos

- Despliegue real de un servicio de PBX virtual con acceso público desde Internet mediante DDNS.

## Servicios ofrecidos

- Servidor FreePBX (softswitch) sobre una máquina virtual en Virtual Box. Dar de alta tres líneas para tres clientes de VoIP.
- Realización de llamadas VoIP con cada softphone a través de Internet y DDNS.
- Dominio DDNS personalizado.
 
## Topología implementada
![Imagen de la topología](/topologia.png)

## Software utilizado

Para la realización de esta práctica se utilizó:
- VirtualBox para albergar el servidor FreePBX.
- FreePBX.
- Zoiper.
- Wireshark.
- PuTTY.
- SNGrep.

## Hardware utilizado

- Host anfitrión.
- Router FTTH.
- Dispositivos clientes con el softphone instalado.

## Instalación y Configuración FreePBX

### Especificaciones Máquina Virtual

1. Configuraremos los recursos que le daremos a la MV, en este caso será necesario reservar por lo menos 3 GB de memoria RAM y 20 GB de disdo duro para que no de ningún fallo.
   
![Configuracion VirtualBox 1](/VB1.png)

2. Configuración del adaptador de red para poder realizar la conexión a Internet.
   
![Configuración VirtualBox 2](/VB2.png)

### Especificaciones FreePBX

Una vez que hemos realizado la instalación de la máquina virtual, se le ha asignado una contraseña y reiniciado la máquina **(¡IMPORTANTE CUANDO SE REALICE EL REINICIO DESMONTAR LA UNIDAD ÓPTICA PARA QUE NO DE FALLOS!)** podremos configurar el archivo de configuración de la interfaz Eth0. Para ello accedemos al documento:

~~~
sudo nano /etc/sysconfig/network-scripst/ifcfg-eth0
~~~

Donde realizaremos la siguiente configuración:
1. Estableceremos el **BOOTPROTO** de *DHCP* a *Static* para que la IP asignada se mantenga.
2. Añadiremos los apartados:
    ~~~
    IPADDR="<IP LIBRE DE TU ROUTER>"
    GATEWAY=<LA GATEWAY DE TU ORDENADOR>
    ~~~

Tras haber realizado la configuración mostrada reiniciaremos la red con:

~~~
sudo /etc/init.d/network restart
~~~

### FreePBX Web Interface
Tras haber reiniciado la red con el comando anterior **Y EL ROUTER** para aplicar la configuración que hemos realizado podremos acceder a la Web Interface de FreePBX, accediendo a través de la dirección IP que hemos establecido en nuestro documento anterior. Tras realizar las configuraciones iniciales podremos configurar las extensiones que se nos pide.

### Configuración de las extensiones

Añadiremos una extensión PjSIP con los siguientes parámetros:

- **Extensión del Usuario** que servirá para marcar en el softphone.
- **Nombre a mostrar** nombre de usuario que aparecerá en la llamada.
- **Secreto** contraseña para realizar la conexión.

![Configuración extensión](extension.png)

### Activación del servicio UDP y TCP

En el apartado de configuración SIP será necesario habilitar manualmente el servicio TCP, ya que de forma nativa únicamet
