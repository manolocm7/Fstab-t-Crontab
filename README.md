# Fstab-y-Crontab
1.FSTAB

1.Crear particiones al primer disco duro

Comprobamos los discos que tenemos conectados con el siguiente comando:

sudo fdisk -l
administrador@ubuntu:~$ sudo fdisk -l

Disk /dev/sda: 8 GiB, 8589934592 bytes, 16777216 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x84ddb02c

Disposit.  Inicio   Start    Final Sectores  Size Id Tipo
/dev/sda1  *         2048   999423   997376  487M 83 Linux
/dev/sda2         1001470 16775167 15773698  7,5G  5 Extendida
/dev/sda5         1001472 16775167 15773696  7,5G 8e Linux LVM


Disk /dev/sdb: 2 GiB, 2147483648 bytes, 4194304 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xa2bb358b


Disk /dev/sdc: 3 GiB, 3221225472 bytes, 6291456 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xe1fe700b


Disk /dev/mapper/ubuntu--vg-root: 6,5 GiB, 7000293376 bytes, 13672448 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/ubuntu--vg-swap_1: 1 GiB, 1073741824 bytes, 2097152 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes



Accedemos al disco duro que se desa crear la particiones:
sudo fdisk /dev/sdb

Una vez estemos aquí creamos la tabla de particiones pulsando la "o"
Una vez creada la tabla de particiones crearemos las particiones con la tecla n
Pulsaremos la p para indicar particion primaria
Seleccionamos la primera particion con el 1
Pulsamos intro para que coja automaticamente el sector de inicio


Introducimos hasta que sector quiere la particion en mi caso 2050000 es aproximadamente 1 GB
Creamos otra particion con la tecla n
Pulsaremos la p para indicar particion primaria
Seleccionamos la segunda particion con el 2
Pulsamos intro para que coja automaticamente el sector de inicio
Pulsamos intro para que coja lo que queda deL disco
Ahora vamos ha cambiar el tipo de la 2 particion a FAT32 para ello pulsamos t


Seleccionamos la particion 2 pulsando la tecla 2
Por defecto el tipo de particion es linux por eso no modificamos el primero
podemos mostrar todos los tipos de particiones que hay con la tecla L


En el caso de escoger fat32 introducimos b
Nos salimos guardamos y escribimos los datos introduciendo la w

Para completar el formato de la primera particion introducimos:
sudo mkfs.ext4 /dev/sdb1


Para completar el formato de la segunda particion introducimos:
sudo mkfs.fat /dev/sdb2

1.Crear particiones al segundo disco duro

Comprobamos los discos que tenemos conectados con el siguiente comando:
sudo fdisk -l

Accedemos al disco duro que se desa crear la particiones:
sudo fdisk /dev/sdc

Una vez estemos aquí creamos la tabla de particiones pulsando la o

Una vez creada la tabla de particiones crearemos las particiones con la tecla n

Pulsaremos la p para indicar particion primaria
Seleccionamos la primera particion con el 1
Pulsamos intro para que coja automaticamente el sector de inicio
Introducimos hasta que sector quiere la particion en mi caso 2050000 es aproximadamente 1 GB
Creamos otra particion con la tecla n

Pulsaremos la p para indicar particion primaria
Seleccionamos la segunda particion con el 2
Pulsamos intro para que coja automaticamente el sector de inicio
Introducimos hasta que sector quiere la particion en mi caso 4100000 es aproximadamente OTRO 1 GB
Creamos otra particion con la tecla n

Pulsaremos la p para indicar particion primaria
Seleccionamos la tercera particion con el 3
Pulsamos intro para que coja automaticamente el sector de inicio
Pulsamos intro para que coja lo que queda deL disco
Ahora vamos ha cambiar el tipo de la 2 particion a NTFS para ello pulsamos t

Seleccionamos la particion 2 pulsando la tecla 2
podemos mostrar todos los tipos de particiones que hay con la tecla L
En el caso de escoger NTFS introducimos 7
Ahora vamos ha cambiar el tipo de la 3 particion a FAT32 para ello pulsamos t

Seleccionamos la particion 3 pulsando la tecla 3
podemos mostrar todos los tipos de particiones que hay con la tecla L
En el caso de escoger fat32 introducimos b
Nos salimos guardamos y escribimos los datos introduciendo la w

Para completar el formato de la primera particion introducimos:
sudo mkfs.ext4 /dev/sdc1

Para completar el formato de la segunda particion introducimos:
sudo mkfs.ntfs /dev/sdc2

Para completar el formato de la tercera particion introducimos:
sudo mkfs.fat /dev/sdc3

2 Crear montaje de los discos duros

Para ello vamos a configurar el fichero:
sudo nano /etc/fstab
/etc/fstab: static file system information.

Use 'blkid' to print the universally unique identifier for a
device; this may be used with UUID= as a more robust way to name devices
that works even if disks are added and removed. See fstab(5).

<file system> <mount point>   <type>  <options>       <dump>  <pass>
/dev/mapper/ubuntu--vg-root /               ext4    errors=remount-ro 0       1
/boot was on /dev/sda1 during installation
UUID=c29a4022-e6c4-429c-84e5-243099e9b66f /boot           ext2    defaults        0       2
/dev/mapper/ubuntu--vg-swap_1 none            swap    sw              0       0
Disco 2
UUID=c3e52e7b-dd1a-45d5-bbef-492582f2cedc       /media/sdb1     ext4    defaults,noauto         0       0
UUID=9BED-B5F0  /media/sdb2     vfat    defaults,noauto 0       0

Disco 3
/dev/sdc1       /media/sdc1     ext4    defaults        0       0
/dev/sdc2       /media/sdc2     ntfs    defaults        0       0
/dev/sdc3       /media/sdc3     vfat    defaults        0       0


Para montar automaticamente las particiones podemos reiniciar sudo reboot o con este comando sudo mount -a.

Para montar manualmente usaremos el siguiente comando sudo mount /dev/sdb1 /media/sdb1 y para la segunda particion sudo mount /dev/sdb2 /media/sdb2 Crear Las carpetas donde deseais montarlos con mkdir.

Para ver donde estan montados o la uuid de las particiones y si estan montadas o no con el siguiente comando
sudo blkid -o list.

2.CRONTAB

Accedemos la fichero mediante este comando
sudo nano /etc/crontab

Parametros del crontab
m: Minutos del 0 a 59
h: Horas del 0 a 23
dom: Día del mes del 1 a 31
mon: Mes del 1 a 12
dow: Día de la semana del 0 a 6, siendo 0 el domingo.

Si se deja un asterisco, quiere decir "cada" minuto, hora, día de mes, mes o día de la semana.
user: El usuario root ejecuta este comando.
command: Instrucciones a realizar al activarse
Una vez editado el fichero lo reiniciaremos con sudo /etc/init.d/cron restart o sudo /etc/init.d/cron reload
Cada hora en punto ejecutamos la sincronización horaria y mandamos la salida a /dev/null/


Instalamos ntp sudo apt-get install ntp
00 * * * * root ntpdate -u hora.rediris.es >> /dev/null

Programar un trabajo (A) para ejecutarse en el minuto 30 de cada hora de cada día.
30 * * * * root echo "A" >> /etc/trabajo ; date +%H:%M >> /etc/trabajo

Programar un trabajo (B) para ejecutarse cada día a las 20:30h.
30 20 * * * root echo "B" >> /etc/trabajo ; date +%H:%M >> /etc/trabajo

Programar un trabajo (C) para ejecutarse de lunes a viernes a las 20:30h.
30 20 * * 1,2,3,4,5 root echo "C" >> /etc/trabajo ; date +%H:%M >> /etc/trabajo

Programar un trabajo (D) para ejecutarse los martes y los jueves a las 20:30h.
30 20 * * 2,4 root echo "D" >> /etc/trabajo ; date +%H:%M >> /etc/trabajo

Programar un trabajo (E) para ejecutarse los días 10 y 20 de todos los meses a las 20:30h.
30 20 10,20 * * root echo "E" >> /etc/trabajo ; date +%H:%M >> /etc/trabajo

Programar un trabajo (F) para ejecutarse cada 15 minutos.
00,15,30,45 * * * * root echo "F" >> /etc/trabajo ; date +%H:%M >> /etc/trabajo

Programar un trabajo (G) para ejecutarse cada día a las 00:00h.
00 00 * * * root echo "G" >> /etc/trabajo ; date +%H:%M >> /etc/trabajo

Programar un trabajo (H) para ejecutarse cada primer día de mes a las 00:00h.
00 00 1 * * root echo "H" >> /etc/trabajo ; date +%H:%M >> /etc/trabajo

El trabajo que se debe ejecutar es:
Añadir al fichero /etc/trabajos (no existe hay que crearlo) el código del trabajo y la hora de ejecución.
