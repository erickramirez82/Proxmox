# Proxmox

Proxmox Virtual Environment, es un proyecto de código abierto, desarrollado y mantenido por Proxmox Server Solutions GmbH y el apoyo financiero de Internet Foundation Austria (IPA). Una completa plataforma de virtualización basada en sistemas de código abierto que permite la virtualización tanto sobre OpenVZ como KVM.
Es una distribución bare-metal, basada en Debian con solo los servicios básicos para de esta forma obtener un mejor rendimiento.

Proxmox, no es solo una maquina virtual más, con una interfaz gráfica muy sencilla esta herramienta permite la migración en vivo de maquinas virtuales, clustering de servidores, backups automáticos y conexión a un NAS/SAN con NFS, iSCSI, etc…

Al utilizar OpenVZ se puede cambiar tanto memoria RAM como espacio en disco asignados, en tiempo real y sin reiniciar el sistema. Otra cosa muy interesante son las plantillas, que consisten en un sistema operativo con algún software preinstalado, que se descargan directamente desde la interfaz de administración y crear una máquina virtual a partir de ellas.

<img src="https://github.com/AgenciaImplementacion/Proxmox/blob/master/proxmox.png?raw=true" />

## Lista de entradas para a configuración de Proxmox
- Configuración de [Cluster](Cluster.md)
- Eliminación configuración [Cluster](eleminar_cluster.md)
