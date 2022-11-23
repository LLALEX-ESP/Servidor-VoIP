# Servidor VoIP ASTERISK Ubuntu Server 22.04 LTS Raspberry Pi

![IMG](https://www.asterisk.org/wp-content/uploads/asterisk-logo-fb-share.png)

## Índice

* Objetivos [📌](#objetivos)
* Hardware [📌](#hardware)
* Red [📌](#red)
* Instalación Ubuntu Server 22.04 LTS en Raspberry-Pi [📌](#instalación-ubuntu-server-22.04-lts-en-raspberry-pi)
* Configuración Ubuntu Server [📌](#configuración-ubuntu-server)
* Instalación de ASTERISK [📌](#instalación-de-asterisk)
* Configuración de ASTERISK [📌](#configuración-de-asterisk)
* Descarga [📌](#descarga)

## Objetivos

En este proyecto se tiene como objetivo 3 funciones:

⏩  **1º** Creación de una pequeña red privada: Simularemos con un router y 4 dispositivos, una pequeña red empresarial o privada para la instalación de un Ubuntu Server en una Raspberry Pi y la instalación de un servicio de VoIP, con el software Asterisk.

⏩  **2º** Instalación de un Ubuntu Server 22.04 LTS en una Raspberry-Pi: Con esta simularemos el servidor y es en la que instalaremos el servicio de VoIP.

⏩  **3º** Instalación de un servicio VoIP: Voz sobre protocolo de internet o Voz por protocolo de internet, también llamado voz sobre IP, voz IP, vozIP o VoIP, es un conjunto de recursos que hacen posible que la señal de voz viaje a través de Internet empleando el protocolo IP. Para su instalación y configuración utilizaremos el software de [Asterisk](https://www.asterisk.org/), un programa de software libre que proporciona funcionalidades de una central telefónica.

## Hardware

El hardware que se utilizara para este proyecto es el siguiente:


* **Router**: [Xiaomi Mi Router 4A Gigabit Edition](https://www.idealo.es/precios/6994109/xiaomi-mi-router-4a-gigabit.html)
  * *Procesador*: MediaTek MT621A a 800 MHz
  * *Memoria Interna*: 16MB
  * *Memoria RAM*: 128MB
  * *Antenas con tecnología [LNA](https://www.digikey.com/es/blog/why-a-good-lna-is-key-to-a-viable-antenna-front-end)* (Low Noise Amplifier)
  * *Ancho de Banda*:
    * 2.4G: 300M
    * 5G: 867M
  * *Interfaz*:
    * 2 x LAN 10/100M
    * 1 x Gigabit Ethernet 10/100/1000M

* **Raspberry Pi**: [Raspberry Pi 4 Modelo B](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/?variant=raspberry-pi-4-model-b-8gb)
  * *Procesador*: Broadcom BCM2711, quad-core Cortex-A72 (ARM v8) 64-bit SoC @ 1.5GHz.
  * *RAM*: 8GB LPDDR4 SDRAM
  * *Conectividad*: WiFi Dual Band 2.4 GHz y 5.0 GHz IEEE 802.11b/g/n/ac, Bluetooth 5.0, BLE
  * *Gigabit Ethernet* 2 × USB 3.0 ports 2 × USB 2.0 ports
  * *GPIO*: Standard 40-pin GPIO header (retrocompatible con las anteriores Raspberry Pi de 40 puertos)

* **Dos dispositivos móviles con conectividad WiFi**

En los dispositivos móviles y en el router podemos utilizar el que nosotros queramos, no tienen que ser estrictamente estos.

## Red

**Configuración de la red**

![LAN](https://user-images.githubusercontent.com/67869168/202767531-d10c0b4d-e803-4cda-ad2e-abc6502b72fe.png)


## Instalación Ubuntu Server 22.04 LTS en Raspberry Pi

La instalación de Ubuntu Server 22.04 LTS en Raspberry Pi la realizaremos desde 0, estos son los pasos a seguir:


* **Descarga de Raspberry Pi Imager** La instalación del S.O lo podemos hacer directamente descargadnos una ISO específica para Raspberry Pi en la página oficial de Ubuntu, pero esta puede llegar a dar problemas, por lo que descargaremos la herramienta de instalación de imágenes de Raspberry Pi en la que podremos instalar el S.O y preconfigurar algunos ajustes. [Raspberry Pi Imager](https://www.raspberrypi.com/software)

![IMG_1](https://user-images.githubusercontent.com/67869168/202757952-10318814-88cd-4af5-a384-55e709e240a7.png)

* **Seleccion del S.O** Una vez instalada la herramienta el primer paso será seleccionar el S.O que instalaremos, para ello seguiremos la siguiente ruta:
```CHOOSE OS``` -> ```Other general-purpose OS``` -> ```Ubuntu``` -> ```Ubuntu Server 22.10 (64-bit)```

![IMG_2](https://user-images.githubusercontent.com/67869168/202758501-46fb4c46-41d2-453c-b8e6-01741294e146.png)
![IMG_3](https://user-images.githubusercontent.com/67869168/202758533-407baff1-0518-4715-833e-a8ba41c31e24.png)

* **Preconfiguracion** Al utilizar la herramienta de Raspberry Pi Imager nos da la configuración de realizar algunas configuraciones en el S.O antes de iniciarlo.

 `Hostname`
 `Enable SSh`
 
 ![IMG_4](https://user-images.githubusercontent.com/67869168/202759463-3e855a70-f8b0-40f0-b8e0-59afeb29761d.png)
 
 `Set username and password`
 
 ![IMG_5](https://user-images.githubusercontent.com/67869168/202759547-34d5b7c5-8809-48ea-b87b-d290ee5abdb9.png)
 
 `Set locale settings`
 
 ![IMG_6](https://user-images.githubusercontent.com/67869168/202759664-2d30389c-0db0-4ae2-b5d2-67968403c1d4.png)
 
 * **Instalacion del S.O** Después de preconfigurar el S.O podemos empezar a instalarlo clicando sobre `WRITE`, el programa comenzara a instalar el S.O, este proceso durara aproximadamente 10 Minutos.
 
 ![IMG_7](https://user-images.githubusercontent.com/67869168/202759916-084413c9-cee7-4b48-86b2-41a3b8e0d41e.png)

## Configuración Ubuntu Server

* **IP Privada Fija** 
  * El archivo que tendremos que editar será `50-installer-config.yaml`, la ruta del archivo es la siguiente: 

    `sudo nano /etc/netplan/00-installer-config.yaml`
    
  * El archivo de fábrica tiene el siguiente formato:
    ```
    network:
        ethernets:
          enp0s3:
            dhcp4: true
        version: 2
    ```
   * El archivo modificado para obtener una IP Estática tiene el siguiente formato:
     ```
     network:
         ethernets :
           ######:             #Tarjeta de Red.
             dhcp4: no
             addresses: [###.###.###.###/##]
             gateway4: ###.###.###.###
             nameservers:
               addresses: [8.8.8.8, 1.1.1.1]
         version: 2
      ```
    * Después de editar el fichero tendremos que aplicar los cambios, para ello emplearemos el siguiente comando:
      ```
      sudo netplan apply
      ```

## Instalación de ASTERISK

### ¿Qué es Asterisk?

* Es un software libre y de código abierto
* Servidor de comunicaciones
  * Sistemas IP PBX
  * Puerta de enlace VoIP
  * Servidores de conferencia
* Principal ventaja
  * Incluye todas las funcionalidades de las más costosas alternativas de código cerrado.
  * Correo de voz,llamada en conferencia, respuesta interactiva de voz (IVR) y distribución automática de llamadas.

### Definiciones

* **PBX (Private Branch Exchange)** Permite la interconexión de teléfonos para realizar llamadas entre si e incluso acceder a comunicaciones fuera de la misma conectando a un proveedor de VolP.
* **Softphone** Aplicación que implemente un terminal telefónico de uno o varios protocolos de VolP en un PC.
* **VoIP (Voice over IP)** Es una tecnología que proporciona la comunicación de voz en forma digital, sobre Protocolo de Internet (IP), en lugar de enviarla en forma analógica, como utiliza la telefonía tradicional.

Instalación de ASTERISK

```
sudo apt-get install asterisk -y
```

Ver versión instalada de ASTERISK

```
asterisk -V
```

```
---> Asterisk 18.14.0~dfsg+~cs6.10.40431411-2
```

Localizar paquetes necesarios para ASTERISK

```
apt search ASTERISK
```
Paquetes localizados que son necesarios

```
asterisk-core-sounds-es
asterisk-core-sounds-es-g722
asterisk-core-sounds-es-gsm
asterisk-core-sounds-es-wav
asterisk-prompt-es
```

Instalación de paquetes

```
sudo apt-get install asterisk-core-sounds-es asterisk-core-sounds-es-g722 asterisk-core-sounds-es-gsm asterisk-core-sounds-es-wav asterisk-prompt-es -y
```

Comprobamos los paquetes instalados 

```
dpkg -l asterisk*
```

```
||/ Nombre                                        Versión                           Arquitectura Descripción
+++-=============================================-=================================-============-=========================================
ii  asterisk                                      1:18.10.0~dfsg+~cs6.10.40431411-2 amd64        Open Source Private Branch Exchange (PBX)
un  asterisk-abi-1fb7f5c06d7a2052e38d021b3d8ca151 <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
ii  asterisk-config                               1:18.10.0~dfsg+~cs6.10.40431411-2 all          Configuration files for Asterisk
un  asterisk-config-custom                        <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
ii  asterisk-core-sounds-en                       1.6.1-1                           all          asterisk PBX sound files - US English
un  asterisk-core-sounds-en-g722                  <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
ii  asterisk-core-sounds-en-gsm                   1.6.1-1                           all          asterisk PBX sound files - en-us/gsm
un  asterisk-core-sounds-en-wav                   <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
ii  asterisk-core-sounds-es                       1.6.1-1                           all          asterisk PBX sound files - Spanish
ii  asterisk-core-sounds-es-g722                  1.6.1-1                           all          asterisk PBX sound files - es-mx/g722
ii  asterisk-core-sounds-es-gsm                   1.6.1-1                           all          asterisk PBX sound files - es-mx/gsm
ii  asterisk-core-sounds-es-wav                   1.6.1-1                           all          asterisk PBX sound files - es-mx/wav
un  asterisk-dahdi                                <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
un  asterisk-dev                                  <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
un  asterisk-doc                                  <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
ii  asterisk-modules                              1:18.10.0~dfsg+~cs6.10.40431411-2 amd64        loadable modules for the Asterisk PBX
ii  asterisk-moh-opsound-gsm                      2.03-1.1                          all          asterisk extra sound files - English/gsm
un  asterisk-ooh323                               <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
un  asterisk-opus                                 <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
un  asterisk-prompt-en                            <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
un  asterisk-prompt-en-us                         <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
un  asterisk-prompt-es                            <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
un  asterisk-prompt-es-mx                         <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
un  asterisk-sounds-main                          <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
un  asterisk-voicemail                            <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
un  asterisk-voicemail-imapstorage                <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
un  asterisk-voicemail-odbcstorage                <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
un  asterisk-vpb                                  <ninguna>                         <ninguna>    (no hay ninguna descripción disponible)
```

Comprobamos el estado de los servicios

```
systemctl status asterisk.service
```

Nos tiene que devolver el siguiente error

```
nov 18 19:11:38 alejandroalsa asterisk[15885]: radcli: rc_read_config: rc_read_config: can't open /etc/radiusclient-ng/radiusclient.conf: No such file or directory
```
Esto sucede porque ASTERISK por defecto tiene configurado que el archivo 'radiusclient.conf' se encuentra almacenado en el directorio '/etc/radiusclient-ng/' si hacemos una pequeña búsqueda podemos encontrar el archivo 'radiusclient.conf' en '/etc/radcli/' por lo que realizaremos algunos cambios:

Antes de hacer cualquier cambio realizaremos una copia de seguridad

```
sudo cp /etc/asterisk/cel.conf /etc/asterisk/cel.conf.copy
```

```
sudo cp /etc/asterisk/cdr.conf /etc/asterisk/cdr.conf.copy
```

Procedemos a editar el archivo donde está almacenando la ruta incorrecta

```
sudo nano /etc/asterisk/cel.conf
```

Nos dirigiremos al final del archivo a la zona de `[radius]` allí modificaremos el archivo

```
Antes -> ;radiuscfg => /usr/local/etc/radiusclient-ng/radiusclient.conf

Ahora -> radiuscfg => /etc/radcli/radiusclient.conf
```

Editamos el otro archivo 

```
sudo nano /etc/asterisk/cdr.conf
```

Nos dirigiremos al final del archivo a la zona de `[radius]` (comentada) allí modificaremos el archivo

```
Antes -> ;[radius]
Antes -> ;radiuscfg => /usr/local/etc/radiusclient-ng/radiusclient.conf

Ahora -> [radius]
Ahora -> radiuscfg => /etc/radcli/radiusclient.conf
```

Reiniciamos el servicio y ya no nos saldrá el error

```
sudo systemctl restart asterisk.service
```

Si listamos el directorio de `/etc/asterisk/` nos daremos cuenta de que saldrán un montón archivos, nuestros por ahora modificaremos dos: `extensions.conf` y `sip.conf`

* **sip.conf** Permite definir los canales SIP (peers), tanto para llamadas entrantes como salientes.
* **extensions.conf** Es el que define el comportamiento que va a tener una llamada en nuestra centralita (qué reglas rigen su enrutamiento o qué aplicaciones van a ejecutar).

## Configuración de ASTERISK

Copia de Seguridad

```
sudo cp /etc/asterisk/sip.conf /etc/asterisk/sip.conf.copy
```

Si abrimos el fichero de `sip.conf` nos daremos cuenta de que es un archivo muy extenso, ya que incluye ejemplos preestablecidos y su documentación, por lo que utilizaremos el ditor *vi* para eliminar las lineas comentadas y lineas en blanco

```
sudo vi /etc/asterisk/sip.conf
```

```
:g/^\s*;/d

:g/^$/d

```

En `sip.conf` la primera sección es `[general]` y permite definir las opciones generales de cada canal y, en consecuencia, los parámetros generales de cada cliente.

El archivo se nos tiene que quedar de la siguiente forma:

```
[general]
context=public                  ; Default context for incoming calls. Defaults to 'default'
allowoverlap=no                 ; Disable overlap dialing support. (Default is yes)
udpbindaddr=0.0.0.0             ; IP address to bind UDP listen socket to (0.0.0.0 binds to all)
tcpenable=no                    ; Enable server for incoming TCP connections (default is no)
tcpbindaddr=0.0.0.0             ; IP address for TCP server to bind to (0.0.0.0 binds to all interfaces)
transport=udp                   ; Set the default transports.  The order determines the primary default transport.
srvlookup=yes                   ; Enable DNS SRV lookups on outbound calls

[authentication]

[basic-options](!)                ; a template
        dtmfmode=rfc2833
        context=from-office
        type=friend

[natted-phone](!,basic-options)   ; another template inheriting basic-options
        directmedia=no
        host=dynamic

[public-phone](!,basic-options)   ; another template inheriting basic-options
        directmedia=yes

[my-codecs](!)                    ; a template for my preferred codecs
        disallow=all
        allow=ilbc
        allow=g729
        allow=gsm
        allow=g723
        allow=ulaw

[ulaw-phone](!)                   ; and another one for ulaw-only
        disallow=all
        allow=ulaw
```

Despues en el apartado de `[general]` anadiremos las siguientes lineas

```
qualify=yes                     ; Permite monitorear la conexción con los teléfonos VoIP.
language=es                     ; Idioma por defecto para todos los usuarios.
disallow=all                    ; Desactivar todos los codificadores.
allow=ulaw                      ; Permitir codificadores en orden de preferencia.
```

Por ultimo y finalizar la configuracion basica anadiremos a un usuario

```
[usuario](!)                     ; Plantilla con la configuración que vamos a utilizar
type=friend                      ; El usuario con esta extensión podra enviar y recibir llanadas
host=dynamic                     ; Cualquier equipo con cualquier IP selpodrá registrar como cliente
context=alejandroalsa            ; Contexto predefinido (ver ->extensions.conf)
```

Despues de ello crearemos las extensiones
```
; Extension 01
[ext_01](usuario)
username=usu_ext_01
secret=usu_ext_01
port=5061
```

```
; Extension 02
[ext_02](usuario)
username=usu_ext_02
secret=usu_ext_02
port=5061
```

Asterisk funcioan atraves de usuario y extensiones, es decir, nosotros creamos un usuario llamemoslo `X` y lo agregamos a la extension `01`, despues creamos otros usuatio llamemoslo `Y` y lo agregamos a la extension `02`, para que el usuario `X` llame al usuario `Y` tendra que marcar su numero de extencion es decir `02`.

Al terminar de editar los fichero es importante comprobar que los ficheros editatos pertenezcan al grupo y usuario `asterisk`

```
sudo chown asteris:asterisk sip.cof
```

Depues de terminar reiniciaremos el servicio de Asterisk
```
sudo systemctl restart asterisk.service
```

Otros de los pasoso fundamentales es la consola que nos porporciona Asterisk, en ella podemos ver en tiempo real todos los procesos que se lleva a cabo en el software y de tener una vista mas amplia de todos los ajustes, modificaciones, usuarios, extensiones, y funciones que agregamos al software, para entrar en la consola introduciremos el siguiente comando:
```
sudo asterisk -rvvvv
```
Antes de comenzar con los comando que hay asiciados es importante activar el modulo de `chan_sip.so`, para poder utlizar los comandos

```
module load chan_sip.so
```

Una vez echo ya podremos utilizarlos.

* **sip show peers** Muestra los peers, los clientes registrados en la centralita (las extensiones). Su estado, IPs etc.
* **sip show users** Muestra los usuarios.
* **sip reload** Recarga la configuración de SIP.
* **dialplan reload** Recarga el plan de marcación.
* **core show channels** Muestra los canales de todo tipo.
* **sip show channels** Muestra información del canal SIP.

En este caso listamos las extensiones y los usuarios que hay asociados a ellas

```
sip show peers
sip show users
```

```
Name/username             Host                                    Dyn Forcerport Comedia    ACL Port     Status      Description                      
ext_01/usu_ext_01         192.168.1.100                            D  Auto (No)  No             38089    OK (2 ms)                                    
ext_02/usu_ext_02         192.168.1.141                            D  Auto (No)  No             38699    OK (3 ms)                                    
2 sip peers [Monitored: 2 online, 0 offline Unmonitored: 0 online, 0 offline]
```
Por ultimo y para finalizar la configuracion tendremos que agregar los usuario a las extensiones en el archivo `/etc/asterisk/extensions.conf`, no sin antes realizar una copia de seguridad.

* **Controla el dialplan de la centralita** Define cómo se comportarán las llamadas entrantes y salientes en el sistema.
* Este fichero está compuesto por **contextos**, **extensiones** y **prioridades**.
* **Contextos** 
  * Cada una de las secciones en las que está dividido el dialplan.
  * Existen 3 contextos reservados: general, global y default.
  * Cuando una extensión, de las que hay definidas sip.conf, tiene definido un contexto (ej. context=alejandroalsa) y efectúa una llamada, empezarán a ejecutarse las líneas de código en ese contexto.

```
sudo cp extensions.conf extensions.conf.copy
```

En el fichero `extensions.conf` agregaremos el ambito `[alejandroalsa]` y las extensiones a los usuarios.

```
[alejandroalsa]
exten => 01,1,Dial(SIP/ext_01)
exten => 02,1,Dial(SIP/ext_02)
```

En este punto la configuracion estara finalizada.

### Instalacion Zoiper

Para poder empezar a utilizar el servicio necesitamos instalar en los dispositivos que utlizaremos como terminal telefonica el programa de Zoiper que es software multiplataforma (funciona con Windows, Linux, MAC, iPod Touch, iPad, iPhone, tablets y Android), diseñado para trabajar con sus sistemas de comunicación IP basado en el protocolo SIP.

Para su instalacion en PC nos tendremos que dirigir a su pagina web oficial y descargarnos el programa. [DESCARGAR](https://www.zoiper.com/en/voip-softphone/download/current)

### Configuracion de Zoiper

Cuando finalizemos la instalacion nos encontraremos con la siguiente pantalla:

![IMG_10](https://user-images.githubusercontent.com/67869168/202850903-a868e230-fccf-41ac-80e9-c87d3ab45764.png)

Seleccionaremos la opcion de `Continue as a Free user`.

En la segunda pantall tendremos que introducir el usuario, direccion ip del servidor y contrasena

![IMG_11](https://user-images.githubusercontent.com/67869168/202850974-9dc79d43-9f1a-403c-8f0b-7c460fc8d85e.png)

```
usuario@direccionip

password
```

En la siguiente pantalla seleccionaremos la misma IP

![IMG_12](https://user-images.githubusercontent.com/67869168/202851000-74155b69-c537-4be9-b3b8-b7c02b13eb91.png)

Por ultimo seleccionamore la opcion de `Authentication and Outbound proxy` donde introduciremos nuestro usuario

![IMG_13](https://user-images.githubusercontent.com/67869168/202851033-f5bd21b3-86ee-4ef4-8c1c-e79000e68ac3.png)

En la siguiente ventana conectara con el servidor donde tiene que salir las siguites opciones

![IMG_13](https://user-images.githubusercontent.com/67869168/202851071-7ec3a0ff-644e-4032-99e8-242f46297a4d.png)

* **SIP TLS** Esta es una opcion PRO
* **SIP TCP** No esta configurado
* **SIP UDP** Encontrado
* **IAX UDP** No esta configurado

Despues ya podremos empeazar a utlizar la herramienta y relizar la primera llamada

![IMG_14](https://user-images.githubusercontent.com/67869168/202851153-f508a93d-f735-4b15-a9ec-d274f6635be3.png)

## Descarga

```
git clone https://github.com/LLALEX-ESP/Servidor-VoIP.git
``` 
