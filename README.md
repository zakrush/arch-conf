# About

Здесь различные конфиги и небольшая инструкция об установке Arch linux на i3+polybar+zsh

## Установка Arch(в VBOX)

http://dkhramov.dp.ua/Comp.InstallArchOnVitrualbox#.X7bBgc0zYuU  

### ДИСК

##### Размечаем диски  
```
fdisk -l
```
или  
```
cfdisk /dev/sda
```
Размечаем `bootable 1G /dev/sda1`(для биоса) и `dev/sda2` для оставшегося диска)

##### Подготовка диска

```
#форматируем
mkfs.ext4 /dev/sda1
mkfs.ext4 /dev/sda2

# монтируем
mount /dev/sda2 /mnt
mkdir /mnt/etc
mkdir -p /mnt/var/lib/pacman
mount --bind /dev /mnt/dev
mount --bind /proc /mnt/proc
mount --bind /sys /mnt/sys

# устанавливаем время
timedatectl set-ntp true

# генерируем fstab
genfstab -U /mnt >> /mnt/etc/fstab
```

### Установка первоначальных пакетов
##### Добавляем зеркало в `/etc/pacman.d/mirrorlist`
```
#устанавливаем нужные тулы
pacstrap /mnt base base-devel linux linux-firmware nano dhcpcd net-tools grub linux-headers networkmanager network-manager-applet

#or
#pacman -r /mnt -Sy base base-devil grub nano linux-headers net-tools dhcpcd
```
### Работа в системе
##### Переключам chroot
```
arch-chroot /mnt
```

##### Локаль 
В `etc/locale.gen` раскомментируем
ru_RU.UTF-8 UTF-8

```bash
echo 'LANG=ru_RU.UTF-8' > /etc/locale.conf
locale-gen
```

Включаем dhcpcd сервис для получения адресов
`systemctl enable dhcpcd`

##### Время 
```bash
ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime
hwclock — systohc
```

##### Загрузчик
```bash
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```
```bash
#Генерируем ядро (не надо при использовании grub)
# mkinitcpio -p linux
```

##### Установка ключей репозиториев
```bash
pacman-key --init
pacman-key --populate archlinux
```

##### Паролим рута: 
`passwd`

```bash
nano /etc/sudoers (wheel group)
```
##### Размонтируемся и перезагружаемся
```bash
exit
umount /mnt/boot
umount /mnt
reboot
```
### DualBOOT

`lsblk` ищем диск с Windows
`fdisl -l /dev/<disk>` ищем загрузчик Windows
`mount /dev/sdc1 /mount` монтируем загрузчик
Если не монтируется из-за ntfs то устанавливаем доп. прогу, так же устанавливаем пакет ядра
`pacman -S ntfs-3g os-prober`
бэкапим конфиг GRUB и генерируем новый: 
```cp /boot/grub/grub.cfg /boot/grub/grub.cfg-origin
grub-mkconfig -o /boot/grub/grub.cfg
```

### Файл подкачки
```bash
fallocate -l 1G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
```

##### Добавляем swapfile 
В /etc/fstab прописываем: 
```/swapfile none swap defaults 0 0```
используется TAB button as delimenter


### УСТАНОВКА ПОДДЕРЖКИ VBOX
По информации из [АрчВики](https://wiki.archlinux.org/index.php/VirtualBox_(%D0%A0%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9)#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0_Arch_Linux_%D0%B2_%D0%B2%D0%B8%D1%80%D1%82%D1%83%D0%B0%D0%BB%D1%8C%D0%BD%D1%83%D1%8E_%D0%BC%D0%B0%D1%88%D0%B8%D0%BD%D1%83 "установка поддежки VBOX TOOLS")

##### Устанавливаем модули ядра: 
`pacman -S virtualbox-guest-utils`
Загружаем модули ядра вручную:
`modprobe -a vboxguest vboxsf vboxvideo`

Для Автозагрузки модулей прописываем файл: 
```/etc/modules-load.d/virtualbox.conf```
vboxguest
vboxsf
vboxvideo

`systemctl enable vboxservice`

##### после установки других модулей
Если у вас нет каких-либо DE, добавьте следующую строку в начале ~/.xinitrc перед любыми exec функциями:
```bash
~/.xinitrc
/usr/bin/VBoxClient-all
```

# Кастомизация

#### Обновляемся
```
pacman -Syyuu && pacman -S python docker htop git bash-completion wget lsof unzip p7zip openssh  nitrogen 

**nitrogen** - обои
```

#### Создаем пользователя
```bash
groupadd -r autologin && \
useradd -m -g wheel -G docker,autologin -s /bin/bash dmitriy && \
passwd dmitriy
```

### AUR intall
```bash
sudo -i -u dmitriy
mkdir sources && \
cd sources && \
git clone https://aur.archlinux.org/yay.git && \
cd yay && \
makepkg -si 
```
#### УСТАНОВКА ВСЕХ ПАКЕТОВ, В т.ч. драйвера для xiaomi network
```
yay -S python docker htop git wget lsof unzip p7zip nitrogen nvidia-utils nvidia-settings xorg-server-devel sddm xorg-apps xorg-xinit i3-wm numlockx dmenu ranger rofi picom feh xorg-setxkbmap xorg-xrandr xclip alacritty pulseaudio pavucontrol bluez bluez-utils pulseaudio-bluetooth blueman noto-fonts ttf-ubuntu-font-family ttf-dejavu ttf-freefont ttf-liberation ttf-droid ttf-inconsolata ttf-roboto terminus-font ttf-font-awesome rtw89-dkms-git micro vscode google-chrome polybar firefox notepadnext python-pip telegram-desktop pycharm-professional burpsuite-pro telegram-desktop jq dnsutils networkmanager-openvpn zsh-completions zsh-theme-powerlevel10k-git autojump autotiling nerd-fonts-jetbrains-mono xclip maim
```


#### Устанавливаем менеджер логина и темы к нему 
```bash
pacman -S slim slim-themes
```
Конфиги тут: `/etc/slim.conf`  
```bash
systemctl enable slim
```

#### Оптимизируем CPU
Тут в примере 4 ядра.
```bash
#Инфа по ядрам: 
lscpu | grep “CPU(s):” | grep -v NUMA:
grep “COMPRESSXZ=(xz” /etc/makepkg.conf && \
grep “#MAKEFLAGS=\”-j” /etc/makepkg.conf && \
sudo sed -i ‘s/COMPRESSXZ=(xz -c -z -)/COMPRESSXZ=(xz -c -T 4 -z -)/g’ /etc/makepkg.conf && \
sudo sed -i ‘s/#MAKEFLAGS=”-j2"/MAKEFLAGS=”-j9"/g’ /etc/makepkg.conf
grep “COMPRESSXZ=(xz” /etc/makepkg.conf && \
grep “#MAKEFLAGS=\”-j” /etc/makepkg.conf 
```


## Установка иксов и i3 
Совмещал статьи:
https://ordanax.github.io/i3wm_polybar
https://xakep.ru/2017/03/22/geek-desktop/
https://losst.ru/nastrojka-i3wm

```bash
 pacman -S xorg-server xorg-apps xorg-xinit i3-wm numlockx xterm dmenu ttf-clear-sans speedtest-cli ranger rofi ttf-font-awesome nitrogen picom feh --noconfirm --needed
 ```
 Устанавливаем шрифты  
 ```bash 
 pacman -S noto-fonts ttf-ubuntu-font-family ttf-dejavu ttf-freefont ttf-liberation ttf-droid ttf-inconsolata ttf-roboto terminus-font ttf-font-awesome -noconfirm -needed
 ```
---
Чтобы работало копирование, необходимо установить и настроить clipboard-manager
https://wiki.archlinux.org/index.php/clipboard
Например, xfce4-clipman-plugin  

---

#### Настройка urxvt

```bash
pacman -S rxvt-unicode  urxvt-perls
nano ~/.Xresources
```
 
##### Установка файлов окружения и нужное ПО
```bash
yay cherrytree slack micro vscode
pacman -S firefox chromium pycharm-community-edition urvxt telegram notepadqq python-pip telegram-desktop jq python-pip python pycharm-community-edition dnstools
```


**COLORS!!!(Можно взять [отсюда](http://dotshare.it/category/terms/colors/))**


#### Устанавливаем zsh
```bash
pacman -S zsh zsh-completions
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
Полезная статья на [Хабр](https://habr.com/ru/post/516004/)  
```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git 
$ZSH_CUSTOM/plugins/zsh-syntax-highlighting
# edit plugins=(git zsh-syntax-highlighting) in .zshrc
yay -S nerd-fonts-jetbrains-mono
git clone https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
```
Для настройки и кастомизации zsh используется ***Powerlevel10k***

#### i3statusbar
Настраивается тут: `~/.config/i3status/config`
Полезный [link](https://igancev.ru/2020-05-11-configuring-i3status-in-i3wm)


### POLYBAR
**HELPFUL LINKS**
https://ordanax.github.io/i3wm_polybar
https://github.com/polybar/polybar/wiki
https://github.com/ordanax/dots/tree/master/3wm_v_3

```bash
yay -S polybar ttf-weather-icons ttf-clear-sans tlp
sudo pacman -S ttf-font-awesome 
pip install --user speedtest-cli
```
   
## Полезные ссылки и ресурсы

http://dotshare.it/category/terms/colors/  
https://medium.com/@mudrii/arch-linux-installation-on-hw-with-i3-windows-manager-part-1-5ef9751a0be  install on VM video:  
https://www.youtube.com/watch?v=ex87GoUEcac



## Файлы необходимые для актуализации в git

Плагины для urvxt: `/usr/lib/urxvt/perl`
```
~/.Xresources
~/.xinitrc
~/.bashrc
~/.config/
~/.zshrc
~/.zsh-profile
~/.p10k.zsh
~/.config/*
```

## Helpfull links
Краткие хоткеи
https://eax.me/i3wm/#:~:text=Mod%20%2B%20F%20%E2%80%94%20%D1%80%D0%B0%D1%81%D0%BA%D1%80%D1%8B%D1%82%D1%8C%20%D0%BE%D0%BA%D0%BD%D0%BE%20%D0%B2%D0%BE,%D0%BD%D0%B0%D0%BF%D1%80%D0%B8%D0%BC%D0%B5%D1%80%2C%20%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%20%D0%BE%D0%B1%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D1%8F%20%D0%BA%D0%BE%D0%BD%D1%84%D0%B8%D0%B3%D0%B0)%3B

https://igancev.ru/2020-01-05-installing-and-configuring-i3wm-on-arch-linux !!! очень качественная статья

https://github.com/arcolinux/arcolinux-i3wm Множество конфигов

https://github.com/arcolinux/arcolinux-i3wm/blob/master/etc/skel/.config/i3/picom.conf конфиг Picom

https://igancev.ru/2020-04-18-terminal-emulator-alacritty про терминал alacritty

https://gist.github.com/tz4678/bd33f94ab96c96bc6719035fcac2b807 про arch и все что связано с работой с ним

https://habr.com/ru/post/515750/

https://rtfm.co.ua/arch-linux-ustanovka-s-efi-i-dual-boot-s-windows/#Windows_dual_boot  ПРо дуалБуут
