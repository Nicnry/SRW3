# Cours SRW3

Henry Nicolas

SI-T1a

## Laboratoire

LABO : POC (proof of concept) LAMP

## Travail pratique

Ajoutez une carte réseau en bridge, puis modifier `/etc/network/interfaces`

```
auto ens32
iface ens32 inet static
address 192.168.106.128
netmask 255.255.255.0
auto ens34
iface ens34 inet static
address 172.17.100.168
netmask 255.255.0.0
```

Puis redemarrez votre machine

`# reboot`

Désactivez vos sites 

`# a2dissite *`

Créer l'arboresence pour chaques utilisateurs :

`/var/www/internet/index.html`

`/var/www/internet/dclient/index.html`

`/var/www/intranet/index.html`

`/var/www/intranet/users/USERNAME/index.html`

Dans le fichier `/etc/apache2/sites-available/internet.conf` :

```
<VirtualHost 172.17.100.168:80>
    RewriteEngine on
    RewriteCond %{HTTPS} off
    RewriteRule ^(.*)$ https://%{SERVER_NAME}%{REQUEST_URI} [R=301]
    Alias /intranet /var/www/intranet
    ServerName www.tp.com
    ServerAdmin webmaster@tp.com
    DocumentRoot /var/www/internet
    <Directory /var/www/internet>
        Options Indexes FollowSymLinks
        Order allow,deny
        Allow from all
        DirectoryIndex index.html
        Require all granted
        AllowOverride None
    </Directory>
    <Directory /var/www/internet/dclient>
        Options Indexes FollowSymLinks
        DirectoryIndex index.html
        AllowOverride All
    </Directory>
    <Directory /var/www/intranet>
        Options Indexes FollowSymLinks
        DirectoryIndex index.html
        AllowOverride All
    </Directory>
</VirtualHost>
    Listen 443
<VirtualHost 172.17.100.168:443>
    SSLCertificateFile /etc/apache2/ssl/tp_internet.pem
    Alias /intranet /var/www/intranet
    SSLEngine on
    ServerName www.tp.com
    ServerAdmin webmaster@tp.com
    DocumentRoot /var/www/internet
    <Directory /var/www/internet>
    Options Indexes FollowSymLinks
    Order allow,deny
    Allow from all
    DirectoryIndex index.html
    AllowOverride None
    Require all granted
    </Directory>
    <Directory /var/www/internet/dclient>
    Options Indexes FollowSymLinks
    DirectoryIndex index.html
    AllowOverride All
    </Directory>
    <Directory /var/www/intranet>
    Options Indexes FollowSymLinks
    DirectoryIndex index.html
    AllowOverride All
    </Directory>
</VirtualHost>
```

Dans le fichier `/var/www/intranet/dclient/.htaccess` :

```
AuthName "@tp.com"
AuthType Digest
AuthDigestDomain "/dclient" "https://172.17.100.168/dclient/" "http://172.17.100.168/dclient/"
AuthUserFile "/etc/apache2/passwd/.tp_usersdigest"
AuthGroupFile "/etc/apache2/passwd/.tp_groups"
Require group customers engineers
Satisfy All
```

Dans le fichier `/etc/apache2/sites-available/internet.conf` :

```
<VirtualHost 192.168.106.128:80>
    RewriteEngine on
    RewriteCond %{HTTPS} off
    RewriteRule ^(.*)$ https://%{SERVER_NAME}%{REQUEST_URI} [R=301]
    Alias /internet /var/www/internet
    ServerName intranet.tp.com
    ServerAdmin webmaster@tp.com
    DocumentRoot /var/www/intranet
    <Directory /var/www/intranet>
        Options Indexes FollowSymLinks
        DirectoryIndex index.html
        Require all granted
        AllowOverride All
    </Directory>
    <Directory /var/www/intranet/users>
        Options Indexes FollowSymLinks
        DirectoryIndex index.html
        AllowOverride All
    </Directory>
    <Directory /var/www/internet/dclient>
        Options Indexes FollowSymLinks
        DirectoryIndex index.html
        AllowOverride All
    </Directory>
</VirtualHost>
Listen 443
<VirtualHost 192.168.106.128:443>
    SSLCertificateFile /etc/apache2/ssl/tp_intranet.pem
    Alias /internet /var/www/internet
    SSLEngine on
    ServerName intranet.tp.com
    ServerAdmin webmaster@tp.com
    DocumentRoot /var/www/intranet
    <Directory /var/www/intranet>
        Options Indexes FollowSymLinks
        Order allow,deny
        Allow from all
        DirectoryIndex index.html
        AllowOverride None
        Require all granted
    </Directory>
    <Directory /var/www/intranet/users>
        Options Indexes FollowSymLinks
        DirectoryIndex index.html
        AllowOverride All
    </Directory>
    <Directory /var/www/internet/dclient>
        Options Indexes FollowSymLinks
        DirectoryIndex index.html
        AllowOverride All
    </Directory>
</VirtualHost>
```

Dans `/var/www/intranet/.htaccess` :

```
Order allow,deny
Allow from 192.168
AuthName "@tp.com"
AuthType Digest
AuthDigestDomain "/intranet" "https://172.17.100.168/intranet/" "http://172.17.100.168/intranet/"
AuthUserFile "/etc/apache2/passwd/.tp_usersdigest"
AuthGroupFile "/etc/apache2/passwd/.tp_groups"
Require group users
Satisfy Any
```

Dans `/var/www/intranet/users/.htaccess` :

```
Order allow,deny
Deny from All
Satisfy All
```

Pour chaques utilisateurs créer `/var/www/intranet/users/USERNAME/.htaccess` :

```
AuthName "@tp.com"
AuthType Digest
AuthDigestDomain "/users/USERNAME" "http://192.168.106.128/users/USERNAME"
AuthUserFile "/etc/apache2/passwd/.tp_usersdigest"
AuthGroupFile /dev/null
Require user USERNAME
Satisfy All
```

Activez Digest :

`# a2enmod auth_digest`

`# service apache2 restart`
Activez Authz_groupfile : 

`# a2enmod authz_groupfile`


Activez SSL :

`# a2enmod ssl`

`# service apache2 restart`

Activez la réecriture : 

`# a2enmod rewrite`

`# service apache2 restart`

Pour le premier utilisateur :

`# htdigest -c /etc/apache2/passwd/.tp_usersdigest "@tp.com" USERNAME`

Puis, pour tout les autres utilisateurs :

`htdigest /etc/apache2/passwd/.tp_usersdigest "@tp.com" USERNAME`

Dans `/etc/apache2/passwd/.tp_groups` :

```
users: mdupont jbricot massain jdeuf kdiocy
management: mdupont
customers: dclient
engineers: jbricot jdeuf kdiocy
accounting: massain
```

Créer le dossier `/etc/apache2/ssl` puis :

`# make-ssl-cert /usr/share/ssl-cert/ssleay.cnf /etc/apache2/ssl/tp_internet.pem` en mettant : `tp.com` puis : `DNS:tp.com,IP:172.17.100.168,IP:192.168.106.128`

Executer une deuxième fois puis :

`# make-ssl-cert /usr/share/ssl-cert/ssleay.cnf /etc/apache2/ssl/tp_intranet.pem`

Avec : `intranet.tp.com` puis : `IP:172.17.100.168,IP:192.168.106.128`

Activer les deux sites puis redémarrer le service apache :
`# a2ensite internet`
`# a2ensite intranet`
`# service apache2 restart`