# PiHole-UnboundDNS y WireGuard-VPN.
- `PiHole "Ad-Block"`
- `Unbound "DNS"`
- `WireGuard "VPN"`
  
Este proyecto esta diseñado con la finalidad de utilizarse en una red domestica, utilizando una Raspberry Pi B 8GB con dispositivo, donde contamos con la configuracion de un servicio de bloqueo de anuncios como PiHole, un servicio de transito de DNS como Unbound y un servicio de VPN como WireGuard, proteger las peticiones DNS de todos los dispositivos de la red y proporcionar una solución VPN para cuando cualquiera de estos dispositivos se encuentre fuera de la red y desee aprovechar las ventajas de seguridad (y velocidad) de la red de forma remota.

![image](https://github.com/TecnologyCASM/PiHoleUnbound/assets/107158068/559e0e0e-a068-4243-ae4f-910319001b79)

# Prerequisitos:
* `Raspberry Pi 4 Modelo B 2GB`. https://amzn.to/3K7diXR
* `64GB tarjeta MicroSD`. https://amzn.to/3ynPiNz
* `Lector de tarjetas SD USB`. https://amzn.to/3wGN8bs
  
Nota: Dicho esto, este proceso debería funcionar en cualquier Raspberry Pi.

# Instalacion de Sistema Operativo en Raspberry Pi:
1) Descarga la aplicacion "[RaspberryOS](https://www.raspberrypi.com/software/)" de la pagina oficial.
2) Conecta a la PC el lector SD con la memoria micro y segue los pasos como se muestra en la imagen mas abajo.
3) Elige el sistema operativo recomendado por raspberry.
![image](https://github.com/TecnologyCASM/PiHoleUnbound/assets/107158068/4173438b-eca6-497a-85d0-ec96bf698629)
![image](https://github.com/TecnologyCASM/PiHoleUnbound/assets/107158068/3a84ef2b-4204-4585-a62f-6c5adf6b9236)
4) Una vez instalado el sistema operativo en la raspberry, conecta esta a la red via cable y conecta un monitor para completar las configuraciones iniciales.
5) En este paso, debemos realizar los ajustes de `Network`, ya que estaremos brindando los servicios mencionados, debemos fijar un direccionamiento IP al equipo, donde estaremos ejecutando el siguiente comando, para que nos arroje un asistente:
```shell
sudo nmtui
```
![image](https://github.com/TecnologyCASM/PiHoleUnbound-WG/assets/107158068/35b590d2-8eab-44af-9d5b-ab48f9270ff5)

6) En este asistente, estaremos eligiendo la opcion `Modificar una conexion`, luego debemos elegir la terjeta de red que dice `Conexion Cableada 1` y presionamos editar:
![image](https://github.com/TecnologyCASM/PiHoleUnbound-WG/assets/107158068/cc2a65fd-be60-46c5-9aec-0b39bc97c184)
10) Alli aplicamos los ajuste de direccionamiento estatico que sean necesarios, como son:
  * Direcciones: En esta opcion debemos colocar la IP estatica que debera llevar la raspberry, el cual debe llevar el siguiente formato `192.168.1.10/24`.
  * Puerta de Enlace: En esta opcion debemos colocar la IP de nuestro router de internet, el cual se encuentra en el mismo segmento mensionado mas arriba, por ejemplo `192.168.1.1`.
  * Servidores de DNS: En esta opcion colocamos los DNS de nuestra eleccion, en mi caso coloco los de google `8.8.8.8` y `8.8.4.4`.
  * Busqueda de Dominio: Esto es cuando estamos trabajando con un dominio, por lo que es opcional.
7) Despues que la Raspberry Pi inicie el OS despues del reinicio, entonces procederemos con los ajustes de algunos parametros en esta, para ello debemos iniciar el siguiente asistente, con el comando:
```shell
sudo raspi-config
```
Este comando nos llevara a la siguente pantalla:
![image](https://github.com/TecnologyCASM/PiHoleUnbound-WG/assets/107158068/c138d6d4-2f87-4468-bd1f-2c13102bac31)

8) En este asistente, tendremos que realizar los siguientes ajustes:
  *  `1-System Options=>Boot/Auto Login=>Desktop Autologin`: Opcion utilizada para que la rasperrypi no solicite credenciales al iniciar sesion. (Solo Raspberry Pi Desktop).
  *  `2-Display Options=>VNC Resolutions`: Opcion utilizada para elegir la resolucion de la pantalla de conexion remota a la raspberry pi. (Solo Raspberry Pi Desktop). 
  *  `3-Interface Options=>VNC`: Opcion habilitada para conexion remota por GUI a la raspberry pi. (Solo Raspberry Pi Desktop).
  *  `5-Localisation Options`:
      - Locale: Configuracion del idioma. En mi caso es `es_DO.UTF-8`.
      - Timezona: Configuracion de zona horaria. En mi caso es `America/Santo_Domingo`.
      - Keyboard: Configuracion del Teclado. Solo elegir para autoreconocimiento.
      - WLAN Country: Configuradion de la ciudad para la red wireless. En mi caso `DO Dominican Republic`.
  *  `6-Advanced Options=>Expand FileSystem`: Opcion para expandir por completo el almaceniamiento de la SD de la raspberry.

 Nota: Para que estos ajustes se apliquen debemos presionar `Finish` y este solicitara un reinicio del equipo.
 
# Proceso Actualiacion OS, Instalacion de Docker y agregar usuario a grupo docker: 
1) Una vez la raspberry pi halla iniciado, procederemos a aplicar los siguientes comandos:
      - Actualiazar la lista de repositorios, Sistema Operativo, instalar dependencias, docker, agregar el usuario al grupo docker y reiniciar el equipo:
  ```shell
sudo apt update && sudo apt-get full-upgrade -y \
   apt-transport-https \
   ca-certificates \
   curl \
   gnupg2 \
   software-properties-common \
   vim \
   fail2ban \
   ntfs-3g &&
sudo curl -fsSL https://get.docker.com/ -o get-docker.sh && sudo sh get-docker.sh &&
sudo usermod -aG docker ${USER} && sudo rm -r get-docker.sh &&
sudo reboot
```
2) Una vez la raspberry pi halla iniciado y para validar que el servicio de docker esta instalado, procederemos a descargar un contenedor de prueba llamado `Helo-Wold`:
```shell
docker run hello-world
```
![image](https://github.com/TecnologyCASM/PiHoleUnbound-WG/assets/107158068/58f35f2b-9c35-4381-8186-8f37298e170a)

# Descarga de proyecto y modificacion del mismo:
1) Ejecutar el siguiente comando para solo descargar el proyecto `HomePiholeDNS-WG` y acceder a la carpeta del proyecto:
```shell
git clone https://github.com/TecnologyCASM/PiholeDNS-WG.git
cd PiholeDNS-WG
ls -la

```   
2) Editar los siguientes archivo a su gusto `docker-compose.yml` y `.env`:
```shell
sudo nano docker-compose.yml
```
```shell
sudo nano .env
```
7) Una vez editado los archivos mencionados mas arriba, solo resta ejecutar el sigueinte comando:
```shell
docker compose up -d && docker ps
```
# Contenedor PiHole:
1) Despues de subir los contenedores, debemos entrar a la web de Pihole para poder visualiar e iniciar las configuraciones de lugar http://X.X.X.X:port/admin/login.php

![image](https://github.com/TecnologyCASM/PiholeDNS-WG/assets/107158068/267d2e41-1c64-46f0-940f-315f796ead7c)

2) Despues de iniciar sesion, nos dirigimos al apartado `Adlist`:
![image](https://github.com/TecnologyCASM/PiholeDNS-WG/assets/107158068/489423e0-2434-436d-a21a-4ffabee21137)
en este apartado debemos agregar la siguiente url de IP que se encuentran en blacklist [PiholeB-locklist](https://github.com/Pyenb/Pi-hole-blocklist)

# Contenedor WireGuward:
1) Este es el segundo contenedor que trabaja el servicio de VPN:
![image](https://github.com/TecnologyCASM/PiholeDNS-WG/assets/107158068/a5541875-34bb-494e-bd0f-415491d2052c)

2) Una vez iniciado session, podemos agregar los perfiles de los clientes que necesitan conectarse via VPN:
![image](https://github.com/TecnologyCASM/PiholeDNS-WG/assets/107158068/4680cba7-5aed-48b0-8a31-b5fd039142fd)

MUCHAS GRACIAS POR UTILIZAR MI APORTE A LA COMUNIDAD...
