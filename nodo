# Proxmox
Proxmox VE 4.x Cluster 

## Servidores

1 – xxx.xxx.xxx.11 (server1)
2 – xxx.xxx.xxx.12 (server2)

Modificamos el archivos hosts ambas maquinas
```bash
$ nano /etc/hosts
```
<b>Servidor server1</b> 
```
xxx.xxx.xxx.11 server1.local virtual pvelocalhost
xxx.xxx.xxx.12 server2.local nodo2
```
<b>Servidor server2</b> 
```
xxx.xxx.xxx.12 server2.local virtual pvelocalhost
xxx.xxx.xxx.11 server1.local nodo1
```
Guardamos archivo.

Comprabar quedo la configuración

```bash
$ ping nodo2
$ ping nodo1
```
<b>Creacción de cluster</b>

Asignamos el Cluster server1
```bash
$ pvecm create <--Nombre_Cluster-->
```

Verificamos el Status cluster
```bash
$ pvecm status
```
<b>Agregar Nodo</b>

Asignación de nodo al cluster vamos a server2
```bash
$ pvecm add server1
```
La respuesta successfuly added node ‘server2’ tu cluster

Verificamos el status 
```bash
$ pvecm status
```
