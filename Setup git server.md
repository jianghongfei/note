## Setup Git Gitolite Gitweb
## Prerequisites- Linux Server (192.168.1.110), git host.
- user `jiang.hongfei` on linux server will be used as git admin.

### Generate ssh keys
Login in as `jiang.hongfei`, execute the following command:

	ssh-keygen -t rsa -C "Jiang Hongfei <jiang.hongfei@gmail.com>" # -C for comment

This will generate ssh keys at `/home/jiang.hongfei/.ssh/`

Copy the public key to a known place for later use:

	cp /home/jiang.hongfei/.ssh/id_rsa.pub /tmp/jiang.hongfei.pub

### Install git-core Gitolite
	sudo apt-get -y install git-core gitolite highlight gitweb

### Add git user
	sudo adduser \
	    --system \
	    --shell $(which bash) \
	    --gecos 'git version control' \
	    --group \
	    --disabled-password \
	    --home /home/git \
	    git

Switch to git:

	sudo su git

Run gitolite setup:

	gl-setup /tmp/jiang.hongfei.pub

### Adding users and repos

Go back to `jiang.hongfei` by:

	exit

To administer your gitolite installation, start by doing this on linux server by `jiang.hongfei`:

	git clone git@localhost:gitolite-admin

If you want to do this on another pc, say a windows, you have to copy both `id_rsa.pub` and `id_rsa.pub` generated in the first step to `C:\Users\<username>\.ssh`, then:

	git clone git@192.168.1.110:gitolite-admin

> NOTE: if you are asked for a password, somthing went wrong.

Now if you `cd gitolite-admin`, you will see two subdirectories in it: `conf` and `keydir`.

To add new users alice, bob, and carol, obtain their public keys and add them to **keydir** as alice.pub, bob.pub, and carol.pub respectively.

To add a new repo `foo` and give different levels of access to these users, edit the file `conf/gitolite.conf` and add lines like this:

	repo foo
    	RW+         =   alice
    	RW          =   bob
    	R           =   carol

Once you have made these changes, do something like this:

	git add .
	git commit -m "added foo, gave access to alice, bob, carol"
	git push

When the push completes, gitolite will add the new users to ~/.ssh/authorized_keys on the server, as well as create a new, empty, repo called "foo".

More detail can be found at [Gitolite readme](https://github.com/sitaramc/gitolite#readme)

### Install Gitweb

	sudo apt-get -y install highlight gitweb

In /home/git/.gitolite.rc make sure you have:

	$REPO_UMASK = 0027;

Gitweb is located at `usr/shared/gitweb`.

Edit the gitweb config to the locations of the project list and repos, and add the highlighting bit at the end of the file:

	sudo vim /etc/gitweb.conf
	# change $projectroot to /home/git/repositories
	# change $projects_list to /home/git/projects.list
	# Add Highlighting at the end
	$feature{'highlight'}{'default'} = [1];

Change gitolite instance to allow access for gitweb. First append www-data to git group so gitweb can access the repos, then change the permissions for git repos and the projects list, finally restart apache:

	sudo usermod -a -G git www-data
	sudo chmod g+r /home/git/projects.list
	sudo chmod -R g+rx /home/git/repositories

### Confgure Apache

	sudo cp /etc/apache2/conf.d/gitweb /etc/apache2/conf-available/gitweb.conf
	cd /etc/apache2/conf-enabled
	sudo ln -s ../conf-available/gitweb.conf

修改 `/etc/apache2/conf-available/gitweb.conf`如下：

	Alias /gitweb /usr/share/gitweb  
  
	<Directory /usr/share/gitweb>  
		Options +FollowSymLinks +ExecCGI  
		AddHandler cgi-script .cgi  
	</Directory> 

(I.e. adding "+" before +FollowSymLinks)

Make sure cgi is enabled, adn restart apache2

	sudo a2enmod cgi  
	sudo service apache2 restart

Then user can access [gitweb](http://192.168.1.110/gitweb/) throw browser.

Finally you need to tell gitolite which repo you want to show up in gitweb. To do this edit the gitolite.conf file from the gitolite-admin.git repo:

	repo testing
	  RW+ = @all
	  R = gitweb

For more information [Install git/gitolite/gitweb on Ubuntu 14.04](http://ubuntuforums.org/showthread.php?t=2244960)