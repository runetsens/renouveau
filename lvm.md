root@vbox:~# fdisk /dev/sdb

Bienvenue dans fdisk (util-linux 2.38.1).
Les modifications resteront en mémoire jusqu'à écriture.
Soyez prudent avant d'utiliser la commande d'écriture.

Le périphérique ne contient pas de table de partitions reconnue.
Created a new DOS (MBR) disklabel with disk identifier 0xe817407c.

Commande (m pour l'aide) : t
Aucune partition n'a encore été définie !

Commande (m pour l'aide) : n
Type de partition
   p   primaire (0 primaire, 0 étendue, 4 libre)
   e   étendue (conteneur pour partitions logiques)
Sélectionnez (p par défaut) : e
Numéro de partition (1-4, 1 par défaut) : 
Premier secteur (2048-41943039, 2048 par défaut) : 
Dernier secteur, +/-secteurs ou +/-taille{K,M,G,T,P} (2048-41943039, 41943039 par défaut) : 

Une nouvelle partition 1 de type « Extended » et de taille 20 GiB a été créée.

Commande (m pour l'aide) : t
Partition 1 sélectionnée
Code Hexa ou synonyme (taper L pour afficher tous les codes) :8e
Type de partition « Extended » modifié en « Linux LVM ».

Commande (m pour l'aide) : p
Disque /dev/sdb : 20 GiB, 21474836480 octets, 41943040 secteurs
Modèle de disque : VBOX HARDDISK   
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d'E/S (minimale / optimale) : 512 octets / 512 octets
Type d'étiquette de disque : dos
Identifiant de disque : 0xe817407c

Périphérique Amorçage Début      Fin Secteurs Taille Id Type
/dev/sdb1              2048 41943039 41940992    20G 8e LVM Linux

Commande (m pour l'aide) : w
La table de partitions a été altérée.
Appel d'ioctl() pour relire la table de partitions.
Synchronisation des disques.

root@vbox:~# pvcreate /dev/sdb1
  Physical volume "/dev/sdb1" successfully created.
root@vbox:~# vgextend vbox-vg /dev/sdb1
  Volume group "vbox-vg" successfully extended
root@vbox:~# vgdisplay
  --- Volume group ---
  VG Name               vbox-vg
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  5
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                3
  Open LV               3
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               <39,52 GiB
  PE Size               4,00 MiB
  Total PE              10116
  Alloc PE / Size       4997 / <19,52 GiB
  Free  PE / Size       5119 / <20,00 GiB
  VG UUID               TPYQWA-AmtO-Klga-Eakl-u2rd-wEYu-RY88C1
   
root@vbox:~# lvcreate --size 1G --snapshot --name home_snap /dev/vbox-vg/home
  Logical volume "home_snap" created.
root@vbox:~# lvs
  LV        VG      Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  home      vbox-vg owi-aos--- <11,76g                                                    
  home_snap vbox-vg swi-a-s---   1,00g      home   0,01                                   
  root      vbox-vg -wi-ao----  <6,81g                                                    
  swap_1    vbox-vg -wi-ao---- 976,00m                                                    
root@vbox:~# mkdir /home-snap
root@vbox:~# mount /dev/vbox-vg/home_snap /home-snap
root@vbox:~# ls /home
lost+found  wilder
root@vbox:~# ls /home-snap
lost+found  wilder
root@vbox:~# su wilder
wilder@vbox:/root$ ls /home
lost+found  wilder
wilder@vbox:/root$ sudo touch /home-snap/test_file
[sudo] Mot de passe de wilder : 
wilder n'est pas dans le fichier sudoers.
wilder@vbox:/root$ su -
Mot de passe : 
root@vbox:~# touch /home-snap/test_file
root@vbox:~# umount /home-snap
root@vbox:~# lvremove /dev/vbox-vg/home_snap
Do you really want to remove active logical volume vbox-vg/home_snap? [y/n]: y
  Logical volume "home_snap" successfully removed.
root@vbox:~# lvs
  LV     VG      Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  home   vbox-vg -wi-ao---- <11,76g                                                    
  root   vbox-vg -wi-ao----  <6,81g                                                    
  swap_1 vbox-vg -wi-ao---- 976,00m                                                    
root@vbox:~# 

