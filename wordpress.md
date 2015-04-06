
mysql -u root -p<password>
CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'masterkey';
CREATE DATABASE wordpress CHARACTER SET utf8 COLLATE utf8_general_ci;
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'localhost';
FLUSH PRIVILEGES;
exit

cd /var/www

sudo wget http://wordpress.org/latest.tar.gz
sudo tar xfz latest.tar.gz
#sudo rm latest.tar.gz

cd wordpress

sudo find . -type f -exec sed -i 's/googleapis.com/useso.com/g' {} +
sudo chown -R www-data:www-data .
sudo chmod -R g+s .

sudo bash -c 'cat << EOF > /etc/apache2/conf-available/wordpress.conf
# wordpress configuration
Alias /wordpress /var/www/wordpress

<Directory /var/www/wordpress>
        Options FollowSymLinks
        DirectoryIndex index.php

        <IfModule mod_php5.c>
                AddType application/x-httpd-php .php

                php_flag magic_quotes_gpc Off
                php_flag track_vars On
                php_flag register_globals Off
        </IfModule>

</Directory>
EOF'

sudo ln -s /etc/apache2/conf-available/wordpress.conf /etc/apache2/conf-enabled/wordpress.conf

sudo service apache2 restart
