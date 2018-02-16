# Cours SRW3

Henry Nicolas

SI-T1a

## Laboratoire 3

LABO : Configurer la sécurité du serveur Web sous IIS

## Exercice 1

- Créer un dossier "C:/SRW/labo".
- Créer les sous répertoires suivant : "C:/SRW/labo/private" "C:/SRW/labo/public".
- Dans le répertoire labo créez une page nommée labo.html
- Dans le répertoire Private créez une page nommée private.html
- Dans le répertoire Public créez une page nommée public.html
- Dans "Internet Information Services (IIS) Manager" créer un site "labo du Module SRW" pointant sur "C:/SRW/labo" et comme port 80.
- (Si un site possède déjà une adresse IP identique, supprimez le, il vous embêtera plus).
- Dans "Advanced Settings" dans la sidebar droite, changez l'ID en 41.
- Dans "Default Document" supprimez tout et ajoutez "labo.html".
- Allez dans "Directory Browsing" et activez le.
- Ensuite, dans votre site, déroulez le et selectionnez "private".
- Retournez dans "Default Document" et supprimez tout puis ajoutez "private.html".
- Effectuez la même action pour "public" mais le fichier se nommera "public.html".

### Tests configuration

- Allez sur votre machine et essayez de vous connecter avec l'adresse IP de votre serveur ("192.168.106.137" pour moi).
- Essayez d'accèder à "http://nomdusite:80"
- Essayez d'accèder à "http://nomdusite"
- Essayez d'accèder à "http://nomdusite/public"
- Essayez d'accèder à "http://nomdusite/private"

<div style="page-break-after: always;"></div>

## Exercice 2

- Allez dans le "Server Manager" puis ajouter un rôle dans "Web Server (IIS).
- Ajoutez le rôle "IP and Domain Restrictions".
- Installez le.
- Dans le "Internet Information Services (IIS) Manager" allez dans "IP Address and Domain Restrictions" de votre site.
- Ajoutez une restriction pour votre IP "192.168.106.1" pour moi.
- (Si vous ne connaissez pas votre adresse IP, sur votre machine faite un "ipconfig" dans votre cmd et choisissez l'adresse commancant par "192.168.").

### Test réstrictions

- Allez sur votre machine et essayez d'accèder à votre page.
- Essayez d'aller dans les sous-répertoires.
- Sur le serveur, essayez d'accèder depuis la même adresse.
- Essayez d'aller dans les sous-répertoires.

<div style="page-break-after: always;"></div>

## Exercice 3

- Aller dans "Active Directory Users and Computers" puis dans votre forêt et sélectionnez "Users".
- Faites clique droit, "New", "User".
- Créer un utilisateur au nom de "SRW_labo" avec mot de passe "Qwertz123456" et décocher "User must change password at next logon".
- Créer un utilisateur au nom de "SRW_private" avec mot de passe "Qwertz123456" et décocher "User must change password at next logon".
- Ouvrez "Internet Information Services (IIS) Manager" et dans votre site "labo du Module SRW" allez dans "Authentication".
- Selectionnez "Anonymous Authentication" et editez le.
- Il faut activez l'utilisateur spécifique "SRW_private" et confirmer.
- Dans le "Server Manager" installer "Digest Authentication" et "URL Authorization".
- Allez dans "Internet Information Services (IIS) Manager" puis dans votre site.
- Dans "authentication" désactivez "Anonymous Authentication" puis activez "Digest Authentication" et "Windows Authentication".
- Allez dans le dossier "private" de votre site et dans la partie "Authorization Rules".
- Modifiez l'existant et activez l'utilisateur spécifique "SRW_private".

### Tests configuration anonyme

- Essayez de se connecter au site et 

### Tests configuration final

Créez un nouveau compte local dans l'AD
Nom : SRW_private
Mot de passe : Qwertz123456
En utilisant l'authentification Windows et digest faites en sorte que seul l'utilisateur SRW_private puisse accéder à http://votresite/private
En vous connectant avec l'utilisateur SRW_private (nouvel session Windows) tester l’accès à http://votresite/private en utilisant IExplorer (authentification Windows)
En vous connectant avec l'utilisateur SRW_private (nouvel session Windows) tester l’accès à http://votresite/private en utilisant l'authentification de digest
Testez le bon fonctionnement de votre configuration (Décrivez les tests que vous avez effectués)