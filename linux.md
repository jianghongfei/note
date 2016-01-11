## Linux
#### Tools to install
- `zsh vim curl build-essential`
- `apache2 php5`
- `mysql-server mysql-client`
- `postgresql`
- `mongod` 
> *\# To install mongo driver for php*  
  `sudo apt-get install php5-dev php5-cli php-pear`  
  `sudo pecl install mongo`  
- `nodejs npm`  
> *\# npm is nodejs' package manager, used to install less css*  
  `$ npm install less`  
  `$ npm install less@latest`

- `tree`
- `libgtk2.0-dev` #use d to compile `scite`
- `chromium-browser`
- `phpmyadmin`
- `openssh-client openssh-server`
- `git mercurial subversion`

#### Use consolas in Debian
- Applications / System Tools / Preferences / Advanced Settings / Font
- Hinting : Slight
- Antialiasing: Rgba

#### Adding SSH Keys to Authorized_Keys

	ssh-copy-id user@host
	
[Reference](http://askubuntu.com/questions/46424/adding-ssh-keys-to-authorized-keys)

#### Search string in subfolders
	grep -nr 'words to search' .

##### Replace string in subfolders
	find . -type f -exec sed -i 's/googleapis.com/useso.com/g' {} +
	find . -type f -print0 | xargs -0 sed -i 's/googleapis.com/useso.com/g'
	grep -rl --include='*.xml' matchstring somedir/ | xargs sed -i 's/string1/string2/g'

#### Change console resolution

	vim /etc/default/grub
	# uncommet line 25, and change resolution to desired resolution
	vim /etc/grub.d/00_header
	# search 'set gfxmode', insert the follwing line after it
	set gfxpayload=keep
	# update-grub
	sudo update-grub
	# reboot

#### Mount windows shared folder
[Reference](https://wiki.ubuntu.com/MountWindowsSharesPermanently)

	# create a mount fodler
	sudo mkdir /media/shared
##### Mounting unprotected (guest) network folders

	# Edit /etc/fstab file (with root privileges) to add this line:
	//servername/sharename /media/shared cifs guest,uid=1000,iocharset=utf8  0  0
	# mount
	sudo mount -a
##### Mount password protected network folders

	# Edit /etc/fstab file (with root privileges) to add this line:
	//servername/sharename /media/shared cifs username=msusername,password=mspassword,uid=1000,noauto,user,iocharset=utf8,sec=ntlm 0 0
	# mount
	sudo mount -a
##### Save user credentials in file

	# create a file for your remote servers logon credential:
	vim ~/.smbcredentials
	# enter your windows username and password in the file:
	username=<msusername>
	password=<mspassword>
	# change permission of the file to prevent unwanted access to your credentials:
	chmod 600 ~/.smbcredentials
	# Edit /etc/fstab file (with root privileges) to add this line:
	//servername/sharename /media/shared cifs credentials=/home/<ubuntuusername>/.smbcredentials,uid=1000,noauto,user,iocharset=utf8,sec=ntlm 0 0
	# mount
	sudo mount -a

## User management
#### How to chang shell
> `chsh <user>`

#### How to change password
> `passwd <user>`

#### How to add user to existing group
> `usermod -a -G group user`

### How to remove user from group
> `gpasswd -d user group`

### How to add new user
```
useradd -m user
useradd -m -g sudo -s `which zsh` jiang.hongfei
```

### How to disable sudo password
```
# add to the end of /etc/sudoer
username ALL=(ALL) NOPASSWD: ALL
```

### How to allow (.) in username
http://askubuntu.com/questions/405638/what-are-the-disadvantages-of-having-a-dot-in-a-user-name
sudo vim /etc/adduser.conf
Locate this line

	#NAME_REGEX="^[a-z][-a-z0-9]*$"
replace with

	NAME_REGEX='^[a-z]([-a-z0-9]|\.)*$'

### How to add new group
> `addgroup group`

### How to change owner of file/folder
> `chown -R [user]:[group] file/folder`

### How to change group of file/folder

#### How to set terminal resolution
in /etc/default/grub

    GRUB_GFXMODE=1280x720
    GRUB_GFXPAYLOAD_LINUX=keep
Then
> sudo update-grub

#### How to remove package by *`apt`*
> `apt-get remove --purge package_name`

#### How to install *`.deb`* package
> `sudo dpkg -i package.deb`

## Apache2 and PHP5
#### set permission to /var/www
> `sudo chmod 777 /var/www`

#### how to enable **`mod_rewrite`**
> `sudo a2enmod rewrite`

#### How to restart apache2 service
> `service apache2 restart`

#### change permission of /var/www
> `sudo usermod -a -G www-data <some_user>`
> `sudo chgrp -R www-data /var/www`
> `sudo chmod -R g+w /var/www`

OR

> `sudo chown -R yourname:www-data folder`
> `sudo chmod -R g+s folder`
First command changes owner and group Second command adds s attribute which will keep new files and directories within cake having the same group permissions

#### How to enable **`.htaccess`**
1. open `/etc/apache2/sites-available/default`
2. Change **`AllowOverride None`** of `/var/www/` directory to **`AllowOverride All`**

## PHP
#### How to find out *`extension-dir`*
> `$ php-config|grep extension-dir`

#### How to install and configure vsftpd
    apt-get install vsftpd
    service vsftpd stop
    cd /etc
    cp vsftpd.conf vsftpd.conf.bak
    vi vsftpd.conf
Make the following changes:

    anonymous_enable=NO
    local_enable=YES
    write_enable=YES
    local_umask=022
    connect_from_port_20=NO
    chroot_local_user=NO
Add the floowing lines to the end of the file and save it:

    userlist_file=/etc/vsftpd.userlist
    userlist_enable=YES
    userlist_deny=NO
Now that basic configuration is complete, begin with locking /securing a directory to user

    useradd -d /var/www/path_to_dir -s /bin/false ftpuser
Setup a password for ftpuser

    passwd ftpuser
Change permission and take ownership

    chown -R ftpuser /var/www/path_to_dir
    chmod 775 /var/www/path_to_dir
Create userlist file and add ftpuser

    vim vsftpd.userlist
    ftpuser
After completing all these procedures it is almost ready to use it, give it a try but you will get a 500 OOPS permission denied error. To fix it you need to add a nologin to the shell set.

    vim /etc/shells
add the following line to the end:

    /sbin/nologin
Create a usergroup and add ftpuser to it

    addgroup ftpusers
    usermod -Gftpusers ftpuser
Now start the vsftpd
    service vsftpd start
