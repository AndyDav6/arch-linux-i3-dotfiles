# arch-linux-i3-dotfiles

# Arch Linux + i3wm en PC de bajos recursos

## ¿Por qué hice esto?
Revitalizar una computadora obsoleta con Intel Pentium G3250 y 4GB de RAM. Necesitaba un entorno gráfico funcional sin sacrificar rendimiento, porque ya no daba mas con Windows 10.

## Setup actual
- **Entorno principal**: i3-WM (ligero, eficiente)
- **Entorno secundario**: KDE Plasma (fallback, por si i3 falla)
- **Motivo**: i3 para el día a día, KDE para tareas gráficas pesadas o por si algo falla en i3.

## Recursos que optimicé
- RAM en idle con i3: 7500MB
- RAM en idle con KDE: 1190MB
- Tiempo de boot: ~45s (systemd-analyze)

## Configuraciones clave que hice
- [ ] Switch entre entornos desde el login manager con LightDM
- [ ] Teclas rápidas personalizadas para lanzar terminal en i3 alacritty, navegador, VS code.

## Comandos útiles que uso
```bash
# Actualizar el sistema
sudo pacman -Syu && paru -Syu && flatpak update

# Ver consumo de RAM
free -h

# Ver procesos pesados
htop

# Para mostrar información técnica detallada del sistema operativo, hardware y software en la termina
fastfetch

# Editor de codigo secundario
nvim 
vim
```
