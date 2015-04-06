## Memcached

### Install
	sudo apt-get -y install memcached
> `memcached` is configured to run at boot by default

	% sudo ls -l /etc/rc*.d/*memcached
	lrwxrwxrwx 1 root root 19 2011-03-23 13:36 /etc/rc0.d/K20memcached -> ../init.d/memcached
	lrwxrwxrwx 1 root root 19 2011-03-23 13:36 /etc/rc1.d/K20memcached -> ../init.d/memcached
	lrwxrwxrwx 1 root root 19 2011-03-23 13:36 /etc/rc2.d/S20memcached -> ../init.d/memcached
	lrwxrwxrwx 1 root root 19 2011-03-23 13:36 /etc/rc3.d/S20memcached -> ../init.d/memcached
	lrwxrwxrwx 1 root root 19 2011-03-23 13:36 /etc/rc4.d/S20memcached -> ../init.d/memcached
	lrwxrwxrwx 1 root root 19 2011-03-23 13:36 /etc/rc5.d/S20memcached -> ../init.d/memcached
	lrwxrwxrwx 1 root root 19 2011-03-23 13:36 /etc/rc6.d/K20memcached -> ../init.d/memcached

### Inspecting Running Configuration
	$ echo "stats settings" | nc localhost 11211
	STAT maxbytes 67108864
	STAT maxconns 1024
	STAT tcpport 11211
	STAT udpport 11211
	STAT inter NULL
	STAT verbosity 0
	STAT oldest 0
	STAT evictions on
	STAT domain_socket NULL
	STAT umask 700
	STAT growth_factor 1.25
	STAT chunk_size 48
	STAT num_threads 4
	STAT stat_key_prefix :
	STAT detail_enabled no
	STAT reqs_per_event 20
	STAT cas_enabled yes
	STAT tcp_backlog 1024
	STAT binding_protocol auto-negotiate
	STAT auth_enabled_sasl no
	STAT item_size_max 1048576
	END
Or using telnet

	telnet localhost 11211
	stats
	quit    # to terminate the telnet session

### How to check version of memcached
	telnet localhost 11211
	version
	quit    # to terminate the telnet session


