# GNOME no Arch Linux

Guia rápido para instalar GNOME com GDM.

## Instalação

```bash
sudo pacman -S gnome gdm
sudo systemctl enable gdm
```

> Reinicie e entre normalmente pela sessão GNOME.

## Pacotes úteis

```bash
sudo pacman -S gnome-tweaks gnome-browser-connector
```

## Dicas de ricing (sem exagero)

- Use `gnome-tweaks` para fontes, tema e comportamento do shell.
- Extensões recomendadas: Dash to Dock, Blur my Shell (opcional), Clipboard Indicator.
- Fontes boas para interface/código: `noto-fonts` e `ttf-jetbrains-mono`.
- Mantenha poucos tweaks para evitar quebra em upgrades.

## Ajustes recomendados

- Configure backups com Déjà Dup.
- Ative fração de escala se necessário (monitores HiDPI).
