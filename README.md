# Instalação do Arch Linux

> Arch linux (kde) para o curso de ciências da computação, homework, etc.

> Rápido e direto tutorial de instalação retirado da wiki https://wiki.archlinux.org/

> Para fazer o bootável use "balenaEtcher"

#### Inicie o pendrive bootável no modo uefi/efi

#### Defina o layout do teclado
 * loadkeys br-abnt2

#### Conecte a uma rede wifi (desnecessário se estiver conectado a uma cabo de rede)

iwctl por linha de comando

 * iwtcl device list
 Se a wlan0 estiver desligada...
 * ip link set wlan0 up
 
 Se retornar o erro RTNETLINK asnwers Operation not possible due to RF-kill
	 * rfkill list all
	 * rfkill unblock all
 
 * iwctl station wlan0 scan
 * iwctl station wlan0 get-networks
 * iwctl station wlan0 connect meuwifi

#### Teste de conecção
	
ping www.google.com
"ctrl" + "c" para parar o teste

ou

ping -c 3 www.google.com
para o teste após 3 tentativas

#### Esquema de Particionamento, formatação e montagem.

	/dev/sda1	/efi	512Mb
	/dev/sda2	/	50Gb
	/dev/sda3	swap	Dobro da RAM
	/dev/sda4	/home	Restante

#### Particionamento

 * cfdisk -z /dev/sda
 
	[IMAGEM]

#### Formatação

 * mkfs.vfat /dev/sda1
 * mkfs.ext4 /dev/sda2
 * mkswap /dev/sda3
 * mkfs.ext4 /dev/sda4

#### Montagem

 * mount /dev/sda2 /mnt
 * swapon /dev/sda3

#### Crie a pasta home
 * mkdir /mnt/home

#### Monte a pasta home	
 * mount /dev/sda4 /mnt/home

#### Crie pasta efi

 * mkdir /mnt/efi
 * mount /dev/sda1 /mnt/efi


#### Atualize o relogio do sistema

 * timedatectl set-ntp true

#### Instale os pacotes essenciais do sistema

  * pacstrap /mnt os_pacotes_aqui_abaixo_separados_por_espaço

	base
	base-devel
	linux
	linux-firmware
	vim

#### Gere o arquivo fstab

 * genfstab -U /mnt >> /mnt/etc/fstab

#### Mude para root

 * arch-chroot /mnt

#### Defina o fuso horário

 * ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
 * ln -sf /usr/share/zoneinfo/America/Maceio /etc/localtime
 * hwclock --systohc

#### Localização

 * vim /etc/locale.gen
	Descomente apagando # do [#en_US.UTF-8 UTF-8]
 * locale-gen

 * echo "LANG=en_US.UTF-8" >> /etc/locale.conf

#### Salve o layout do teclado

* echo "KEYMAP=br-abnt2" >> /etc/vconsole.conf

#### Defina a configuração de internet

 * echo "NomedaMáquina" >> /etc/hostname

 * vim /etc/hosts

	[Imagem]

#### Defina senha do usuário root

* passwd

#### Instale gerenciador de boot

	grub efibootmgr

#### Configure o gerenciador de boot

 * grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB
 * grub-mkconfig -o /boot/grub/grub.cfg

#### Instale os drivers, interface gráfica, programas básicos.
	xorg
	xorg-server
	xorg-xinit
	xf86-video-intel
	mesa
	networkmanager
	wpa_suplicant
	dialog
	pulseaudio
	kde-desktop

#### kde e seus programas + programas terceiros

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

#### Crie usuário, pasta na partição /home e permissões especiais

 * useradd -m -G audio,video,storage,wheel -s /bin/bash pessoa1
 * passwd pessoa1

#### Permissão do sudo

 * vim /etc/sudoers
descomente wheel (ALL) = ALL

#### Crie o arquivo .xinitrc

 * echo "exec startplasma-x11" >> ~/.xinitrc

#### Habilite os serviços

* systemctl enable NetworkManager
* systemctl enable sddm.service

#### Recomendação de pacotes para o terminal

	ranger
	oh-my-zsh "bira"
	neofetch
	htop
	git
	most
	
#### Finalizando
