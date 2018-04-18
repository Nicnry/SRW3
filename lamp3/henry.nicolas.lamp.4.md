# Cours SRW3

Henry Nicolas

SI-T1a

## Laboratoire

LABO : Hôtes virtuelles

## Hébergement virtuel par nom
1. Désactivez les sites actifs sur votre serveur avec la commande a2dissite
2. Créez un nouveau fichier de configuration nommé sites-virtuels dans le répertoire
/etc/apache2/sites-available/
NameVirtualHost [votre adresse IP]:80

<VirtualHost *:80>
    ServerName test1
    ServerAdmin webmaster@test1
    DocumentRoot /var/www/test1
    <Directory /var/www/test1>
        DirectoryIndex test1.html
    </Directory>
</VirtualHost>

<VirtualHost *:80>
    ServerName test2
    ServerAdmin webmaster@test2
    DocumentRoot /var/www/test2
    <Directory /var/www/test2>
        DirectoryIndex test2.html
    </Directory>
</VirtualHost>

3. Créez un répertoire /var/www/test1 avec un fichier test1.html
4. Créez un répertoire /var/www/test2 avec un fichier test2.html
5. Renseignez le fichier /etc/hosts :
127.0.0.1 localhost
[votre adresse IP] test1
[votre adresse IP] test2
6. Pour que le fichier précédent, décrivant les sites virtuels, soit intégré dans apache2.conf il
faut placer les liens vers le répertoire /etc/apache2/sites-enabled/. Cette mise en service
se fait facilement grâce à la commande a2ensite, puis on recharge la configuration.
7. Testez et vérifiez http ://test1 depuis votre machine virtuelle
8. Testez et vérifiez http ://test2 depuis votre machine virtuelle
9. Modifiez le fichier host de votre machine Windows pour pouvoir accéder au site test1 et
test2 de votre serveur 1
Hébergement virtuel par IP et Port
1. Désactivez tous les sites actifs sur votre serveur avec la commande a2dissite
2. Rajoutez une carte réseau à votre machine virtuelle VMware
a. Sous VMware ajoutez un nouvel Adaptateur Réseau NAT
b. Modifiez le fichier /etc/network/interfaces en rajoutant les lignes suivantes
auto eth1
iface eth1 inet dhcp
c. Redémarrez votre machine virtuelle et contrôlez le bon fonctionnement du périphérique
eth1 (ifconfig, ping. . . )
3. Créez un répertoire /var/www/site1/
a. Créez dans le répertoire /var/www/site1/ une page nommée site1.html
4. Créez un répertoire /var/www/site2/
a. Créez dans le répertoire /var/www/site2/ une page nommée site2.html
5. Créez un répertoire /var/www/site3/
a. Créez dans le répertoire /var/www/site3/ une page nommée site3.html
6. Créez dans le répertoire de configuration des sites disponibles trois nouveaux fichiers de
configuration nommée site1.conf, site2.conf et site3.conf
7. Le fichier de configuration site1.conf permet de configurer un site virtuel avec les
paramètres suivants
a. liaison : @IP_eth0 :80
b. Répertoire de publication : /var/www/site1/
c. Page par défaut : site1.html
d. Indexation des répertoires et utilisation .htaccess refusées
8. Le fichier de configuration site2.conf permet de configurer un site virtuel avec les
paramètres suivants
a. liaison : @IP_eth0 :8080
b. Répertoire de publication : /var/www/site2/
c. Page par défaut : site2.html
d. Indexation des répertoires et utilisation .htaccess autorisées
9. Le fichier de configuration site3.conf permet de configurer un site virtuel avec les
paramètres suivants
a. liaison : @IP_eth1 :80
b. Répertoire de publication : /var/www/site3/
c. Page par défaut : site3.html
d. Indexation des répertoires autorisée et utilisation .htaccess refusée
10. Activez les sites virtuels site1,site2 et site3
11. Testez la disponibilité de vos trois sites par leur adresse IP et port respectif en local et
depuis votre machine host.