# i3-gaps no Arch Linux

`i3-gaps` adiciona espaçamento entre janelas para um ricing mais moderno.

## Instalação

No Arch atual, boa parte da comunidade usa `i3-wm` + recursos equivalentes via configuração/pacotes auxiliares.
Se quiser uma experiência "i3 com gaps", normalmente usa-se:

```bash
sudo pacman -S i3-wm i3status dmenu picom feh alacritty
```

> Caso seu repositório/comunidade disponibilize pacote separado de i3-gaps, adapte conforme sua base.

## Exemplo de ricing (trecho de config)

```conf
# ~/.config/i3/config
smart_gaps on
gaps inner 10
gaps outer 6
```

## Stack comum de personalização

- Barra: `polybar` ou i3bar
- Launcher: `rofi`
- Compositor: `picom`
- Wallpaper: `feh`
- Notificações: `dunst`

## Dicas para não quebrar o setup

- Faça backup da config antes de mudanças grandes.
- Altere um bloco por vez e recarregue com `Mod+Shift+r`.
- Versione `~/.config` com Git para rollback rápido.
