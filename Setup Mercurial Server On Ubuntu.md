## Setup Mercurial server on Ubuntu

### Install mercurial and apache2
	sudo apt-get install apache2 apache2-utils mercurial

### Make sure cgi is enabled
	sudo apache2ctl -M

Looke for `cgi_module (shared)`, if it's not loaded, load it with:

	sudo a2enmod cgid

Then restart apache2:

	sudo service apache2 reload

### Setup mercurial workding direcotry

	cd /mnt/sd/
	sudo mkdir -p mercurial/repositories
	cd mercurial
	sudo cp /usr/share/doc/mercurial/examples/hgweb.cgi .
	sudo chmod a+x hgweb.cgi
	sudo vim hgweb.cgi

Change config in line 7:

	config = "/mnt/sd/mercurial/hgweb.config"   # Will be configured latter

*Note:* `/mnt/sd` is the disk where I want my code to be stored at, other place would be totally fine.

### Configure `hgweb.config`

	sudo vim hgweb.config

Change the content to:

	[collections]
	/mnt/sd/mercurial/repositories = /mnt/sd/mercurial/repositories

	[web]
	style = gitweb
	encoding = "UTF-8"

	allow_push = *
	push_ssl = false

### Change owner of mercurial to www-data

	sudo chown -R www-data:www-data /mnt/sd/mercurial

### Configuring Apache

	sudo vim /etc/apache2/conf-available/mercurial.conf

Fill with the following lines:

	ScriptAlias /hg "/mnt/sd/mercurial/hgweb.cgi"

	<Location /hg>
    	AuthType Basic
    	AuthName "Mercurial repositories"
    	AuthUserFile /mnt/sd/mercurial/hgusers   # Will be configured latter
    	Require valid-user
	</Location>

For details, please refer to [Publishing Mercurial Repositories](http://mercurial.selenic.com/wiki/PublishingRepositories)

Create a link to conf-enabled:

	sudo ln -s /etc/apache2/conf-available/mercurial.conf /etc/apache2/conf-enabled/mercurial.conf

Add users:

	cd /mnt/sd/mercurial
	# add the first user (will propmpt for password)
	sudo htpasswd -mc hgusers admin    # hgusers file will be created automatically
	# add other users (will prompt for password, too)
	sudo htpasswd -m hgusers jiang.hongfei

### Restart apache2

	sudo service apache2 restart

### Create a test repository

	cd /mnt/sd/mercurial/repositories
	sudo mkdir test
	cd test
	sudo hg init

### Summary
Now the user could open `http://localhost/hg` to see all repositories in `/mnt/sd/mercurial/repositories`; or clone a repository just like this:

	hg clone http://username:password@locahost/hg/test