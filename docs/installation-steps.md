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
