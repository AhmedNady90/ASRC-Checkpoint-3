
# Partie 1 : Gestion des utilisateurs
## Q.1.1.1 Créer l'utilisateur Lionel Lemarchand avec les même attribut de société que Kelly Rhameur.
Ouvrir Active Directory Users and Computers (ADUC).

Lance "Active Directory Users and Computers" depuis le serveur Windows.
Rechercher Kelly Rhameur.

Dans le volet de gauche, trouve le compte Kelly Rhameur dans l'OU où elle est stockée.
Copier les attributs.
![attributs Kelly](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/AttribuKelly.PNG)
Clique droit sur Kelly Rhameur et sélectionne Properties.

Allez dans l'onglet Organization et note les attributs associés à Kelly (ex : Département, Poste, etc.).
![attributs Kelly2](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/AttribuKelly2.PNG)
*Créer Lionel Lemarchand*
Clique droit dans l'OU où tu veux créer le nouvel utilisateur et sélectionne New > User.
Dans les champs de création, renseigne les informations de Lionel Lemarchand.
Dans les attributs Company, Department, etc., entre les mêmes valeurs que celles de Kelly Rhameur.
Attribuer un mot de passe et finir la création.

![attributs Lionel](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/AttributLion.PNG)

## Q.1.1.2 Créer une OU DeactivatedUsers et déplace le compte désactivé de Kelly Rhameur dedans.
Créer l'OU.

Dans Active Directory Users and Computers, fais un clic droit sur le domaine ou sur l'OU principale.
Sélectionne New > Organizational Unit (OU).
Nomme l'OU DeactivatedUsers.
Déplacer l'utilisateur Kelly Rhameur.

Trouve le compte Kelly Rhameur.
Clique droit sur Kelly Rhameur et sélectionne Move.
Déplace-le dans l'OU DeactivatedUsers.
Désactiver le compte.
![KellyDésactivé](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/kelly_DesactivatedUsersOU.PNG)
Clique droit sur Kelly Rhameur et choisis Disable Account pour désactiver son compte.
![KellyDisabled](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/kellyDisabled.PNG)
## Q.1.1.3 Modifier le groupe de l'OU dans laquelle était Kelly Rhameur en conséquence.
Vérifier l'OU d'origine.
ajoute Lionel Lemarchand aux groupes nécessaires, ou retire Kelly des groupes où elle était incluse.
![Kellyremoved Lioneladded](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/kellyRemoved%20LionelAdded.PNG)

## Q.1.1.4 Créer le dossier Individuel du nouvel utilisateur et archive celui de Kelly Rhameur en le suffixant par -ARCHIVE.

Créer un dossier pour Lionel Lemarchand.
Sur le serveur, crée un dossier pour Lionel Lemarchand sous C:\Users\
Archiver le dossier de Kelly Rhameur sous C:\Users\
![Lioneldossiercreated et Kelley archieved](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/kellyarchivedLionelCreated.PNG)
# Partie 2 : Restriction utilisateurs
## Q.1.2.1 Faire en sorte que l'utilisateur Gabriel Ghul ne puisse se connecter que du lundi au vendredi, de 7h à 17h.

Ouvrir l'utilisateur Gabriel Ghul.

Dans Active Directory Users and Computers, recherche Gabriel Ghul.
Configurer les restrictions d'accès.

Clique droit sur Gabriel Ghul > Properties > Account > Logon Hours.
Dans la fenêtre Logon Hours, bloque les heures pendant lesquelles Gabriel ne doit pas pouvoir se connecter. Sélectionne les heures de 7h à 17h, du lundi au vendredi.
![logonhours](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/GabrailLogonhours.PNG)
# Q.1.2.2 De même, bloquer sa connexion au seul ordinateur CLIENT01.
Configurer l'option Logon Workstations.
Dans les propriétés de l'utilisateur Gabriel Ghul, sous l'onglet Account, clique sur Logon Workstations.
Dans la fenêtre, entre le nom de l'ordinateur CLIENT01 pour permettre à Gabriel de se connecter uniquement à cet ordinateur.
![logoin PC](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/logon%20Client1.PNG)
# Q.1.2.3 Mettre en place une stratégie de mot de passe pour durcir les comptes des utilisateurs de l'OU LabUsers.
Créer une GPO pour l'OU LabUsers.

Dans Group Policy Management, fais un clic droit sur LabUsers et choisis Create a GPO in this domain, and Link it here.
Nomme la GPO, par exemple, LabUsers Password Policy.
Configurer les paramètres de sécurité du mot de passe.

Clique droit sur la GPO créée et sélectionne Edit.
Dans Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy.
Modifie les options de mot de passe pour durcir la politique.
![mot de pass strategie](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/motdepasse%20strategie.PNG)
# Partie 3 : Lecteurs réseaux
## Q.1.3.1 Créer une GPO Drive-Mount qui monte les lecteurs E: et F: sur les clients.
Créer une GPO pour monter les lecteurs réseaux.

Dans Group Policy Management, crée une nouvelle GPO nommée Drive-Mount.
Éditer la GPO pour ajouter les lecteurs.

Fais un clic droit sur la GPO Drive-Mount et sélectionne Edit.
Va dans User Configuration > Preferences > Windows Settings > Drive Maps.
Clique droit et choisis New > Mapped Drive.
Pour le lecteur E:, configure la lettre du lecteur et l’emplacement du partage réseau.
on Répète l’opération pour le lecteur F:.
![drivemapE et F](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/drivemap.PNG)


Appliquer la GPO aux utilisateurs ou ordinateurs.



![GPOlink to the whole domaine](https://github.com/AhmedNady90/ASRC-Checkpoint-3/blob/main/GPO%20link%20to%20the%20domain.PNG)

