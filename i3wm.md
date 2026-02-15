# i3wm no Arch Linux

i3wm é um gerenciador de janelas tiling, rápido e altamente configurável.

## Instalação

```bash
sudo pacman -S i3-wm i3status dmenu picom feh alacritty
```

Opcional para login gráfico:

```bash
sudo pacman -S lightdm lightdm-gtk-greeter
sudo systemctl enable lightdm
```

## Arquivos importantes

- Config principal: `~/.config/i3/config`
- Status bar: `~/.config/i3status/config`

## Dicas de ricing

- Defina gaps visuais usando i3-gaps (ou mude para `i3gaps.md`).
- Combine com `picom` para transparência/sombra suave.
- Use `feh` para wallpaper e consistência visual.
- Organize workspaces por contexto: 1-web, 2-code, 3-term, 4-chat.

## Boas práticas

- Tenha um arquivo de atalhos comentado.
- Evite configs enormes sem versionamento.
