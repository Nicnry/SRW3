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

<div style="page-break-after: always;"></div>

L’entreprise DUPONT vient de faire l’acquisition d’un nouveau serveur dans le but d’héberger les sites INTERNET et INTRANET de l’entreprise. L’entreprise DUPONT ne possède pas encore une gestion centralisée des comptes utilisateurs, la gestion des utilisateurs se fera en local à partir du serveur IIS.

Technologies à utiliser --> Windows Web Server IIS

M. Dupont, patron de cette entreprise, vous expose sa vision des choses :

"Je désire installer un site INTERNET et INTRANET au sein de mon entreprise. Chaque employé de mon entreprise doit pouvoir accéder aux pages générales de l'INTRANET ainsi qu’à une section privée qui lui est réservée. Il doit aussi pouvoir accéder par la page principale de l'INTRANET à la page principale du site INTERNET et vice versa. Pour accéder à l'INTRANET depuis le réseau de mon entreprise, je ne veux pas avoir besoin de m’identifier sauf pour atteindre ma section privée. Depuis l’extérieur, le site INTERNET doit être accessible à tout le monde. Il doit être aussi possible depuis le site INTERNET, si l’on est un employé de mon entreprise, d’atteindre le site INTRANET après une demande d'authentification. Une section privée dclient unique du site INTERNET doit être réservée à mes clients. Un nom d'utilisateur LOGIN et un mot de passe générique seront utilisés par tous mes clients pour accéder à cette section. Cette section dclient privée doit aussi être accessible à tous mes ingénieurs par leur identifiant personnel. Les ingénieurs de mon entreprise ne doivent pas avoir les droits d'administrer le serveur ni le service IIS mais doivent pouvoir administrer les sites INTERNET et INTRANET."

Pour réaliser ce TP, vous allez configurer votre machine virtuelle avec deux interfaces réseau, la première (@IP1) configurée en BRIDGE, la seconde (@IP2) en NAT. L'adressage des deux interfaces se statique.

    @IP1  -->  Bridge -->  IP CPNV   -->  172.17...  -->  WAN
    @IP2  -->  NAT    -->  IP VMWARE -->  192.168... -->  LAN
    
Liste des employés de l’entreprise DUPONT

    | Nom     | Prénom | Fonction  | Login   | Password     |
    |---------|--------|-----------|---------|--------------|
    | DUPONT  | Marcel | Directeur | mdupont | Qwertz123456 |
    | BRICOT  | Juda   | Ingénieur | jbricot | Asdfgh123456 |
    | ASSAIN  | Marc   | Comptable | massain | Yxcvbn123456 |
    | DEUF    | John   | Ingénieur | jdeuf   | Qaywsx123456 |
    | DIOCY   | Kelly  | Ingénieur | kdiocy  | Edcrfv123456 |
    | Clients |        |           | dclient | Tgbzhn123456 |

<div style="page-break-after: always;"></div>

## Exercice 2

La sécurité est un élément extrêmement important, surtout dans un environnement tel que celui d’Internet. Les informations qui circulent entre un ordinateur client et un serveur Web sont souvent confidentielles (mot de passe, numéro de carte de crédit, etc.). Si ces informations ne sont pas sécurisées, elles peuvent être interceptées par n’importe qui, ce qui est fortement dommageable.

Sur les sites Intranet et Internet, de nombreux répertoires ne sont accessibles qu’à partir du moment où l’utilisateur a fourni ses identifiants. La vérification de la validité des identifiants saisis est effectuée avec l’authentification de base. Cette dernière présente l’avantage d’être compatible avec la majorité des navigateurs. Son gros inconvénient vient du fait que les informations saisies par l’utilisateur transitent en clair sur le réseau. N’importe qui, muni d’un logiciel spécial (sniffers), peut facilement les intercepter et ainsi usurper l’identité de l’utilisateur. L’authentification de base doit donc être couplée avec le protocole SSL (Secure Socket Layer). Ce dernier permet de sécuriser les transactions sur Internet entre un client et un serveur. Le protocole HTTP a été modifié pour supporter SSL et a pris l’acronyme HTTPS (Hypertext Transfer Protocol Secure sockets).

Lorsque l’utilisateur se connecte à un serveur sécurisé, ce dernier va lui envoyer un certificat, lequel est une sorte de pièce d’identité (nom de l’entreprise ou de l’établissement, adresse, etc.). Le navigateur Internet de l’utilisateur va dès lors vérifier la validité du certificat, qui doit être certifié par une autorité compétente. Si le certificat s’avère être invalide, le navigateur n’affichera pas le contenu demandé et indiquera à l’utilisateur que l’accès au site présente un risque. Libre à lui de poursuivre le chargement du contenu ou de quitter. L’utilisation d’un certificat est nécessaire pour implémenter SSL.

Deux solutions sont dès lors envisageables :

Certificat signé par un organisme de certifications sont nécessaires lorsqu'il s'agit d'assurer la sécurité des échanges avec des utilisateurs anonymes, par exemple dans le cas d'un site Web.
Certificat autosigné sont des certificats à usage interne. Signé par un serveur local, ce type de certificat permet de garantir la confidentialité des échanges au sein d'une organisation, par exemple pour le besoin d'un intranet.
Le contenu du site INTERNET et INTRANET nécessitant une authentification pour y accéder est réservé à des utilisateurs connus (clients de l’entreprise et employés). Il est donc inutile de mettre en place un certificat signé par un organisme de certification. Toutefois, étant donné que les utilisateurs doivent saisir leurs identifiants personnels pour accéder à ce contenu, il est impératif de mettre en place un système de transaction sécurisée. L’utilisation d’un certificat autosigné est donc la solution idéale.

Le cryptage sera utilisé dans deux cas :

lorsque l’utilisateur doit saisir des identifiants personnels pour accéder à un répertoire du site (authentification de base)
lorsque les identifiants personnels de l’utilisateur sont utilisés pour lui permettre d’accéder à un répertoire du site (authentification Windows)


LAN [intranet] 192.168.XXX.XXX (9 pts.)
Nouvelle navigation anonyme

[ ok ] http://[intranet] (OK pas d'authentification)
[ ok ] http://[intranet]/internet (OK pas d'authentification)
[ ok ] http://[intranet]/internet/dclient (OK dclient)
Nouvelle navigation anonyme

[ ok ] http://[intranet]/internet/dclient (auth. massain) NOK comptable
[ ok ] http://[intranet]/internet/dclient (auth. jbricot) OK ingénieur
[ ok ] http://[intranet]/../mdupont (NOK sans nouvelle auth.)
[ ok ] http://[intranet]/../mdupont (OK auth. mdupont)
[ ok ] http://[intranet]/../jbricot (NOK sans nouvelle auth.)
[ ok ] http://[intranet]/../jbricot (OK auth. jbricot)


WAN [internet] 172.17.XXX.XXX (11 pts.)
Nouvelle navigation anonyme

[ ok ] http://[internet] (Pas d'authentification)
[ ok ] http://[internet]/dclient (auth. dclient)
Nouvelle navigation anonyme

[ ok ] http://[internet]/dclient (auth. jbricot) - OK ingénieur
[ ok ] http://[internet]/intranet (OK sans nouvelle auth.)
[ ok ] http://[internet]/intranet/../jbricot (OK sans nouvelle auth.)
[ ok ] http://[internet]/intranet/../massain (NOK sans nouvelle auth.)
[ ok ] http://[internet]/intranet/../massain (auth. massain)
Nouvelle navigation anonyme

[ ok ] http://[internet]/intranet (OK auth. kdiocy)
[ ok ] http://[internet]/intranet/../jbricot (NOK sans nouvelle auth.)
[ ok ] http://[internet]/intranet/../jbricot (auth jbricot)
[ ok ] http://[internet]/dclient (OK auth. jbricot)


mass
Gestion des sites par les ingénieurs (3 pts.)
Ouverture d'une session windows pour l'utilisateur jbircot

[ ] Console IIS (gestion site INTERNET par jbircot) 
Divers (3 pts.)
[ ok ] Utilisation d'un groupe `Ingénieurs`
[ ok ] Répertoire parent `Users` pour couper l’héritage par défaut
[ ] Rajouter les droit au groupe `IIS_IUSRS` pour lecture des fichiers **web.conf**
SSL (4 pts. + 2 pts. bonus)
[ ] https://[internet]
[ ] https://[intranet]

[ ] Redirection http --> https (http://[internet]/intranet) - BONUS