requis:

     - installation apache
       
        $ sudo apt install apache2


Cloner le répertoire distant:

    $ git clone https://github.com/GregLebreton/apache-config.git

Copie du site dans le répertoire racine d'apache:

    $ cp -R monsite/ /var/www/html



Modification des droits d'accès aux dossiers pour apache:

    $ chmod -R 755 /var/www



Configuration du VirtualHost:

    $ nano /etc/apache2/sites-available/www.site1.com.conf

        <VirtualHost *:80>
                ServerAdmin greg@legaragenumerique
                ServerName www.site1.com
                ServerAlias site1.com
                DocumentRoot /var/www/html/monsite/


                <Directory />
                        Require all denied
                        Options -Indexes -Includes -ExecCGI -FollowSymlinks
                </Directory>

                <Directory /var/www/html/monsite/>
                        Require all granted
                        Options -Indexes -Includes -ExecCGI +FollowSymlinks
                </Directory>

                ErrorLog /var/log/apache2/error.log

                LogLevel warn

                CustomLog /var/log/apache2/access.log combined

        </VirtualHost>


Modification du fichier /etc/hosts:

    $ nano /etc/hosts

        127.0.0.1 www.monsite.com


Création du lien symbolique du fichier de configuration vers les sites "actifs":

    $ ln -s /etc/apache2/sites-available/www.site1.com.conf /etc/apache2/sites-enabled/www.site1.com.conf


Redémarrage de apache et du réseau de la machine:

    $ systemctl restart apache2

    $ systemctl restart networking


Ouvrir son navigateur à l'adresse www.monsite.com

