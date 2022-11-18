# Servidor VoIP ASTERISK Ubuntu Server 22.04 LTS Raspberry-Pi

![IMG](https://www.asterisk.org/wp-content/uploads/asterisk-logo-fb-share.png)

## Índice

* Objetivos [📌](#objetivos)
* Hardware [📌](#hardware)
* Red [📌](#red)
* Instalación Ubuntu Server 22.04 LTS en Raspberry-Pi [📌](#instalación-ubuntu-server-22.04-lts-en-raspberry-pi)
* Configuración Ubuntu Server [📌](#configuración-ubuntu-server)
* Instalación de ASTERISK [📌](#instalación-de-asterisk)
* Configuración de ASTERISK [📌](#configuracion-de-asterisk)
* Token [📌](#token)
* Descarga [📌](#descarga)

## Objetivos

En este proyecto se tiene como objetivo 3 funciones:

⏩  **1º** Creación de una pequeña red privada: Simularemos con un router y 4 dispositivos, una pequeña red empresarial o privada para la instalación de un Ubuntu Server en una Raspberry Pi y la instalación de un servicio de VoIP, con el software Asterisk.

⏩  **2º** Instalación de un Ubuntu Server 22.04 LTS en una Raspberry-Pi: Con esta simularemos el servidor y es en la que instalaremos el servicio de VoIP.

⏩  **3º** Instalación de un servicio VoIP: Voz sobre protocolo de internet o Voz por protocolo de internet, también llamado voz sobre IP, voz IP, vozIP o VoIP, es un conjunto de recursos que hacen posible que la señal de voz viaje a través de Internet empleando el protocolo IP. Para su instalación y configuración utilizaremos el software de [Asterisk](https://www.asterisk.org/), un programa de software libre que proporciona funcionalidades de una central telefónica. Configuraremos las siguientes funciones:

* **Configurar Servidor VoIP**
* **Reproducir un fichero de audio**
* **Grabar un audio para reproducirlo**


## Hardware

El hardware que se utilizara para este proyecto es el siguiente:


* **Router**: [Xiaomi Mi Router 4A Gigabit Edition](https://www.idealo.es/precios/6994109/xiaomi-mi-router-4a-gigabit.html)
  * *Procesador*: MediaTek MT621A a 800 MHz
  * *Memoria Interna*: 16MB
  * *Memoria RAM*: 128MB
  * *Antenas con tecnología LNA* (Low Noise Amplifier)
  * *Ancho de Banda*:
    * 2.4G: 300M
    * 5G: 867M
  * *Interfaz*:
    * 2 x LAN 10/100M
    * 1 x Gigabit Ethernet 10/100/1000M

* **Rasbery Pi**: [Raspberry Pi 4 Modelo B](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/?variant=raspberry-pi-4-model-b-8gb)
  * *Procesador*: Broadcom BCM2711, quad-core Cortex-A72 (ARM v8) 64-bit SoC @ 1.5GHz.
  * *RAM*: 8GB LPDDR4 SDRAM
  * *Conectividad*: WiFi Dual Band 2.4 GHz y 5.0 GHz IEEE 802.11b/g/n/ac, Bluetooth 5.0, BLE
  * *Gigabit Ethernet* 2 × USB 3.0 ports 2 × USB 2.0 ports
  * *GPIO*: Standard 40-pin GPIO header (retrocompatible con las anteriores Raspberry Pi de 40 puertos)

* **Dos dispositivos móviles con conectividad WiFi**

En los dispositivos móviles y en el router podemos utilizar el que nosotros queramos, no tienen que ser estrictamente estos.

## Red

## Instalación Ubuntu Server 22.04 LTS en Raspberry-Pi

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

## Configuración de ASTERISK

## Token

## Descarga

