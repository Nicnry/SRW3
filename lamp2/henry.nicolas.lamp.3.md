# Cours SRW3

Henry Nicolas

SI-T1a

## Laboratoire

LABO : Accessibilité

## Modifier l'accessibilité à un répertoire
Modifiez l'accès au site courant pour interdire l'accès à tous sauf à votre machine hôte (Windows).

- Dans le fichier /etc/apache2/sites-available/monsite.conf, rajouter les lignes suivantes:
```
<Directory "/var/apache">
    Order Allow,Deny
    Allow from 192.168.106.1
    Allow from localhost
</Directory>
```

Créez un sous répertoire /var/apache/local/

- Dans le répértoire suivant '/var/apache/local/' : # mkdir local

Modifiez l'accès au répertoire /var/apache/local/ pour interdire l'accès à tous sauf à votre machine locale (Linux).

- Dans le fichier /etc/apache2/sites-available/monsite.conf, rajouter les lignes suivantes:
```
<Directory "/var/apache/local">
    Order Allow,Deny
    #Allow from 192.168.106.1
    Allow from localhost
</Directory>
```

## Protections des accès
Dans cet exercice vous allez protéger l'accès à un répertoire particulier sur site principal.

Le sous-répertoire /var/apache/private/ ne devra être accessible qu'à un ensemble limité de comptes Apache (et non Linux !) à créer. La première requête adressée à ce répertoire protégé provoquera l'affichage d'une boîte de dialogue par laquelle l'utilisateur devra s'authentifier (nom et mot de passe).

Créez le répertoire /var/apache/private/ ainsi qu’une page index.html.

- \# mkdir private
- \# nano index.html

Testez son accessibilité par tous.

Changez, seulement pour le site en cours, le nom du fichier d’accès (AccessFileName) en .tpAPACHE

```
# nano /etc/apache2/sites-available/monsite.conf

AccessFileName .tpAPACHE
<Directory /var/apache/private>
    Order deny,allow
    Allow from all
</Directory>

# service apache2 restart
```
Empêchez tous les fichiers commençants par .tp d’être accessibles à travers le protocole http
```
# nano /etc/apache2/sites-available/monsite.conf

AccessFileName .tpAPACHE
<FilesMatch "^\.tp">
    Require all denied
</FilesMatch>
<Directory /var/apache/private>
    Order deny,allow
    Allow from all
</Directory>

# service apache2 restart
```

Créez dans le répertoire à protéger (private) un fichier d’accès qui permettra seul à l’utilisateur webmaster d’accéder à cette section.
Le fichier d’authentification des utilisateurs .tp_user doit se trouver dans le répertoire /etc/apache2/passwd/

Créez le compte webmaster dans le fichier d’authentification avec comme mot de passe apache
La boîte d’authentification doit afficher le message suivant : APACHE : section privée

```
# htpasswd -c /etc/apache2/passwd/.tp_user webmaster

# nano /var/apache/private/.tpAPACHE

DirectoryIndex index.html
AuthUserFile /etc/apache2/passwd/.tp_user
AuthName "APACHE : section privée"
AuthType Basic
Require user webmaster
```

Que faut-il encore modifier pour que votre protection fonctionne ?

```
# nano /etc/apache2/sites-available/monsite.conf

<Directory /var/apache/private>
    Order deny,allow
    Allow from all
    AllowOverride All
</Directory>

# service apache2 restart
# lynx 127.0.0.1/private
```

Créez un sous-répertoire /var/apache/private/test/ ainsi qu’une page index.html et testez l’accès de ce nouveau répertoire.

```
# mkdir /var/apache/private/test
# echo "DIR:Test" > /var/apache/private/test/index.html
# lynx 127.0.0.1/private/test
```

<div style="page-break-after: always;"></div>

## Création d'un répertoire d'administration
Créez un répertoire d'administration nommé /var/apache/admin/ ainsi qu’une page admin.html

```
# mkdir /var/apache/admin
# echo "DIR:Admin" > /var/apache/admin/admin.html
```

À l’aide des fichiers d’accès, faites en sorte que ce répertoire ne puisse pas être indexé et que la page par défaut soit admin.html

```
# nano /etc/apache2/sites-available/monsite.conf

<Directory /var/apache/admin>
    AllowOverride All
</Directory>

# service apache2 restart
# nano /var/apache/admin/.tpAPACHE

Options -Indexes
DirectoryIndex admin.html
Order deny,allow
Deny from all
Allow from 127.0.0.1 localhost
```

Le contenu de ce répertoire doit être accessible en local (machine virtuelle) sans demande d'authentification et depuis votre machine hôte par le groupe d’utilisateur admin. (webmaster est membre du groupe admin)

Le fichier des groupes .tp_groupe doit se trouver dans le répertoire /etc/apache2/passwd/

```
# nano /etc/apache2/passwd/.tp_groupe

admin : webmaster

# a2enmod authz_groupfile
# service apache2 restart

# nano /var/apache/admin/.tpAPACHEb

Order deny,allow
Deny from all
Allow from 127.0.0.1 localhost

AuthUserFile /etc/apache2/passwd/.tp_user
AuthGroupFile /etc/apache2/passwd/.tp_groupe
AuthName "APACHE : section privée"
AuthType Basic
Require group admin

Satisfy Any
```

Créez un nouvel utilisateur apache nommé momo et rajoutez-le au groupe admin
Le mot de passe de l'utilisateur momo est momo-apache

```
# htpasswd /etc/apache2/passwd/.tp_user momo
# nano /etc/apache2/passwd/.tp_groupe
admin : webmaster momo
```

Vérifiez que momo puisse effectivement accéder au répertoire d'administration depuis votre machine hôte.

```
# lynx 127.0.0.1/admin
```

Redirigez les erreurs 401 du répertoire d'administration sur la page d’accueil du site WEB. (directive ErrorDocument)

```
# nano /var/apache/admin/.tpAPACHE

ErrorDocument 401 /
```