# Pasos para iniciar en la ciberseguridad

## Instalaciones

- Instalar VirtualBox
- Instalar Kali Linux
- Actualizar paquetes de Kali Linux
  `sudo apt-get update`
  `sudo apt-get upgrade`
- Intalar terminator (terminal mejorada)
  `sudo apt-get install terminator`

## Comandos basicos de linux

`ls` (lista los archivos y carpetas)
`cd` (cambia de directorio)
`cd ..` (cambia al directorio anterior)

## Comandos para la red

- `sudo` (permite ejecutar comandos como administrador)
- `ifconfig` (muestra la configuracion de la red)
  - wlan0: interfaz de red inalambrica
  - inet: ip de la red
  - ether: direccion mac
- `ifconfig wlan0 down` (desactiva la interfaz de red necesario para cambiar el modo de la tarjeta)
- `ifconfig wlan0 hw ether 00:11:22:33:44:55` (cambia la direccion mac)
- `ifconfig wlan0 up` (activa la interfaz de red)

  ### Modos inalambricos

- `iwconfig` (muestra los modos de la tarjeta)
  - Modo monitor: permite capturar paquetes de red
  - Modo managed: permite conectarse a una red solo conecta adaptador con el router
- `ifconfig wlan0 down`
- `airmon-ng check kill` (mata los procesos que esten usando la tarjeta para mejorar el rendimiento de receptor)
- `iwconfig wlan0 mode monitor`
- `ifconfig wlan0 up`

  ### aircrack-ng

  Muestra informacion de la redes inalambricas a su alrededor

- `sudo apt-get install aircrack-ng` (instala aircrack-ng si no estas en Kali Linux)
- `airodump-ng wlan0` (muestra las redes inalambricas a su alrededor)

  - BSSID: direccion mac del router
  - ESSID: nombre de la red
  - CH: canal de la red
  - ENC: tipo de encriptacion
  - PWR: potencia de la señal
  - Beacons: numero de paquetes que se han enviado
  - IV: numero de paquetes que se han capturado

  ## Bandas wifi 2.4Ghz y 5Ghz

  Determina los canales que podemos usar para evitar interferencias con otras redes inalambricas depende de los adaptadores que tengamos para poder capturar paquetes

- bandas comunes

  - a: 5.15 - 5.25 GHz
  - b - g: 2.4 - 2.4835 GHz
  - n: 2.4 - 2.4835 GHz y 5.15 - 5.25 GHz
  - ac: 5.15 - 5.25 GHz

- `airodump-ng --band a wlan0` (muestra las redes inalambricas a su alrededor en la banda a)
- `airodump-ng --band abg wlan0` (muestra las redes inalambricas a su alrededor en la banda abg)
