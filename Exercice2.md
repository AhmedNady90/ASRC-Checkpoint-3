
# Partie 1 : Gestion des utilisateurs
## Q.2.1.1 Sur le serveur, créer un compte pour ton usage personnel.
Créer un utilisateur : useradd -m trump
Attribuer un mot de passe : passwd trump

voici l'utilisateur crée trump


![Comptetrumpcreated](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/trumpCreated.PNG)
## Q.2.1.2 Quelles préconisations proposes-tu concernant ce compte ?
Utiliser un mot de passe fort 

utiliser l’authentification par clé SSH au lieu d'un mot de passe

ne pas donner de privilèges d'administrateur à ce compte

ajouter cet utilisateur à un groupe spécifique qui aura des permissions adaptées à ses besoins

# Partie 2 : Configuration de SSH
## Q.2.2.1 Désactiver complètement l'accès à distance de l'utilisateur root.
Modifier la configuration SSH

nano /etc/ssh/sshd_config

Désactiver l'accès SSH pour root 

PermitRootLogin no

Redémarrer le service SSH 

systemctl restart sshd
## Q.2.2.2 Autoriser l'accès à distance à ton compte personnel uniquement.
dans le fichier de configuration SSH, ajoute AllowUsers trump

Voir l'image founie

![sshconfig](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/SSHCONFIG.PNG)
## Q.2.2.3 Mettre en place une authentification par clé valide et désactiver l'authentification par mot de passe

Générer une paire de clés SSH sur vm local

ssh-keygen -t rsa -b 4096

Copier la clé publique sur le serveur

ssh-copy-id trump@172.16.211.50

Désactiver l'authentification par mot de passe 

PasswordAuthentication no

# Partie 3 : Analyse du stockage
## Q.2.3.1 Quels sont les systèmes de fichiers actuellement montés ?
Lister les systèmes de fichiers montés
df -h
![systèmes de fichiers montés](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/fichiermont%C3%A9s.PNG)
## Q.2.3.2 Quel type de système de stockage ils utilisent ?
le type de système de fichiers

lsblk -f

![type de système de fichiers](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/type%20de%20syst%C3%A8me%20de%20fichiers.PNG)
## Q.2.3.3 Ajouter un nouveau disque de 8,00 Gio au serveur et réparer le volume RAID
le disk a été ajouté et pour Vérifier la présence du disque

lsblk

Réparer le volume RAID

mdadm --add /dev/md0 /dev/sdb

![Réparer le volume RAID](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/raid%20reparer.PNG)
## Q.2.3.4 Ajouter un nouveau volume logique LVM de 2 Gio qui servira à héberger des sauvegardes. Ce volume doit être monté automatiquement à chaque démarrage dans l'emplacement par défaut : /var/lib/bareos/storage.
Créer un volume logique LVM : pvcreate /dev/sdb

Crée un groupe de volumes : vgcreate vg_sauvegarde /dev/sdX

Crée un volume logique (LV) de 2 Go : lvcreate -L 2G -n lv_sauvegarde vg_sauvegarde

Formate le volume logique : mkfs.ext4 /dev/vg_sauvegarde/lv_sauvegarde

Monter le volume : mount /dev/vg_sauvegarde/lv_sauvegarde /var/lib/bareos/storage

montage automatique : Ajoute : /dev/vg_sauvegarde/lv_sauvegarde /var/lib/bareos/storage ext4 defaults 0 0 une ligne dans le fichier /etc/fstab

![lvdisplay](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/lvdisplay.PNG)
![lvm verification](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/verficationlvm.PNG)

## Q.2.3.5 Combien d'espace disponible reste-t-il dans le groupe de volume ?
vgdisplay vg_sauvegarde 
![espace disponible](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/espace%20disponible.PNG)
# Partie 4 : Sauvegardes
## Q.2.4.1 Expliquer succinctement les rôles respectifs des 3 composants bareos installés sur la VM (Les composants bareos-dir, bareos-sd et bareos-fd sont installés avec une configuration par défaut).
*bareos-dir* :
Le Director est le composant qui gère les politiques et les plans de sauvegarde. Il contrôle toutes les opérations de sauvegarde et de restauration.
*bareos-sd* :
Le Storage Daemon gère les supports de sauvegarde, tels que les disques.
*bareos-fd* :
Le File Daemon est installé sur les machines clientes et est responsable de la sauvegarde des fichiers.
# Partie 5 : Filtrage et analyse réseau
## Q.2.5.1 Quelles sont actuellement les règles appliquées sur Netfilter ?
Pour afficher les règles de filtrage actuelles sur iptables, utilise  nft list ruleset
![les règles](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/nfr%C3%A8gles.PNG)
## Q.2.5.2 Quels types de communications sont autorisées ?
(voir l'image fournie ci-dessus![les règles](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/nfr%C3%A8gles.PNG))
Les connexions SSH (port 22)
Les connexions ICMP 
les paquets qui arrivent sur l'interface de loopback lo
## Q.2.5.3 Quels types sont interdit ?
le paquet marqué comme invalide par le suivi de connexion (connection tracking) (voir l'image fournie ci-dessus![les règles](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/nfr%C3%A8gles.PNG))
## Q.2.5.4 Sur nftables, ajouter les règles nécessaires pour autoriser bareos à communiquer avec les clients bareos potentiellement présents sur l'ensemble des machines du réseau local sur lequel se trouve le serveur.
*Autoriser Bareos Director (port 9101)* 
*Autoriser Bareos File Daemon (port 9102)* 
*Autoriser Bareos Storage Daemon (port 9103)* 
nft add rule inet inet_filter_table in_chain ip saddr 172.16.0.0/16 tcp dport { 9101, 9103 } accept
Vérifier les règles ajoutées : nft list ruleset
![RègleBareos](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/r%C3%A8gleBareos.PNG)
# Partie 6 : Analyse de logs
## Q.2.6.1 Lister les 10 derniers échecs de connexion ayant eu lieu sur le serveur en indiquant pour chacun : La date et l'heure de la tentative, L'adresse IP de la machine ayant fait la tentative
Utilise la commande suivante pour afficher les 10 derniers échecs de connexion :
sudo journalctl | grep "Failed password" | tail -n 10 | awk '{print $1, $2, $3, $11}'
![la sortie de logs](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/logs.PNG)
