# Cours SRW3

Henry Nicolas

SI-T1a

## Laboratoire 2

LABO : Configuration de IIS

## Exercice 1

- Créer un dossier "SRW" dans le répertoire "C:\".
- Créer un dossier "Site01" et "repvirtuel" dans votre dossier "SRW".
- Dans le dossier "Site01" créer un fichier "indexsite01.html".
- Dans le dossier "repvirtuel" créer un fichier "indexvirtuel.html".
- Créer un site dans votre Gestionnaire des services Internet (IIS) se nommant "Site01 du Module SRW" qui sera sur "C:/SRW/Site01" et qui utilise comme port 8080.
- Allez dans les paramètres avancé puis changez l'ID en 31.
- Allez dans les documents par defauts et supprimez tout les documents puis rajoutez un dont le nom sera "indexsite01.html".
- Allez dans "Navigation dans le répertoire" et activez dans les actions dans le menu de droite.
- Clique droit sur votre site et ajouter un répertoire virtuel.
- Dans "Alias" écrivez test1 et dans "Path" le dossier sur lequel vous pointerez "C:/SRW/repvirtuel".

<div style="page-break-after: always;"></div>

## Exercice 2

- Créer un dossier "Site02" dans votre dossier "SRW".
- Dans le dossier "Site02" créer un fichier "indexsite02.html".
- Ouvrez votre cmd en admin et placez vous dans "C:\Windows\System32\inetsrv".
- Lancez la commande "appcmd add site /name:"Site02 du Module SRW" /id:32 /physicalPath:C:\SRW\Site02 /bindings:http/*:8888:"
- Lancez la commande "appcmd set config "Site02 du Module SRW" /section:defaultDocument /~files" pour retirer tout les fichiers de Default Documents
- Lancez la commande "appcmd set config "Site02 du Module SRW" /section:defaultDocument /+files.[value='indexsite02.html']" pour rajouter le fichier dans les Default Documents.
- Lancez la commande "appcmd add vdir /app.name:"Site02 du Module SRW/" /path:/test2 /physicalPath:C:\SRW\repvirtuel" pour créer votre répertoire virtuel. 
- Lancez la commande "appcmd set config "Site02 du Module SRW" /section:directoryBrowse /enabled:true" pour autoriser l'exploration de répertoire.
- Dans le "Windows Firewall with Advanced Security" Ajoutez une règle pour le port 8888.
- Redemarrez votre site avec "appcmd start sites "Site02 du Module SRW".

<div style="page-break-after: always;"></div>

## Exercice 3

- Créer un dossier "Site03" dans votre dossier "SRW".
- Dans le dossier "Site03" créer un fichier "indexsite03.html".
- Allez dans "C:\Windows\System32\inetsrv\config" et ouvrez "applicationHost.config"
- Ajoutez :
```xml
<site name="Site03 du Module SRW" id="33" serverAutoStart="true">
    <application path="/">
        <virtualDirectory path="/" physicalPath="C:\SRW\Site03" />
        <virtualDirectory path="/test2" physicalPath="C:\SRW\repvirtuel" />
    </application>
    <bindings>
        <binding protocol="http" bindingInformation="*:8880:" />
    </bindings>
</site>
```

- Dans votre dossier "C:\SRW\Site03" créer un fichier web.config contenant 

```xml
<site name="Site03 du Module SRW" id="33" serverAutoStart="true">
    <application path="/">
        <virtualDirectory path="/" physicalPath="C:\SRW\Site03" />
        <virtualDirectory path="/test3" physicalPath="C:\SRW\repvirtuel" />
    </application>
    <bindings>
        <binding protocol="http" bindingInformation="*:8880:" />
    </bindings>
</site>
```

- Dans le "Windows Firewall with Advanced Security" Ajoutez une règle pour le port 8880.

<div style="page-break-after: always;"></div>

## Exercice 4

- Pour afficher vos sites disponibles, faites "appcmd list site".
- Pour trouver les sites écoutant le port 8080 : "appcmd list site /bindings:http/*:8080:".
- Pour stopper les sites écoutant le port 8080 : "appcmd list site /xml /bindings:http/*:8080: | appcmd stop site /in"
- Pour démarrer un site écoutant le port 8080 :  "appcmd list site /xml /bindings:http/*:8080: | appcmd start site /in"
- Pour supprimer le répertoire virtuel « test1 »du site01 : "appcmd delete vdir "Site01 du Module SRW/test1"".
- Pour effectuez une sauvegarde de la configuration de IIS : "appcmd add backup "Backup iis site02""
- Pour supprimer le site02 présent sur votre serveur : "appcmd delete site "Site02 du Module SRW""
- Pour restaurez votre sauvegarde précédente de la configuration de IIS : "appcmd restore backup "My Backup Name""