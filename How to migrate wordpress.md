# How to migrate www.mhccenter.com

## Backup old one
### Backup wordpress database

    mysqldump -u root -p dbname > mhccenter.sql

### Backup website files

    tar zcf mhccenter.tar.gz /var/www/wordpress

## Setup a clean wordpress
Using a pre-configured wordpress template from the vm provider is recommended.

Find out the following parameters from the manual or tutorial:

* root password for the system
* wordpress username and password
* ftp username and password
* root password for mysql
* username and password for wordpress to login mysql database

## Overwrite new wordpress
### Overwrite wp-content

    # un-zip backuped mhccenter website
    tar jxf mhccenter.tar.gz
    
    # delete new wp-content
    rm -rf /var/www/wordpress/wp-content
    
    # copy wp-content from mhccenter backup to new wordpress
    cp -r wp-content /var/www/wordpress
 
    # find out which group apache is running, say ftp
    ps aux | grep httpd
    
    # assign the group to wp-content, and ssign write privilege to it
    chown -R :ftp /var/www/wordpress/wp-content
    chmod -R 775 /var/www/wordpress/wp-content

### Overwrite wordpress database

    # drop existing wordpress database
    drop datapbase wordpress;
    
    # create a new one
    CREATE DATABASE wordpressDatabaseName CHARACTER SET utf8 COLLATE utf8_general_ci;
    
    # assign user
    GRANT ALL ON `wordpressDatabaseUsername`.* TO 'wordpressDatabaseName'@'localhost';
    FLUSH PRIVILEGES;
    
    # restore backuped wordpress database
    use wordpressDatabaseName;
    source mhccenter.sql
    
    # update to new ip or url
    SET @siteurl = 'http://101.200.238.123';
    SET @old = 'http://www.mhccenter.com';
    UPDATE wp_options SET option_value = replace( option_value, @old, @siteurl) WHERE   option_name = 'home' OR option_name ='siteurl';
    UPDATE wp_posts SET post_content = replace(post_content, @old, @siteurl);
    UPDATE wp_posts SET guid = replace(guid, @old, @siteurl);

    SET @old = 'http://182.92.99.165';  -- in case old ip is used
    UPDATE wp_options SET option_value = replace( option_value, @old, @siteurl) WHERE option_name = 'home' OR option_name ='siteurl';
    UPDATE wp_posts SET post_content = replace(post_content, @old, @siteurl);
    UPDATE wp_posts SET guid = replace(guid, @old, @siteurl);
    
     
### rt-theme
The replacing trick dosen't work with rt-theme. So you have to update the urls in rt-theme manully.
* Appearance / Weidgets
* RT-Theme / General Options / Logo
* RT-Theme / General Options / Footer
* RT-Theme / General Options / Footer Banner / Footer Banner Text
