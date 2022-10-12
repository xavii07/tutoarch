---
title: Instalación y configuración de Zsh
description: Instalación y configuración de Zsh
keywords: Zsh, instalación, configuración
---

## Instalar Zsh

- `echo $SHELL` para ver el shell actual
- `sudo pacman -S zsh zsh-completions` para instalar zsh y completar el comando
- `chsh -s /bin/zsh` para cambiar el shell por defecto

### Personalizar ZSH con Oh My Zsh.

- Repositorio de [Oh My Zsh](https://ohmyz.sh/)
- Necesitas tener instalado git y curl
- `sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` para instalar oh my zsh
- Podemos ingresar a `nano ~/.zshrc` para poder cambiar el tema y otras configuraciones

### TEMA: powerlevel10k

- `yay -S --noconfirm zsh-theme-powerlevel10k-git` para instalar el tema
- `echo 'source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc` para agregar el tema
- `sudo pacman -S powerline-common awesome-terminal-fonts` para instalar las fuentes
- `yay -S --noconfirm ttf-meslo-nerd-font-powerlevel10k` para instalar las fuentes
- `p10k configure` para configurar el tema
- `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting` para instalar el plugin de sintaxis
- `git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions` para instalar el plugin de autosugerencias
- `nano ~/.zshrc` editamos el archivo y agregamos los plugins
- ```bash
    plugins=(git
  zsh-autosuggestions
  zsh-syntax-highlighting
  )
  ```
- `source ~/.zshrc` para aplicar los cambios
