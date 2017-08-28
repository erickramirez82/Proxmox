# Configuración Nfs en Proxmox

Este es un sub-tutorial de la configuración de un cluster de proxmox. 
Basado en https://www.howtoforge.com/tutorial/how-to-configure-a-proxmox-ve-4-multi-node-cluster/

El primer paso fue conectar físicamente el disco duro a la máquina que va a servir para almacenar los archivos. En este caso se puso un disco HDD de 4TB de espacio.

Es importante aclarar cómo es la arquitectura de un almacenamiento basado en LVM.

<img src="https://github.com/erickramirez82/Proxmox/blob/master/lvm-schema.png?raw=true" />

Extraído de: http://www.cpanelkb.net/lvm-configuration-in-linux-cpanel-server/

<img src="https://github.com/erickramirez82/Proxmox/blob/master/LinuxLVMvolume-virtualobjects.jpg?raw=true" />

Extraído de: http://www.unixarena.com/2013/08/linux-lvm-volume-creation-operation.html


## Instalación de NFS

### servidor:

En el servidor NFS ejecutamos:
```
apt-get install nfs-kernel-server nfs-common
```

A continuación, creamos los enlaces de inicio del sistema para el servidor NFS y lo iniciamos:

### cliente:

En el cliente podemos instalar NFS de la siguiente manera (esto es realmente lo mismo que en el servidor):
```
apt-get install nfs-common
```

## Añadir disco y crear particiones

Una vez instalado el disco físicamente y arrancado el sistema, accedemos a la terminar vía SSH o terminal (línea de comandos). Necesitaremos identificar nuestro nuevo disco (con usuario root o permisos suficientes):

```bash
fdisk -l 
Disk /dev/sdb: 931.5 GiB, 1000171331584 bytes, 1953459632 sectors
``` 
Vía: Añadir disco duro como unidad estándar al sistema
Nuestro disco duro adicional es “/dev/sdb” de 1tera. Para añadirlo como disco útil:

```bash
fdisk /dev/sdd
```
```
Opción n (añadir nueva partición)
Si sólo vamos a crear una partición, se dejan los valores por defecto
Opción t (cambiar tipo de partición)
Seleccionamos 8e (tipo LVM)
Opción w (grabar y salir)
```

El disco ya tiene partición definida. Ahora creamos el sistema de ficheros:

```bash
mkfs -t ext4 /dev/sdb
```

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

Ahora agregue todas las direcciones IP de proxmox al archivo de configuración NFS, editaré el archivo "exports" con vim:
```bash
vim / etc / exports
```

Pegue la configuración a continuación:

```
/Var/nfsbackup 192.168.1.114(rw,sync,no_root_squash) 
/var/nfsbackup 192.168.1.115(rw,sync,no_root_squash) 
/var/nfsbackup 192.168.1.116(rw,sync,no_root_squash)
```

Guarde el archivo y salga del editor.
Para activar la nueva configuración, vuelva a exportar el directorio NFS y asegúrese de que el directorio compartido está activo:

```bash
exportfs -r 
exportfs -v
```
Reiniciar(no necesario, solo por pruebas)

```bash
reboot
```


