# Pasos de instalación de Arch

## 1. Verificar si hay conexión a Internet
```bash
ping -c 2 google.com
```

## 2. Cargar el keyboard o el idioma del teclado
```bash
loadkeys es
loadkeys la-latin1
```

## 3. Crear las particiones
```bash
fsdisk -l /dev/sda
cfdisk /dev/sda
```

## 4. Formatear las particiones
```bash
mkfs.ext4 /dev/sda2
mkfs.ext4 /dev/sda3
mkswap /dev/sda1
```

## 5. Montaje de las particiones
```bash
mount /dev/sda2 /mnt

mkdir -p /mnt/home
mount /dev/sda3 /mnt/home

swapon /dev/sda1

lsblk
```

## 6. Instalacion base del sistema
```bash
pacstrab -K /mnt base base-devel linux linux-firmware sudo nano vim nvim wget which
```

## 7. Generar el fstab
```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

Debemos
```bash
cat /mnt/etc/fstab
```


## 8. Entrar al sistema
```bash
arch-root /mnt
```

## 9. Configurar el sistema
```bash
ln -sf /usr/share/zoneinfo/America/guayaquil /etc/localtime
hwclock --systohc

echo "es_EC.UTF-8 UTF-8" >> /etc/locale.gen
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
# o
nano /etc/locale.gen
locale-gen

# Hostname
echo "and" > /etc/hostname
#  o
nano /etc/hostname

# Hosts
cat > /etc/hosts << EOF
127.0.0.1   localhost
::1         localhost
127.0.1.1   and.localdomain and
EOF
# o 
nano /etc/hosts
```

## 10. Creacion de la contraseña del root y crear el usuario
```bash
passwd

# Creamos un usuario
useradd -m -G wheel,audio,video -s /bin/bash and 
passwd and

# Configurar sudo, descomentamos esta línea: %wheel ALL=(ALL:ALL) ALL
# Para buscar mas rapido usemos ctrl + w
EDITOR=nano visudo

# Red
pacman -S networkmanager
systemctl enable --now NetworkManager
```

## 11. Instalar el entorno de escritorio
```bash
# Instalamos Xorg y KDE
pacman -S xorg plasma-meta konsole dolphin plasma-nm kscreen

# Instalamos el gestor de sesión
pacman -S lightdm
systemctl enable lightdm

# Instalar los Drivers Intel
pacman -S mesa vulkan-intel intel-media-driver

# Es importante dependiendo de que escritorio queramos tener en nuestro Arch
# debemos ir a su pagina y leer su documentacion
```

## 12. Instalar el bootloader **(Grub)**
```bash
pacman -S grub os-prober

# Instalamos el GRUB  y ajustamos /dev/sda al disco, sin número.
grub-install --target=i386-pc /dev/sda

# Generamos la configuración
grub-mkconfig -o /boot/grub/grub.cfg
```

## 13. Salir y reiniciar
```bash
exit  # Salir del chroot
umount -R /mnt # Desmontamos
reboot # Reiniciamos
```
