# XFCE no Arch Linux

XFCE é leve, estável e excelente para máquinas antigas ou foco em produtividade.

## Instalação

```bash
sudo pacman -S xfce4 xfce4-goodies lightdm lightdm-gtk-greeter
sudo systemctl enable lightdm
```

## Pacotes úteis

```bash
sudo pacman -S thunar-archive-plugin ristretto mousepad
```

## Dicas de ricing no XFCE

- Use tema GTK simples + ícones Papirus.
- Painel superior minimalista e dock inferior opcional.
- Compositor: desative efeitos pesados se quiser mais desempenho.
- Terminal com fonte mono (`ttf-jetbrains-mono`) e esquema escuro.

## Ajustes recomendados

- Configure atalhos globais (terminal, launcher, captura de tela).
- Revise serviços de sessão para iniciar só o necessário.
