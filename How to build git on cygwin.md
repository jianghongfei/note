# How to build git on cygwin

## Install tools & libs
- gcc
- make
- pyton
- perl
- ncurses
- libiconv-devel
- openssl-devel
- libcurl-devel
- curl
- curl-dev
- expat
- expat-devel
- tcl-tk


## Prepare src
- wget https://github.com/git/git/archive/v2.3.2.tar.gz
- tar xf v2.3.2.tar.gz
- ln -s /usr/bin/perl git-2.3.2/perl/0    # otherwise it will complain during make

## Make & install
make NO_MSGFMT=YesPlease NO_GETTEXT=YesPlease prefix=/usr/local install 

## Install man
	git clone https://github.com/gitster/git-manpages.git
	mkdir -p /usr/local/share/man
	mv ./man* /usr/local/share/man
