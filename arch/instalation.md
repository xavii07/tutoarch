---
title: Instalación de Arch Linux
description: Comandos para instalar Arch Linux
keywords: Arch Linux, instalación, comandos
---

# Instalar Arch Linux

Descargar la imagen de instalación de Arch Linux desde el sitio oficial.

## Crear el usb boatable

Crear el boot con el programa rufus.

## Iniciar la instalación

- Cambiar configuración de teclado a español.
  `loadkeys es`

- Verificar si tenemos conexión a internet.
  `ping www.google.com`

### Conexión por wifi

- `ip link` para ver las interfaces de red y ver cual es su adaptador wifi.
- `ip link set wlan0 up` para levantar la interfaz de red.
- `iwlist wlan0 scan | more` listar las redes disponibles.

#### Conectar wifi sin contraseña

- `iwconfig wlan0 essid CNT_GAS`

  #### Conectar wifi con contraseña WEB

  - `iwconfig wlan0 essid CNT_GAS key s:12345`

  #### Conectar wifi con contraseña WPA

  - `wpa_passphrase CNT_GAS 12345 > /etc/configWIFI`
  - Ver el archivo de configuración `cat /etc/configWIFI`
  - Conectar a wifi `wpa_supplicant -B -i wlan0 -D wext -c /etc/configWIFI`

### Conexión que me funciono

- `iwctl --passphrase 123456 station wlan0 connect "ALFA-GASPATA"`

## Particiones

### UEFI

- Verificar si el ordenador esta en modo UEFI
  `ls /sys/firmware/efi/efivars`

- Verificar si tenemos particiones
  `lsblk`

- `cfdisk` y elegimos gpt para formato de particiones.

#### Crear las particiones

- /boot -> 512M -> EFI System -> write
- / -> Linux Filesystem -> write
- /home -> Linux Filesystem -> write
- /swap -> Linux Swap -> write

#### Formatear particiones

- /boot -> `mkfs.vfat -F32 /dev/sda1`
- / -> `mkfs.ext4 /dev/sda2`
- /home -> `mkfs.ext4 /dev/sda3`
- /swap -> `mkswap /dev/sda4` -> `swapon`

#### Montar particiones

- / -> `mount /dev/sda2 /mnt`
- /boot -> `mkdir /mnt/boot` -> `mkdir /mnt/boot/efi`-> `mount /dev/sda1 /mnt/boot/efi`
- /home -> `mkdir /mnt/home` -> `mount /dev/sda3 /mnt/home`

### BIOS

- `cfdisk /dev/sda` y elegimos dos para formatear de particiones.

#### Crear las particiones

- /boot -> 512M -> primary -> 83 Linux -> Bootable -> write
- / -> primary -> 83 Linux -> write
- /home -> primary -> 83 Linux -> write
- /swap -> primary -> 82 Linux Swap / Solaris -> write

#### Formatear particiones

- /boot -> `mkfs.ext2 /dev/sda1`
- / -> `mkfs.ext4 /dev/sda2`
- /home -> `mkfs.ext4 /dev/sda3`
- /swap -> `mkswap /dev/sda4` -> `swapon`

#### Montar particiones

- / -> `mount /dev/sda2 /mnt`
- /boot -> `mkdir /mnt/boot` -> `mount /dev/sda1 /mnt/boot`
- /home -> `mkdir /mnt/home` -> `mount /dev/sda3 /mnt/home`

### Instalar paquetes base

`pacstrap /mnt linux linux-firmware nano base networkmanager dhcpcd efibootmgr`

#### Conexión wifi

`pacstrap /mnt wpa_supplicant dialog netctl`

### Generar fstab

`genfstab /mnt >> /mnt/etc/fstab` -> `cat /mnt/etc/fstab`

### Configurar el sistema

- Ingresar como root `arch-chroot /mnt`
- Nombre de pc `echo pcArch > /etc/hostname`
- Consultamos la zona horaria `timedatectl list-timezones`
- Zona horaria `ln -sf /usr/share/zoneinfo/America/Guayaquil /etc/localtime`
- Idioma `nano /etc/locale.gen` -> descomentar `es_EC.UTF-8 UTF-8` -> `locale-gen`
- Reloj `hwclock -w` -> `date`
- Teclado `echo KEYMAP=es > /etc/vconsole.conf` `echo LANG=es_EC.UTF-8 > /etc/locale.conf`

### GRUB

#### UEFI

`grub-install --efi-directory=/boot/efi --bootloader -id='Arch Linux' --target=x86_64-efi`

#### BIOS

`grub-install /dev/sda`

#### Configurar grub

`grub-mkconfig -o /boot/grub/grub.cfg`

### Crear usuario

- Clave root `passwd`
- Crear usuario `useradd -m -g users -G wheel -s /bin/bash juan`
- Clave usuario `passwd juan`
- Desmontar particiones `exit` -> `umount -R /mnt`
- Reiniciar `reboot`

### Si vas a utilizar el grub de otra distro

- Ingresas a la otra distro no se necesita instalar nada del grub.
- `sudo update-grub`

### Internet

- Ingresar como super usuario `su`
- `ping www.google.com`
- Encender `systemctl start NetworkManager.service`
- Cada que se inicie de active `systemctl enable NetworkManager.service`

#### Wifi

- Ver las interfaces de red y ver cual es su adaptador wifi. `ip link`
- Levantar la interfaz de red. `ip link set wlan0 up`
- Listar las redes disponibles. `iwlist wlan0 scan | more`
- Conectar wifi. `nmcli dev wifi connect CNT_GAS password 12345`

### Drivers Gráficos

- Saber que gráficos necesitas instalar`lspci | grep -i VGA`
- Gráficos genéricos `pacman -S xf86-video-vesa`
- Gráficos Intel `pacman -S xf86-video-intel intel-ucode`
- Gráficos Nvidia `pacman -S nvidia nvidia-utils nvidia-settings`
- Gráficos AMD `pacman -S xf86-video-amdgpu`

### Instalar Xorg y Mesa

- `pacman -S xorg-server xorg-xinit mesa mesa-demos`

### Instalar entorno de escritorio

- Gnome `pacman -S gnome gnome-extra`
- Gestor de inicio `pacman -S gdm` -> `systemctl enable gdm.service`
- `reboot`
