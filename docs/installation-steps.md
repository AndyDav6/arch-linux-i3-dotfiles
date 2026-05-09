# Guia de instalacion de Arch para PC de bajos recursos
Esta guía documenta el proceso que seguí para instalar Arch Linux en una computadora de recursos limitados, basado en la [Arch Wiki oficial](https://wiki.archlinux.org/title/Installation_guide).

## Hardware objetivo
- Procesador: Intel(R) Pentium(R) G3250 (2) @ 3.20GHz
- RAM: 3.74 de RAM
- Disco: SATA 1TB de almacenamiento

## Resumen del proceso

### 1. Preinstalación
- Verificar conexión a internet: `ping -c 2 google.com`
- Configurar teclado: `loadkeys es` (español)
- Verificar modo UEFI/BIOS: `ls /sys/firmware/efi/efivars` (vacío = BIOS)

### 2. Particionado (BIOS)
```bash
fdisk -l /dev/sda
cfdisk /dev/sda
```

Estructura aplicada:
| Particion | Tamaño | Tipo |
|:-------------------------:|
| /dev/sda1 | 16G    | SWAP |
|:-------------------------:|
| /dev/sda2 | 200G   | ext4 (/) |
|:-------------------------:|
| /dev/sda3 | 704G   | ext4 (/home) |
|:-------------------------:|

### 3. Formateo y particiones
```bash
mkfs.ext4 /dev/sda2
mkfs.ext4 /dev/sda3
mkswap /dev/sda1

mount /dev/sda2 /mnt
mkdir -p /mnt/home
mount /dev/sda3 /mnt/home
swapon /dev/sda1
```

### 4. Instalación base del sistema
```bash
pacstrap -K /mnt base base-devel linux linux-firmware sudo nano vim networkmanager
```

### 5. Generar fstab
```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

### 6. Chroot al sistema
```bash
arch-chroot /mnt
```

### 7. Configuración regional
```bash
ln -sf /usr/share/zoneinfo/America/Guayaquil /etc/localtime
hwclock --systohc

nano /etc/locale.gen  # Descomentar es_EC.UTF-8 y en_US.UTF-8
locale-gen

echo "LANG=es_EC.UTF-8" > /etc/locale.conf
```

### 8. Red y hostname
```bash
echo "arch-i3-pc" > /etc/hostname

cat > /etc/hosts << EOF
127.0.0.1   localhost
::1         localhost
127.0.1.1   arch-i3-pc.localdomain arch-i3-pc
EOF

systemctl enable NetworkManager
```

### 9. Usuario y contraseñas
```bash
passwd  # root
useradd -m -G wheel,audio,video -s /bin/bash and
passwd and

EDITOR=nano visudo  # Descomentar: %wheel ALL=(ALL:ALL) ALL
```

### 10. Entorno gráfico
```bash
# Xorg y controladores Intel
pacman -S xorg mesa vulkan-intel intel-media-driver

# KDE Plasma
pacman -S plasma-meta konsole dolphin

# Gestor de pantalla (compartido)
pacman -s lightdm
systemctl enable lightdm
```

### 11. Bootloader (GRUB para BIOS)
```bash
pacman -S grub
grub-install --target=i386-pc /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

### 12. Finalizar
```bash
exit
umount -R /mnt
reboot
```
