# Cours SRW3

Henry Nicolas

SI-T1a

## Laboratoire

LABO : Installation et configuration d'un Serveur IIS

## Exercice 1

### Installation du serveur WEB IIS 10

- Une fois votre serveur windows 2016 installé, allez dans les rôles et allez dans les rôles serveur.
- Choisissez Web Server (IIS) et choisissez le FTP Server et dans Web Server, ajoutez .NET Extensibility 4.6, ASP.NET 4.6, CGI, ISAPI Extensions et Filters Puis installez.

### Configuration des sites

- Créer un nouveau dossier à la racine de C: qui se nommera iis_www.
- Allez dans IIS et selectionnez le server crée. 
- Clique droit puis Internet Information Services (IIS) Manager.
- Selectionnez votre page et ajoutez un nouveau site.
- Dans Site Name : Site IIS
- Dans Physical path : C:\iis_www
- modifiez le port en 8080 et tout est bon.

Dans votre nouveau site, allez dans Default Document puis ajoutez votre fichier iis.html
clique droit sur le Default Website et dans Manage Website, stoppez la page par defaut.
Créer un fichier iis.html dans votre repertoire iis_www.

Allez dans le Firewall puis dans Inbound ajoutez une nouvelle règle et ajoutez le port 8080.

Testez votre site depuis votre machine physique avec ip:8080.

Allez sur Internet Information Services (IIS) Manager et Ajouter une nouvelle web platform composant puis telechargez la plateforme et installez la.

### Installation de PHP

- Installez Visual C++ Redistributable for Visual Studio 2012 Update 4 et php 5.6.31.

- Créer un fichier index.php dans votre dossier iis_web contenant `phpinfo();` Puis testez le en faisant ip:8080/index.php.

<div style="page-break-after: always;"></div>

### Authentification

Ajouter le rôle AD Domain Services et installez le.

Créer un nouveau Tree et mettez votre session à l'intérieur.

Installez le Windows Authentification dans les Web Server (IIS) puis redemarrez la console Manager de IIS et désactivez l'Anonymous Authentication et activez la Windows Authentication.

### FTP

Allez dans votre Manager Services et faites un clique droit sur site puis, ajouter un site FTP.
Ne mettez pas votre adresse IP et laissez le port 21, puis, choisissez aucun certificat SSL.

Sous authentication choisissez Basic, Authorization choisissez All users et sous Permissions donnez Read Write.

Vous pouvez tester votre accès FTP en faisant ftp://ip et si tout est bon, il vous retournera le contenu de votre dossier iis_web.


### Backup

Install feature Windows Server Backup

Si vous n'avez qu'une partition sur votre Serveur, vous devrez en ajouter un pour y déposer vos backup Si vous avez une partition vide, passez à l'étape suivante.

Allez dans Disk Management puis clique droit sur votre disque C et faite un "Shrink Volume..." donnez à votre disque un espace suffisant pour y deposer la sauvegarde de votre site et de votre config IIS.

Ouvrir Windows Server Backup
Allez dans Local Backup puis ajoutez un "Backup Schedule".

Définissez un backup custom et ensuite, ajoutez les dossiers que vous souhaitez backupper puis définissez l'heure a laquel vous souhaitez faire votre sauvegarde. 
Dans notre cas nous allons sauvegarder "C:\iis_www\" et "C:\Windows\System32\inetsrv\config\"
Faites le Backup sur un volume et c'est tout bon.