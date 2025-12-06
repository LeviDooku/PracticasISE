1. Crear sdb y sdc OK 

2. Formatearlos fdisk /dev/sdb y /dev/sdc

3. instalar mdadm con dnf install mdadm

4. Consultar manuan mdadm. Sirve para gestionar MultiDevices

5. mdam --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1 para el raid1

6. Comprobar que md0 ha sido creado con lsblk

7. Crear Physical Volume : pvs ; pvcreate /dev/md0 ; pvs

8. Crear Volume Group : vgs ; vgcreate vg_raid1 /dev/md0 ; vgs

9. Crear Logical Volume : lvs ; lvcreate -L <tam 1.9GB aprox> -n new_var vg_raid1 ; lvs

10. Cifrar Logical Volume (new_var) con cryptsetup : dnf install cryptsetup ; man cryptsetup ; cryptsetup luksFormat /dev/vg_raid1/new_var 

11. Activar volumen cifrado (decirle al kernel que tiene la contraseña y el Volumen y montar el sistema de archivos)
        a. cryptsetup luksOpen /dev/vg_raid1/new_var vg_raid1-new_var_crypt
        b. Introducir contraseña
        c. Comprobar en /dev/mapper

12. Crear sistema de archivos con mkfs
        a. mkfs -t xfs /dev/mapper/vg_raid1/vg_raid1-new_var_crypt

13. Montar el Volumen Lógico con mkdir 
        a. mkdir /mnt/new_var
        b. mount /dev/mapper/vg_raid1-new_var_crypt /mnt/new_var

14. Copiar información con systemctl y cp -a
        a. systemclt isolate rescue
        b. systemclt status
        c. cp -a /var/. /mnt/new_var
        d. ls -laZ /var

15. Editar el FS anterior
        a. nano /etc/fstab
        b. /dev/mapper/vg_raid1-new_var_crypt   /var    xfs     dafaults        0 0

16. crypttab
        a. blkid | grep LUKS >> /etc/crypttab ; Para redirigir a tabla de encriptado ; editar prefijo /dev/mapper ; 
                Especificar UUID sin comillas ; poner none al final para especificar ninguna opción al final ; 
                vg_raid1-new_var_crypt UUID=<UUID> none <---- Así tiene que quedar 

17. mv /var /var_old ; 

18. reboot ; pedirá contraseña ; iniciar sesión ; lsblk y comprobar que el diseño es correcto
