# Instalacao-do-Arch-Linux

 ## My arch linux (kde) for computer science course
 ### Rápido e direto tutorial de instalação

Defina o layout do teclado
 * loadkeys br-abnt2

######Check as conecções (aqui no caso, via wifi)
	Wifi-menu por interface
 * wifi-menu
ou	
	nmcli por linha de comando
 * nmcli device wifi list
 * nnmcli device wifi connect meuwifi password lasenadouaifai

Teste de conecção
	
	ping www.google.com
	["ctrl" + "c"] para parar o teste
ou
	ping -c 3 www.google.com
	para o teste após 3 tentativas

Esquema de Particionamento, formatação e montagem.

 * lsblk
 
	[IMAGEM]

	/dev/sda1	/efi	512M
	/dev/sda2	/	100Gb
	/dev/sda3	swap	4Gb ou 8Gb
	/dev/sda4	/home	Restante

Particionamento

 * cfdisk -z /dev/sda

	[IMAGEM]

Formatação

 * mkfs.vfat /dev/sda1
 * mkfs.ext4 /dev/sda2
 * mkswap /dev/sda3
 * mkfs.ext4 /dev/sda4

Montagem

# mount /dev/sda2 /mnt
# swapon /dev/sda3

Criar pasta home
# mkdir /mnt/home
	
# mount /dev/sda4 /mnt/home

Criar pasta efi

 * mkdir /mnt/efi
 * mount /dev/sda1 /mnt/efi

Atualizar o relogio do sistema

 * timedatectl set-ntp true

Instalar o pacotes essenciais do sistema

#pacstrap /mnt *

*base
*base-devel
*linux
*linux-firmware
*xorg
*vim
*nano

Gerar arquivo fstab

 * genfstab -U /mnt >> /mnt/etc/fstab

Mudar para root

 * arch-chroot /mnt

Definir fuso horário

 * ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
 * ln -sf /usr/share/zoneinfo/America/Maceio /etc/localtime
 * hwclock --systohc

Localização

 * vim /etc/locale.gen
	Descomente apagando # do [#en_US.UTF-8 UTF-8]
 * locale-gen

 * vim /etc/locale.conf
	LANG=en_US.UTF-8

Salvar o layout do teclado
 * vim /etc/vconsole.conf
	KEYMAP=

Definir configuração internet

 * echo "NomedaMáquina" >> /etc/hostname

 * vim /etc/hosts

127.0.0.1	localhost
::1		localhost
127.0.1.1	NomedaMáquina.localhost	NomedaMárquina

Definir senha
 * passwd

Instalar e configurar gerenciador de boot

 * pacman -S grub efibootmgr

 * grub-install --target=x86_64-efi --efi-directory=esp --bootloader-id=GRUB
 * grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB
 * grub-mkconfig -o /boot/grub/grub.cfg

Instalar drivers, interface gráfica, programas básicos.

 * xorg-server 
 * xorg-xinit 
 * xf86-video-intel
 * mesa
 * networkmanager
 * wpa_suplicant
 * dialog
 * pulseaudio


kde e seus programas + programas terceiros

	Equivalente ao winrar			ark
	Terminal				konsole
	Leitor de pdf				okular
	Visualiador de imagens			gwenview
	Gerenciador de arquivos			dolphin
	Terminal auxiliar			yakuake
	Google Chrome de código aberto		chromium
	Tradicional firefox			firefox
	Media player				vlc
	Torrent					ktorrent
	Office					calligra
	Documentação offline			khelpcenter
	Editor de texto				kate
	Estatistica de uso de disco		filelight
	Calculadora científica			kcalc
	Para procurar arquivos e pastas		kfind
	Capiturar Screenshot			spectacle

Criar usuário, pasta na partição /home e permissões especiais

 * useradd -m -G audio,video,storage,wheel -s /bin/bash pessoa1
 * passwd pessoa1

Permissão do sudo

 * vim /etc/sudoers
descomente wheel (ALL) = ALL

Recomendação

	ranger
	oh-my-zsh "bira"
	neofetch
	htop
	Fontes

Essetial stuff for computer science

	python
	python pip
	git

Talvez

	VScode
	Steam
	Spotify
	Discord
	Gimp
	Krita
	
Finalizando
