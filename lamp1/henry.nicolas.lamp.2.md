# Cours SRW3

Henry Nicolas

SI-T1a

## Laboratoire

LABO : Configuration générale d'Apache Page

## Directives de configuration

[✓] Sous Debian, quel est le fichier de configuration principal d'Apache ?
[✓] apache2.conf

[✓] Listez les directives include présentes dans le fichier apache2.conf et expliquez à quoi elles servent.

Inclut d'autres fichiers de configuration dans un des fichiers de configuration du serveur

59:# ServerRoot: The top of the directory tree under which the server's
69:#ServerRoot "/etc/apache2"
cpnv@SRW3-debian:/etc/apache2$ grep -ni include /etc/apache2/apache2.conf
35:# * ports.conf is always included from the main configuration file. It is
145:# Include module configuration:
146:IncludeOptional mods-enabled/*.load
147:IncludeOptional mods-enabled/*.conf
149:# Include list of ports to listen on
150:Include ports.conf
218:# Include of directories ignores editors' and dpkg's backup files,
221:# Include generic snippets of statements
222:IncludeOptional conf-enabled/*.conf
224:# Include the virtual host configurations:
225:IncludeOptional sites-enabled/*.conf

[✓] Pour chacune des directives ci-dessous, donnez le nom du fichier de configuration où elle se trouve et expliquez à quoi elle sert (utilisez l'aide en ligne installée précédemment pour vous aider à répondre à cette question) :

| Nom            | Localisation                                  | Déscription                                                                                                      |
|----------------|-----------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| ServerName     | /etc/apache2/sites-available/000-default.conf | Permet de définir un nom d'hôte et un numéro de port                                                             |
| ServerRoot     | /etc/apache2/apache2.conf                     | Le répertoire de niveau supérieur contenant les fichiers du serveur                                              |
| PidFile        | /var/run/apache2/apache2.pid                  | Les fichiers pid contiennent l'identifiant de processus d'un programme donné.                                    |
| DocumentRoot   | /etc/apache2/sites-available/000-default.conf | Répertoire contenant la plupart des fichiers HTML qui seront servis en réponse aux requêtes                      |
| Listen         | /etc/apache2/ports.conf                       | Identifie les ports sur lesquels votre serveur Web acceptera les demandes entrantes                              |
| User           | /etc/apache2/envvars   (www-data)             | Définit le nom d'utilisateur du processus serveur et détermine les fichiers auxquels le serveur peut avoir accès |
| Group          | /etc/apache2/envvars   (www-data)             | Spécifie le nom de groupe des processus du Serveur HTTP Apache                                                   |
| ServerAdmin    | /etc/apache2/sites-available/000-default.conf | Adresse électronique de l'administrateur du serveur Web                                                          |
| AccessFileName | .htaccess                                     | Fichier que le serveur doit utiliser pour les informations de contrôle d'accès dans chaque répertoire            |
| DirectoryIndex | /etc/apache2/mods-enabled/dir.conf            | Page servie par défaut lorsqu'un utilisateur demande un index de répertoire                                      |

<div style="page-break-after: always;"></div>

## Réglages d'exécution

[✓] Quel est le nombre actuel de serveurs WEB en exécution lors du démarrage d'Apache ? le nombre maximum en exécution simultanément ?

[✓] ps -aux | grep apache
Minimum 5
Maximum 25

## Exercices de configuration de base

### Repertoire de publication
- cd /var
- `# mkdir apache`
- cd apache
- `# nano apache.html`
- cd /etc/apache2/sites-available
- `# cp 000-default.conf monsite.conf`
- `# nano monsite.conf`
- Modifier le documentRoot en /var/apache
- Ajouter en dessous du DocumentRoot
- DirectoryIndex apache.html
- Supprimer les hotes virtuels
- `# rm /etc/apache2/sites-enabled/*`
- Dans /etc/apache2/sites-available 
- `# a2ensite monsite.conf`
- Dans /etc/apache2/apache2.conf ajoutez les lignes suivantes: 
```
<Directory /var/apache/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
```

<div style="page-break-after: always;"></div>

## Configuration des modules
[✓] Comment connaître la liste des modules compilés avec le core ?
`# apache2ctl -t -D DUMP_MODULES`

[✓] Quelle directive d'inclusion du fichier de configuration d'Apache2 prend en compte les fichiers de configuration des modules ?

- LoadModule dans /etc/apache2/mods-available/

### Module UserDir

[✓] Ouvrez le contenu du fichier userdir.conf, que constatez-vous ?
- On peut utiliser le dossier public_html des dossiers utilisateurs comme DocumentRoot

[✓] Ouvrez le contenu du fichier userdir.load, que constatez-vous ?
- Il charge le module mod_userdir.so

Activez le module UserDir (commande a2enmod)
`# a2enmod userdir`

Créez un nouvel utilisateur linux nommé collegue
`# adduser collegue`

[✓] Quel est le nom du répertoire que collegue doit créer et où est configuré ce nom ?
- On doit créer le dossier suivant :
- mkdir ~/public_html

[✓] Quels droits minimum doivent avoir ce répertoire et les fichiers qui s’y trouvent.
- Lecture et execution

[✓] Quel est le nom de la page par défaut du site personnel et où cette directive est elle configurée ?
- index.html
- dans : `/etc/apache2/site-available/default`

[✓] Par quelle URL pouvez-vous accéder au site personnel de collegue ?
- localhost/nomdutilisateur/public_html

[✓] Mettez en place un système qui permettra de créer automatiquement pour tous les nouveaux utilisateurs de votre serveur un répertoire web ainsi qu’une page d’accueil par défaut. (/etc/skel)
```
# mkdir /etc/skel/public_html
# touch /etc/skel/public_html/index.html
```

<div style="page-break-after: always;"></div>

## Journal des accès

[✓] Quelle commande permet de visualiser le contenu du fichier access.log en direct ?

`# tail -f /var/log/apache2`

[✓] Interprétez les champs de chaque ligne, en particulier repérez les renseignements qui concernent le client et les codes de retour des diverses requêtes (200 réussie, 404 "not found" ...)
- Adresse IP du client
- Heure de la requête
- La requête avec l'action HTTP.
- Le message HTTP et les informations provenant de la source de la requête.

## Alias

[✓] Créez un répertoire /var/administration/ ainsi qu’une page admin.html
```
# mkdir /var/administration
# Alias /admin "/var/administration
```

[✓] Faites-en sorte que le répertoire /var/administration/ puisse être accessible à travers l’url http://@IP/admin/ et que la page admin.html devienne la page par défaut de ce répertoire.

```
<Directory "/var/administration">
  Options Indexes FollowSymLinks
  AllowOverride None
  Require all granted
  DirectoryIndex admin.html
</Directory>
```