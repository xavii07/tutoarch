---
title: Instalación y configuración de Qtile
description: Instalación y configuración de Qtile
keywords: Qtile, instalación, configuración
---

## Habilitar sudo

- `su` para cambiar a root
- Instalar sudo `pacman -S sudo neovim`
- Instalar editor vi `pacman -S vi`
- Agregar usuario al grupo sudo `usermod -aG wheel juan`
- Abrir archivo `visudo` y descomentar la linea `%wheel ALL=(ALL) ALL`

## Instalar gestor de inicio

- `sudo pacman -S lightdm lightdm-gtk-greeter`
- `sudo nvim /etc/lightdm/lightdm.conf`
- Buscamos la linea `greeter-session=lightdm-gtk-greeter`
- `sudo systemctl enable lightdm.service`

## Instalar Qtile y paquetes necesarios

- `sudo pacman -S qtile xterm code brave-bin rofi which nitrogen`
- `sudo pacman -S ttf-dejavu ttf-liberation noto-fonts`
- `sudo pacman -S pulseaudio pavucontrol pamixer`
- `sudo pacman -S arandr udiskie ntfs-3g network-manager-applet`
- `sudo pacman -S volumeicon cbatticon xorg-xinit base-devel git thunar ranger glib2 gvfs lxappearance geeqie vlc unzip wget python-pip redshift`

## Instalar alacritty

- `sudo pacman -S alacritty neofetch htop`
- `nvim .config/qtile/config.py`
- Buscamos la linea en donde abre la terminal y colocamos `alacritty` en lugar de `terminal`
- Reiniciar qtile `mod + shift + r`

## Lanzar menu de inicio

- `nvim .config/qtile/config.py`
- Agregar la siguiente linea `Key([mod], "m", lazy.spawn("rofi -show drun"), desc="Lanzar menu de inicio"),`
- Reiniciar qtile `mod + shift + r`

## Crear archivo para que se ejecute al iniciar sesión

- Ingresar al link [Qtile-Arch](https://wiki.archlinux.org/title/Qtile) y buscamos la parte de **autostart** y copiamos.

```python
    import os
    import subprocess
    from libqtile import hook

    @hook.subscribe.startup_once
    def autostart():
        home = os.path.expanduser('~')
        subprocess.Popen([home + '/.config/qtile/autostart.sh'])
```

- Ingresamos al archivo `nvim .config/qtile/config.py`
- Al principio del archivo pegamos las tres primeras lineas del código que copiamos es decir las importaciones.
- Y al final pegamos las otras lineas del código que copiamos.

- Creamos y abrimos el archivo autostart.sh `nvim .config/qtile/autostart.sh` en donde configuraremos todas las apps que se ejecutaran al iniciar sesión.
- Agregamos las siguientes lineas

```bash
    #!/bin/bash
    setxkbmap latam & # Configurar teclado a español
    # iconos
    udiskie &
    nm-applet &
    volumeicon &

    cbatticon -u 5 & # icono de batería para portátiles
```

- Ejecutamos el archivo `chmod +x .config/qtile/autostart.sh`
- Reboot `reboot`

## Fondo de escritorio

- Ingresamos a [Drakula_theme](https://draculatheme.com/wallpaper) y descargamos el archivo .zip.
- Descomprimimos el archivo `unzip wallpaper-master.zip` y movemos la carpeta dentro de otra carpeta llamada imágenes en la raíz.
- Ejecutamos `nitrogen ~/imagenes/`
- Seleccionamos la imagen que deseamos.
- Editar archivo `autostart.sh` para que se ejecute al iniciar sesión.
- Agregamos la siguiente linea `nitrogen --restore &`

## Oh my bash

- `sudo pacman -S git curl`
- Instalamos repositorio `sh -c "$(curl -fsSL https://raw.github.com/ohmybash/oh-my-bash/master/tools/install.sh)"`
- Editamos el archivo `nano ~/.bashrc`
- Buscamos la linea `OSH_THEME="agnoster"` y cambiamos el valor por el de su preferencia.

## Terminal con LSD

- Instalamos lsd `sudo pacman -S lsd`
- Editamos el archivo `nano ~/.bashrc`
- Creamos un alias para ls `alias ls='lsd'`
- Reiniciamos la terminal

## BAT para CAT

- Instalamos bat `sudo pacman -S bat`
- Editamos el archivo `nano ~/.bashrc`
- Creamos un alias para cat `alias cat='bat'`

## Personalizar nano

- Ingresamos al repositorio de [nano-highlight](https://github.com/valerie-makes/nano-highlight)
- Creamos carpeta `mkdir kk`
- Clonamos el repositorio `git clone ...`
- `cd nano-highlight` -> `make install`
- `echo "include /.nano/syntax/ALL.nanorc" >> ~/.nanorc`

## Tema para GTK

- Ingresamos [Dracula_GTK](https://draculatheme.com/gtk)
- Descargamos el archivo .zip
- Descomprimimos el archivo `unzip gtk-master.zip`
- `mkdir ~/.themes`
- Movemos la carpeta a la carpeta de .themes `mv gtk-master ~/.themes/dracula`
- Abrimos lxappearance desde el menu y seleccionamos el tema.

## Iconos para GTK

- Descargamos los iconos .zip
- Descomprimimos el archivo `unzip Dracula.zip`
- `mkdir ~/.icons`
- Movemos la carpeta a la carpeta de .icons `mv Dracula ~/.icons`
- Abrimos lxappearance desde el menu y seleccionamos los iconos.

## Configurar menu rofi

- `rofi-theme-selector`
- Seleccionamos el tema que deseamos. `Alt + a` para aplicar el tema.

## Tema para alacritty

- Ingresamos a [Dracula_Alacritty](https://draculatheme.com/alacritty)
- Descargamos el archivo .zip
- Descomprimimos el archivo `unzip alacritty-master.zip`
- `mkdir ~/.config/alacritty`
- Copiamos el archivo a la carpeta creada `dracula.yml` -> `cp dracula.yml ~/.config/alacritty/alacritty.yml`

## Instalar nerd-fonts

- Instalar yay

```bash
    git clone https://aur.archlinux.org/yay-bin.git
    cd yay-bin
    makepkg -si
```

```bash
    cd ~/Downloads
    yay --getpkgbuild nerd-fonts-complete (or git clone https://aur.archlinux.org/nerd-fonts-complete.git)
    cd nerd-fonts-complete
    wget -O nerd-fonts-2.1.0.tar.gz https://github.com/ryanoasis/nerd-fonts/archive/v2.1.0.tar.gz
    makepkg -sci BUILDDIR=.
```

## Instalar picom

- `yay -S picom-git`
- `xprop` para saber el nombre de la ventana.

## Configurar barra de estado

- Abrimos el archivo `nvim .config/qtile/config.py`
- En screens podemos modificar todo lo necesario

## Configurar lightdm

Podemos instalar temas desde estos repositorios

- [lightdm-webkit-theme-osmos](https://github.com/Exauthor/lightdm-webkit-theme-osmos)
- [lightdm-webkit-theme-glorious](https://github.com/manilarome/lightdm-webkit2-theme-glorious)

## Pacman

- listar paquetes instalados `pacman -Qqe`

## Descomprimir archivos .tar

- `tar -xf archivo.tar`
