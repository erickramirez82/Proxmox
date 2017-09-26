# Migrar Maquinas en Proxmox

## Migrar de Proxmox a Vmware o VirtualBox

Ejecutamos el siguiente comando en la consola  

```
qemu-img convert -f raw -O vmdk /dev/mapper/pve-vm--<id_maquina 000>--disk--1  /home/maquinas/vm-<id_maquina 000>-disk-1.vmdk
```

## Migrar de Vmware o VirtualBox a Proxmox

Ingresamos donde alojamos img de la maquina virtual.

```
dd if=image.img of=/dev/mapper/pve-vm--<id_maquina 000>--disk--1 bs=50M
```
