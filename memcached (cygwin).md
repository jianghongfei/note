## Memcached (cygwin)

prerequisites:  

- [libevent](http://libevent.org/)  
- [Memchached](http://memcached.org/)

### Install libevent  
	tar xf libevent-2.0.22-stable.tar.gz
	./configure --prefix=/usr/local/libevent
	make
	make install

### Install memcached
	tar xf memcached-1.4.22.tar.gz

Configure with [static linkage](http://archives.seul.org/libevent/users/Sep-2010/msg00020.html)

	export CFLAGS='-static -static-libgcc'

Then just add a cast to int in sanitycheck (memcached.c line 5013) [Reference](http://stackoverflow.com/questions/6869655/building-memcached-1-4-x-on-windows-error-with-cygwin)
  
	!isdigit((int)ever[3]))

Then

	./configure --prefix=/usr/local/memcached --with-libevent=/usr/local/libevent
	make
	make install