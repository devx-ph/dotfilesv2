# Dotfiles
Dotfiles and instructions to make my Arch Linux portable and easy to replicate

## Last Screenshot
![image](https://github.com/devx-ph/dotfilesv2/blob/main/screenshot.png?raw=true)

# How I Installed Arch Linux

## Basic Steps
- Boot live environment
- Connect to the internet `iwctl station <INTERFACE> connect <SSID>`
- Use archinstall command `archinstall`
    
    This command will make your life easier, some of my choices are:
    
   ```yaml
   Bootloader: grub
   Profile: xorg
   Audio: pipeware
   NetworkConfiguration: NetworkManager
   AdditionalPackages: [
        vi,                 # Text Editor
        vim,                # Test Editor
        git,                # Git
        qtile,              # Window manager
        qutebrowser,        # Web Browser
        zip,                # Zip command
        unzip,              # Unzip command
        neovim,             # Text Editor / IDE
        brightnessctl,      # Brightness control
        zsh,                # Shell
        kitty,              # Terminal Emulator
        arandr,             # Screen profiles
        autorandr,          # Screen profiles
        dunst,              # Notifications
        feh,                # Wallpapers
        rofi                # Menus
  ]
   ```
   **Note 1:** Set a password for root and add an non root user \
   **Note 2:** You dont need chroot into the installed distro, just reboot and login

## AUR helper - [Yay](https://github.com/Jguer/yay#readme)
- Install yay
    ```bash
        git clone https://aur.archlinux.org/yay.git
        cd yay
        makepkg -si
    ```
 
## Fonts
- Install these fonts
    ```bash
        yay -S ttf-font-awesome ttf-fira-code ttf-nerd-fonts-symbols
    ```

## Dotfiles (This reposiory)
- Clone and checkout dotfiles repository
    
    ```bash
    echo ".dotfiles" >> .gitignore
    git clone --bare <https://github.com/devx-ph/dotfiles.git> $HOME/.dotfiles
    alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles --work-tree=$HOME'
    dotfiles config --local status.showUntrackedFiles no
    dotfiles checkout
    ```
    **Obs.:** When checking out you may receive a message requesting to remove files when you already have them.

## Shell - [Zsh](https://wiki.archlinux.org/title/Zsh)
- Run `chsh -s /usr/bin/zsh` (to make it default shell)
- Install `exa` and `procs` (that replace `ls` and `ps`. See more on [Rewritten in Rust Commands](https://zaiste.net/posts/shell-commands-rust))
- Clone `zsh-autosuggestions` [from GitHub](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md#manual-git-clone)
- Install and configure [powerlevel10k](https://github.com/romkatv/powerlevel10k) (zsh theme)
- Install `fzf` fuzzy finder 

## Terminal Emulator - [Kitty](https://wiki.archlinux.org/title/Kitty)
- Nothing to do

## Display Manager - [Ly](https://github.com/fairyglade/ly)
- Enable Ly service
    ```bash
        sudo systemctl enable ly.service
    ```
    
## Window Manager - [Qtile](https://wiki.archlinux.org/title/Qtile)
- Install these packages:
    ```bash
        yay -S wireless_tools iwgtk alsa-utils flameshot python-pip

        # wireless_tools (for wlan widget)
        # iwgtk (for GUI on click of wlan widget)
        # alsa-utils (for volume)
        # flameshot (for screenshots)
        # pip (for install python packages)
    ```
    
- Install these python packages
    ```bash
        pip install iwlib psutil dbus-next
    ```

- Install a compositor (for transparency, transitions, blurs and more effects) \
    I used [Pijulius](https://github.com/pijulius/picom)

**Note 1:** Fonts are required to see all symbols in Qtile. Otherwise you will see weird squares instead symbols. \
**Note 2:** Check [Qtile documentation](http://docs.qtile.org/en/stable)

## Wallpapers and Screen Locker (Optional) - [Betterlockscreen](https://github.com/betterlockscreen/betterlockscreen)
- Download some wallpaper images into `~/.wallpapers`
- Install the screen locker
    ```bash
        yay -S betterlockscreen
        betterlockscreen -u ~/.wallpapers
    ```
- Use [Feh](https://wiki.archlinux.org/title/Feh) to set the wallpaper
    ```bash
        # Example
        feh --bg-fill --randomize ~/.wallpapers/*
    ```

## App Launcher and menus (Optional) - [Rofi](https://wiki.archlinux.org/title/Rofi) & [dmenu](https://wiki.archlinux.org/title/dmenu) 
- I used [adi1090x/rofi](https://github.com/adi1090x/rofi) for my rofi theme
    - Install adi1090x rofi theme
    - Run `dotfiles restore ~/.config/rofi`
    - For power menu works as expected:
        - run visudo and add these lines:
            ```bash
                ## Admin user group is allowed to execute halt and reboot 
                %admin ALL=NOPASSWD: /sbin/halt, /sbin/reboot, /sbin/poweroff
            ```
        - And add your user to admin group
            ```bash
                sudo groupadd admin
                sudo usermod igortxra -a -G admin
            ```

## File Manager - [ranger](https://wiki.archlinux.org/title/Ranger)

## Other
### Discord 
`yay -S discord ttf-symbola noto-fonts-cjk noto-fonts-emoji`
### udiskie 
`yay -S udiskie` and see [permissions](https://github.com/coldfix/udiskie/wiki/Permissions)

## More Configurations
- Install and use **lxappearance** to set themes and icons. 

- Set Qutebrowser as default browser running: `xdg-settings set default-web-browser org.qutebrowser.qutebrowser.desktop`
- Set your Qutebrowser quickmarks (I stored somewhere and downloaded)
- Set a theme for Qutebrowser such as [Dracula](https://draculatheme.com/qutebrowser)

- Screen Profiles
    - First use **arandr** to configure screen layout
    - After that use **autorandr** to save the profile
         ```bash
            autorandr --save <profile-name>
         ```
