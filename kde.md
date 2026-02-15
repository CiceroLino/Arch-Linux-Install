# KDE Plasma no Arch Linux

Guia rápido para instalar e deixar o KDE Plasma pronto para uso no Arch.

## Instalação

```bash
sudo pacman -S plasma-meta kde-applications sddm
sudo systemctl enable sddm
```

> Reinicie e selecione a sessão Plasma no login.

## Pacotes úteis

```bash
sudo pacman -S konsole dolphin ark spectacle kate
```

## Dicas de ricing (leve e elegante)

- Tema global: **Breeze Dark** (base estável).
- Fontes: `noto-fonts`, `ttf-jetbrains-mono`.
- Ícones: `papirus-icon-theme`.
- Transparência/sombras: ajuste em *Configurações > Comportamento da Área de Trabalho*.
- Painel minimalista: só lançador, tarefas, rede, volume e relógio.

## Ajustes recomendados

- Ative Night Color.
- Ative firewall (`ufw`) se for desktop exposto.
- Em notebooks, prefira perfil de energia equilibrado.
