# Configuración Nfs en Proxmox

Este es un sub-tutorial de la configuración de un cluster de proxmox. Basado en https://www.howtoforge.com/tutorial/how-to-configure-a-proxmox-ve-4-multi-node-cluster/

El primer paso fue conectar físicamente el disco duro a la máquina que va a servir para almacenar los archivos. En este caso se puso un disco HDD de 4TB de espacio.

Es importante aclarar cómo es la arquitectura de un almacenamiento basado en LVM.

<img src="https://github.com/erickramirez82/Proxmox/blob/master/lvm-schema.png?raw=true" />

Extraído de: http://www.cpanelkb.net/lvm-configuration-in-linux-cpanel-server/

<img src="https://github.com/erickramirez82/Proxmox/blob/master/LinuxLVMvolume-virtualobjects.jpg?raw=true" />

Extraído de: http://www.unixarena.com/2013/08/linux-lvm-volume-creation-operation.html

Ver qué unidad se le asignó al nuevo disco con

```bash
fdisk -l
```

En este caso se montó en `/dev/sdb`, allí se hace una partición con todo el tamaño del disco

```bash
fdisk /dev/sdb
```

Una vez creado el `/dev/sdb1` se crea un volume group (vg)

```bash
vgcreate nfsbackupgroup /dev/sdb1
```

Crear un phisical volume con sdb1

```bash
pvcreate /dev/sdb1
```

Luego un logical volume (lv) en el pv

```bash
lvcreate -n nfsbackup -l 100%FREE nfsbackupgroup
```

Crear un directorio donde montar la partición

```bash
mkdir /var/nfsbackup
```

Probar que se puede montar exitosamente

```bash
mount /dev/nfsbackupgroup/nfsbackup /var/nfsbackup
```

Agregar la siguiente línea al archivo `/etc/fstab`

```bash
/dev/nfsbackupgroup/nfsbackup /var/nfsbackup ext4 defaults 0 0
```

Reiniciar(no necesario, solo por pruebas)

```bash
reboot
```
