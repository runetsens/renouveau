# Partie 1 : Gestion des utilisateurs
Q.2.1.1 Sur le serveur, créer un compte pour ton usage personnel
![Capture d’écran du 2025-01-17 09-34-12](https://github.com/user-attachments/assets/9257457a-8e60-4b39-a294-d8408da35297)
Q.2.1.2 Quelles préconisations proposes-tu concernant ce compte ?
creer un mot de passe robuste avec 12 caractere  comprenant majuscule,chiffre et symbole

# Partie 2 : Configuration de SSH
Q.2.2.1 Désactiver complètement l'accès à distance de l'utilisateur root.
![Capture d’écran du 2025-01-17 09-47-17](https://github.com/user-attachments/assets/37625d92-1ec3-441e-a7c9-7bdedcd4b2fe)

Q.2.2.2 Autoriser l'accès à distance à ton compte personnel uniquement.
![Capture d’écran du 2025-01-17 09-52-10](https://github.com/user-attachments/assets/9f5874d7-12a6-4105-b4ac-b3256ab46dbf)

Q.2.2.3 Mettre en place une authentification par clé valide et désactiver l'authentification par mot de passe
![Capture d’écran du 2025-01-17 09-55-29](https://github.com/user-attachments/assets/6e37749d-59ff-4199-8be8-f5732fc53dc7)
![Capture d’écran du 2025-01-17 09-55-11](https://github.com/user-attachments/assets/5dd796d7-d9ff-47d5-a982-842cf2797f18)
sur le pc qui veut accedé au serveur il faut creer une clé public et privé
![Capture d’écran du 2025-01-17 09-59-00](https://github.com/user-attachments/assets/bb61dc38-4345-49a1-bf60-26bb5e14296d)

# Partie 3 : Analyse du stockage

Q.2.3.1 Quels sont les systèmes de fichiers actuellement montés ?
![Capture d’écran du 2025-01-17 10-07-34](https://github.com/user-attachments/assets/efd25532-3bb0-4333-bf78-d0ad79869d6b)


Q.2.3.2 Quel type de système de stockage ils utilisent ?
raid
Q.2.3.3 Ajouter un nouveau disque de 8,00 Gio au serveur et réparer le volume RAID

Q.2.3.4 Ajouter un nouveau volume logique LVM de 2 Gio qui servira à héberger des sauvegardes. Ce volume doit être monté automatiquement à chaque démarrage dans l'emplacement par défaut : /var/lib/bareos/storage.

Q.2.3.5 Combien d'espace disponible reste-t-il dans le groupe de volume ?

# Partie 4 : Sauvegardes

Le logiciel bareos est installé sur le serveur.
Les composants bareos-dir, bareos-sd et bareos-fd sont installés avec une configuration par défaut.

Q.2.4.1 Expliquer succinctement les rôles respectifs des 3 composants bareos installés sur la VM.

bareos dir:Bareos Director
C'est le chef d'orchestre. Il est responsable de la planification, du contrôle et du lancement des tâches de sauvegardes. Il contrôle l'ensemble des autres composants. Il est installé sur le serveur en charge de la gestion des sauvegardes.

bareos sd :Bareos Storage Daemon
Bareos permet d'effectuer des sauvegardes sur différents types de supports. L'écriture sur ces supports est effectué par un Storage Daemon.

 
 bareos-fd :Bareos File Daemon
composant installé sur chaque machine devant être sauvegardée.
en charge de collecter les informations à sauvegarder et de les envoyer au Bareos Storage Daemon


 
# Partie 5 : Filtrage et analyse réseau

Q.2.5.1 Quelles sont actuellement les règles appliquées sur Netfilter ?
![Capture d’écran du 2025-01-17 10-31-06](https://github.com/user-attachments/assets/f9fcf0a6-af7b-4c64-85c3-97c54970add1)


Q.2.5.2 Quels types de communications sont autorisées ?
autorise le trafic local
autorise le ping icmp en ipv4 et ipv6
autorise les connexion ssh

Q.2.5.3 Quels types sont interdit ?
tout est bloqué

Q.2.5.4 Sur nftables, ajouter les règles nécessaires pour autoriser bareos à communiquer avec les clients bareos potentiellement présents sur l'ensemble des machines du réseau local sur lequel se trouve le serveur.

Rappel : Bareos utilise les ports TCP 9101 à 9103 pour la communication entre ses différents composants.

# Partie 6 : Analyse de logs

Q.2.6.1 Lister les 10 derniers échecs de connexion ayant eu lieu sur le serveur en indiquant pour chacun :

    La date et l'heure de la tentative
    L'adresse IP de la machine ayant fait la tentative

    ![Capture d’écran du 2025-01-17 10-59-21](https://github.com/user-attachments/assets/ec716068-dbd9-4b4f-974b-0f409ad41bd9)

