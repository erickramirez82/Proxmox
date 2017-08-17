
# Eliminar toda la configuración del cluster

## Realizamos un backup de la configuración del cluster

```
# backup
cp -va /etc/pve /root
cp -va /etc/corosync/ /root/
cp -va /var/lib/corosync/ /root/
cp -va /etc/pve/nodes/ /root/
cp -va /etc/pve/nodes/ /root/
cp -va /var/lib/pve-cluster/ /root/
```
## Paramos los siguientes servicios
```
systemctl stop pvestatd.service
systemctl stop pvedaemon.service
systemctl stop pve-cluster.service
systemctl stop corosync
systemctl stop pve-cluster
```
## Ahora procedemos a borrar los archivos donde estan las configuraciones
```
pmxcfs -l
rm /etc/pve/corosync.conf
rm /etc/corosync/*
rm /var/lib/corosync/*
rm -rf /etc/pve/nodes/*
sqlite3 /var/lib/pve-cluster/config.db "select * from tree where name='corosync.conf'"
sqlite3 /var/lib/pve-cluster/config.db "delete from tree where name='corosync.conf'"
sqlite3 /var/lib/pve-cluster/config.db "select * from tree where name='corosync.conf'"
```
## Y como ultimo procedemos a reiniciar

```
reboot
```

## Solución de problemas de SSH

La adición de nodos funciona mejor con keyauth. En caso de que haya reinstalado un nodo o algo así, trate de conectarse a través de ssh desde el host en cuestión a su 'primer' hv.

Ya que los hosts conocidos se almacenan en /etc/ssh/ssh_known_hosts, no ~/.ssh/known_hosts:

```
# in case you have trouble on a certain host
> /root/.ssh/known_hosts
> /etc/ssh/ssh_known_hosts
ssh-copy-id FIRST_HV
```

## Podemos volver repetir los siguiente pasos.

- Configuración de [Cluster](Cluster.md)


