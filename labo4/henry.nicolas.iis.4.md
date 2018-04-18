# Cours SRW3

Henry Nicolas

SI-T1a

## Laboratoire 4

LABO : Mise en place d'un service Internet et Intranet

## Exercice 1

- Allez dans les paramètres de votre machine virtuel, lui ajouter une carte réseau en bridge et vérifier son IP avec le bon vieux "ipconfig".
- Allez dans votre Active Directory et ajouter 4 groupes et 6 utilisateurs avec ces informations:
    
| Nom     | Prénom | Fonction  | Login   | Password     |
|---------|--------|-----------|---------|--------------|
| DUPONT  | Marcel | Directeur | mdupont | Qwertz123456 |
| BRICOT  | Juda   | Ingénieur | jbricot | Asdfgh123456 |
| ASSAIN  | Marc   | Comptable | massain | Yxcvbn123456 |
| DEUF    | John   | Ingénieur | jdeuf   | Qaywsx123456 |
| DIOCY   | Kelly  | Ingénieur | kdiocy  | Edcrfv123456 |
| Clients |        |           | dclient | Tgbzhn123456 |

- Créez un dossier "www" à la racine de "C:\\\\".
- Créez un dossier "internet" et "intranet" dans "www" avec un fichier "index.html" dans les dossiers.
- Dans votre dossier "intranet" créer un dossier "users".
- Créer un dossier pour chaque utilisateur (Exemple: "jbricot", "massain") avec un fichier "index.html" à l'intérieur.
- Créez un site internet qui pointe sur "C:\\\\www\internet" avec comme ip "172.17. ...".
- Créez un site intranet qui pointe sur "C:\\\\www\intranet" avec comme ip "192.168. ...".
- Créer un "virtual host" dans votre site internet qui pointe sur "C:\\\\www\intranet".
- Créer un "virtual host" dans votre site intranet qui pointe sur "C:\\\\www\internet".
- Dans le site "internet": 
    - "Authentication" -> "Anonymous Authentication"
    - "Default Document" -> tout supprimer et ajouter "index.html"
    - Dossier "dclient":
        - "Authentication" -> "Windows Authentication" et désactiver les autres.
        - "Authorization Rules" -> Supprimer "All Users" et ajouter les groupes "Ingénieur" et "dclient".
- Dans le site "intranet":
    - "Authentication" -> "Anonymous Authentication"
    - "Default Document" -> tout supprimer et ajouter "index.html"
    - Dossier "users":
        - "Authentication" -> "Windows Authentication" et désactiver les autres.
        - "Authorization Rules" -> Supprimer "All Users".
        - Dossier "jbricot":
            - "Authorization Rules" -> Supprimer "All Users" et ajouter "User->jbricot".
        - Effectuer la même oppération pour chaques utilisateurs.
- verifier que les droits soient identiques pour vos virtuals hosts.

- Dans votre machine win7, installez http://www.iis.net/downloads/microsoft/iis-manager (http://www.iis.net/downloads/microsoft/iis-manager) pour gérer vos sites.
- Sur la console IIS de votre serveur, activez "Enable connections".
- Cliquez sur "apply" puis sur "start"
- une fois le gestionnaire des services Internet (IIS) installé sur votre machine, ajoutez une connexion à un site sur votre site internet et intranet.
- Sur la console IIS, cliquez sur le site internet, cliquez sur IIS Manager Permissions puis autorisez le groupe Ingenieur

- Sur votre console IIS du serveur, cliquez sur votre serveur et allez dans "Server Certificates".
- Dans le menu de droite, "Create Self-Signed Certificate...".
- Ajoutez un nom pour votre certificat et choisissez "Web Hosting"
- Dans votre site, allez dans "Binding" et ajoutez votre https à votre addresse.
- Effectuez le https pour vos deux sites.