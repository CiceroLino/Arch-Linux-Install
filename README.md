<a href="https://github.com/ApertureLaboratory">
    <img alt="Aperture Laboratories - Chell Series" src="https://github.com/ApertureLaboratory/4chell/blob/main/.github/ChellSeries.png" />
</a>

</br>

<p align="center">
    A intenção desse repositório é guiar você através de tutoriais, artigos e dicas para te mostrar os caminhos na área da tecnologia e desenvolvimento, seja você iniciante, curioso ou alguém que anseia e se alimenta de conhecimento. </br>
    Esse é a "Chell Series"
</p>

<p align="center">
Esse projeto é alimentado pela equipe de desenvolvedores e contribuidores da <a href="https://discord.gg/nyTRNSV">Aperture Laboratories</a>
</p>

<p align="center">
    <i>Science without results is just a witchcraft</i>
</p>

</br></br>

# Guia de instalação do Arch Linux com interfaces gráficas (KDE e GNOME, e em breve, outras)

Este guia (feito por um estudante universitário) é para os curiosos que querem começar com Linux do jeito mais old school e prático para aprendizado, o conhecimento adquirido aqui pode levantar uma boa base para seus estudos com linux e máquinas virtuais, e dito isso, chega de enrolação e vamos a por a mão na massa.

Ah! E também pularei toda introdução do Arch Linux, isto é, do que é (espero que tenha pesquisado), sua história, vantagens e desvantagens, etc (façam seu dever de casa). Mas também sinta-se livre em abrir uma issue e perguntar caso tenha alguma dúvida (da instalação) que não consiga resolver mesmo pesquisando ou procurando na documentação e fóruns.

## Sumário

1. Requisitos e preparação
2. Iniciando o bootável e considerações
3. Layout do teclado, internet
4. Esquema de particionamento, formatação e montagem
4.1 Ocupando todo o HD
4.2 Dual boot com windows 10
5. Instalação dos pacotes base do sistema
6. Configurações iniciais
7. Configuração do usuário
8. Instalação e configuração da interface gráfica e aplicativos

### Requisitos e preparação

- Talvez o dia todo livre para fazer somente essa instalação hahahaha!
- Conexão com internet (wifi ou cabeada)
- Pendrive [bootável](https://www.balena.io/etcher/) com a iso [Arch Linux](https://archlinux.org/download/) (Esse software é o mais fácil e prático que achei, mas sinta-se livre em usar outro)
- Acesso ao boot do sistema (é comum ter que desabilitar o security boot para iniciar o pendrive bootável)
- Tempo e paciência (é um processo demorado, semelhante a uma cirurgia no sistema)
- Um pouco de inglês ajuda muito. (Estudar tecnologia sem inglês é igual a um padre pregar sem saber latim na idade média)

### Iniciando o bootável e considerações

![Lain](/imgs/lain.gif)

Siga esses passos:

1. Plugue o pendrive na sua máquina desligada
2. Ligue e acesse o boot (não a bios) da máquina, e isso dependerá da fabricante da placa-mãe. No meu caso é a tecla f12 o menu de boot e f2 a bios
3. Ache o seu pendrive e clique enter

Ao seguir os passos acima e esperar um pouco, você estará de cara com o terminal. Agora a parte divertida vai começar.

### Layout do teclado, internet

Você começará definindo o layout do teclado para o nosso que estamos acostumados, isso inclui o 'ç' e pontuações nos lugares que novamente estamos habituados.

`loadkeys br-abnt2`

Aproveitando vou abrir uma tangente: caso tenham dúvidas no comando, vocês podem a qualquer momento acessar as páginas do manual, ou documentação (também conhecidas como man pages) pelo terminal, exemplo, `man loadkeys`, daí é só usar as setas para cima e para baixo e teclar q de 'quit' para sair do manual.

![Man page](/imgs/most.png)

Defina uma conexão estável com a internet para baixar e atualizar pacotes, se for cabeada vai da para pular essa parte e se não, prossiga comigo.

Faça uma checagem da sua interface de rede, basicamente verificar quais você tem e, se está listada e ativa

`ip link`

Caso você tenha uma interface com o nome de wlan0 prossiga usando `iwctl`

Conecte-se a uma rede wifi seguindo os passos abaixo

Faça a checagem da rede

`iwctl device list`

Se wlan0 estiver ainda desligada...

`ip link set wlan0 up`

Caso retorne algum erro tipo ***RTNETLINK answers Operation not possible due to RF-kill*** faça:

`rfkill list all` (para curiosos)

`rfkill unblock all` (para aqueles que crentes na fonte: confia)

Para fazer a conexão sigas esses passos (bem autoexplicativos)

`iwctl station wlan0 scan`

`iwctl station wlan0 get-networks`

`iwctl station wlan0 connect meu_wifi`

Teste de conexão

`ping www.google.com`

> “ctrl” + “c” para parar o teste

ou

`ping -c 3 www.google.com`

> Para o teste após 3 tentativas

### Esquema de particionamento, formatação e montagem

![Particionamento](/imgs/partition.jpg)

nota: o que vc vai mais usar no `cfdisk` é "new", tamanho número em G para Gib ou M para Mib, "write" e "type" para escrever as modificações. Você pode pesquisar por fora o conceito ou entendê-lo com a prática.

### Ocupando todo o HD

nota: dual boot com windows é mais a frente, mas por favor, leia isso aqui, vai lhe ser útil

Primeiro vamos montar o esquema de particionamento e para isso vamos usar a linha de comando `cfdisk` por ser mais interativa

Use `cfdisk -z /dev/sda`, onde a flag `-z` indica que será no cenário a partir do zero, ou seja, em todo o disco do HD

Agora escreva as partições seguindo o modelo abaixo (caprichei para não ter que explicar isso em texto):

|Partição        |Diretório                      |Espaço                       |Tipo                         |
|----------------|-------------------------------|-----------------------------|-----------------------------|
|/dev/sda1       |`/efi`                         | 300Mb                       | efi                         |
|/dev/sda2       |`/`                            | O quanto você quiser        | ext4                        |
|/dev/sda3       |`swap`                         | Dobro da RAM                | swap                        |
|/dev/sda4       |`/home`                        | Restante da memória         | ext4                        |

Mas basicamente, será assim: `/efi` será o carinha que vai dizer para placa mãe que irá dizer como vamos dar o boot no sistema, `/` vai ser o `C:` do windows que estamos habituados, ou seja, vai carregar todo mundo, principalmente os programas e configurações, `/swap` este é a nossa extenção da memória ram e por último e não menos importante `/home` que será para nossos usuários, ou usuário, que terá documentos, downloads, músicas, etc.

Formatação

`mkfs.vfat -F 32 /dev/sda1` para formatar em f32 igual ao sistema de arquivos de pendrive

`mkfs.ext4 /dev/sda2` para formatar em sistema de arquivo linux

`mkswap /dev/sda3` para formatar em sistema de arquivo swap

`mkfs.ext4 /dev/sda4` e esse também é para formatar em sistema de arquivo linux

Montagem (USE `lsblk` para fazer checagem, e resumindo a utilidade dele, lista informações de blocos disponíveis)

Faça a montagem da pasta raiz

`mount /dev/sda2 /mnt`

Faça a montagem do nosso swap

`swapon /dev/sda3`

Crie a pasta home

`mkdir /mnt/home`

Monte a pasta home

`mount /dev/sda4 /mnt/home`

Crie e monte a pasta efi

`mkdir /mnt/efi`

`mount /dev/sda1 /mnt/efi`

### Dual boot com windows 10

Aqui em baixo é o esquema de particionamento formatação e montagem em disco previamento ocupado (com windows e mais especificamente o 10), que foi o que estava instalado na minha máquina e por onde me basiei a instalação. Então basicamente será o mesmo processo, exceto que você precisará separar uma partição no "gerenciador de discos" do windows.

Ou seja, visualmente explicando.

|Partição        |Diretório                      |Espaço                       |Tipo                         |
|----------------|-------------------------------|-----------------------------|-----------------------------|
|/dev/sda1       |`Windows efi`                  |                             |                             |
|/dev/sda2       |`Microsoft reserved`           |                             |                             |
|/dev/sda3       |`Microsoft basic data`         |                             |                             |
|/dev/sda4       |`Windows recovery environment` |                             |                             |
|/dev/sda5       |                               |                             |                             |

Esse `/dev/sda5` é o espaço que você separou

Particionamento

`cfdisk`

Escreva as partições seguindo o modelo abaixo:

|Partição        |Diretório                      |Espaço                       |Tipo                         |
|----------------|-------------------------------|-----------------------------|-----------------------------|
|/dev/sda1       |`Windows efi`                  |                             |                             |
|/dev/sda2       |`Microsoft reserved`           |                             |                             |
|/dev/sda3       |`Microsoft basic data`         |                             |                             |
|/dev/sda4       |`Windows recovery environment` |                             |                             |
|/dev/sda5       |`/efi`                         | 300Mb                       | efi                         |
|/dev/sda6       |`/`                            | O quanto você quiser        | ext4                        |
|/dev/sda7       |`swap`                         | Dobro da RAM                | swap                        |
|/dev/sda8       |`/home`                        | Restante da memória         | ext4                        |

Formatação (este trecho expliquei anteriormente, portanto, vou pular as explicações)

`mkfs.vfat -F 32 /dev/sda5`

`mkfs.ext4 /dev/sda6`

`mkswap /dev/sda7`

`mkfs.ext4 /dev/sda8`

Montagem (USE `lsblk` para fazer checagem)

`mount /dev/sda6 /mnt`

`swapon /dev/sda7`

Crie a pasta home

`mkdir /mnt/home`

Monte a pasta home

`mount /dev/sda8 /mnt/home`

Crie e monte a pasta efi

`mkdir /mnt/efi`

`mount /dev/sda5 /mnt/efi`

Atualize o relógio do sistema

`timedatectl set-ntp true`

Faça a checagem

`timedatectl status`

## Instalação e configuração base do sistema

Instale os pacotes essenciais do sistema

![pacstrap](/imgs/pacstrap.gif)

`pacstrap /mnt/ base base-devel linux linux-firmware nano vim`

## Configurações iniciais

Gere o arquivo fstab

`genfstab -U /mnt >> /mnt/etc/fstab`

Mude para root

`arch-chroot /mnt`

Defina o fuso horário (use "ln /usr/share/zoneinfo/" para encontrar sua região e "ln /usr/share/zoneinfo/America/ para encontrar sua cidade")

Modelo no arch wiki

`ln -sf /usr/share/zoneinfo/Region/City /etc/localtime`

Exemplo prático

`ln -sf /usr/share/zoneinfo/America/Maceio /etc/localtime`

Para sincronizar o relógio com as informações da BIOS, se ela estiver correta, o seu relógio também estará:

`hwclock --systohc`

Para conferir se data e hora do sistema estão corretas:

`date`

Então vamos editar o arquivo de locales para dizer qual encode de caracteres vamos usar.

Configurando a localização

`nano /etc/locale.gen`

 Descomente a linha que vai usar(retirar o simbolo de # na frente):

en_US.UTF-8 UTF-8
pt_BR.UTF-8 UTF-8

Crie o arquivo locale.conf e defina a variável LANG adequadamente (localidade brasileira com mensagens em inglês):

`nano /etc/locale.conf`

Escreva:

LANG=pt_BR.UFT-8

Gere o "locale" executando:

`locale-gen`

Salve o layout do teclado usando:

`echo "KEYMAP=br-abnt2" >> /etc/vconsole.conf`

Configuração de rede

Crie o arquivo hostname

`echo "meu_host_name_que_criei" >> /etc/hostname`

Adicione as entradas correspondentes ao hosts

`nano /etc/hosts`

![hosts](imgs/hosts.png)

Defina a senha do root

`passwd`

Instale gerenciador de boot (desconsidere os-prober e ntfs-3g se não existir dual com windows)

`pacman -S grub efibootmgr os-prober ntfs-3g`

Configure o gerenciador de boot

`grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB`

`grub-mkconfig -o /boot/grub/grub.cfg`

Instale o networkmanager:

`pacman -S networkmanager`

Habilite-o

`systemctl enable NetworkManager`

Instale o driver proprietário (placas e processadores intel) e aplicativos para placa de som "ADVANCED LINUX SOUND ARCHITECTURE (ALSA)"

`pacman -S xf86-video-intel mesa pulseaudio alsa-utils`

## DESMONTE AS PARTIÇÕES E REINICIE

 exit
 umount -R /mnt
 poweroff

## Configuração de usuário

Crie usuário, pasta na partição /home e permissões especiais

`useradd -m -G audio,video,storage,wheel -s /bin/bash pessoa1`

`passwd pessoa1`

Permissão do sudo

`nano /etc/sudoers` e descomente `wheel (ALL) = ALL`

NetworkManager vem com `nmcli` e `nmtui`, mas o `nmcli` é o mais que suficiente para fazer a tarefa

Exemplos de nmcli

Lista redes wifi próximas:

`nmcli device wifi list`

Para conectar a uma rede wifi:

`nmcli device wifi connect NOME_da_REDE password SENHA_da_REDE`

### Interface gráfica

![interface gráfica](imgs/sidonia.gif)

Para preparar o ambiente das interfaces gráficas precisamos de um servidor de exibição, no caso Xorg

`sudo pacman -S xorg-server xorg-xinit xorg-apps mesa`

Depois do servidor de exibição, precisa-se do driver de vídeo

`sudo pacman -S xf86-video-intel` Intel

`sudo pacman -S nvidia nvidia-settings` Nvidia

`sudo pacman -S xf86-video-amdgpu` AMD

[KDE](kde.md)

[Gnome](gnome.md)

[Cinnamon](cinnamon.md)
