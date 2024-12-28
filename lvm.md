# Gestion du stockage avec LVM sur Linux

## Étape 1 : Création d'une partition LVM sur un nouveau disque

root@vbox:~# fdisk /dev/sdb


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

Commande (m pour l'aide) : w

## Étape 2 : Initialiser la partition pour LVM
root@vbox:~# pvcreate /dev/sdb1
  Physical volume "/dev/sdb1" successfully created.
  
## Étape 3 : Ajouter le PV au groupe de volumes existant

root@vbox:~# vgextend vbox-vg /dev/sdb1
  Volume group "vbox-vg" successfully extended

### vérification:

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
 
 ## Étape 4 : Créer un snapshot du volume logique /home  

root@vbox:~# lvcreate --size 1G --snapshot --name home_snap /dev/vbox-vg/home
  Logical volume "home_snap" created.

### vérification:

root@vbox:~# lvs
  LV        VG      Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  home      vbox-vg owi-aos--- <11,76g                                                    
  home_snap vbox-vg swi-a-s---   1,00g      home   0,01                                   
  root      vbox-vg -wi-ao----  <6,81g                                                    
  swap_1    vbox-vg -wi-ao---- 976,00m

  ## Étape 5 : Monter le snapshot et vérifier son contenu

root@vbox:~# mkdir /home-snap
root@vbox:~# mount /dev/vbox-vg/home_snap /home-snap
root@vbox:~# ls /home
lost+found  wilder
root@vbox:~# ls /home-snap
lost+found  wilder

## Étape 6 : Travailler sur le snapshot
 
 root@vbox:~# touch /home-snap/test_file

## Étape 7 : Démonter et supprimer le snapshot

root@vbox:~# umount /home-snap

root@vbox:~# lvremove /dev/vbox-vg/home_snap
Do you really want to remove active logical volume vbox-vg/home_snap? [y/n]: y
  Logical volume "home_snap" successfully removed.

### vérification

root@vbox:~# lvs
  LV     VG      Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
   home   vbox-vg -wi-ao---- <11,76g                                                    
  root   vbox-vg -wi-ao----  <6,81g                                                    
  swap_1 vbox-vg -wi-ao---- 976,00m                                                    
root@vbox:~# 

