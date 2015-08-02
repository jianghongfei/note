# Node.js with Nginx
### Install Node.js
	sudo apt-get update
	sudo apt-get install -y npm
By default, it's `nodejs`, but what we need is `node`, so we need to create a link:

	sudo ln -s /usr/bin/nodejs /usr/bin/node

#### Test node
Copy the example code from [Node.js](https://nodejs.org/) to a `example.js`:

	var http = require('http');
	http.createServer(function (req, res) {
  		res.writeHead(200, {'Content-Type': 'text/plain'});
  		res.end('Hello World\n');
	}).listen(1337, '127.0.0.1');
	console.log('Server running at http://127.0.0.1:1337/');

Issue the following command to start the server:

	% node example.js
	Server running at http://127.0.0.1:1337/

See what we get from a browser:

	% curl http://localhost:1337
	Output:
	Hello World

**NOTE:** quit node before going forward

### Install PM2
**PM2** is a process manager for Node.js applications. It provides an easy way to manage and daemonize applications (run them as a service).

	sudo npm install pm2 -g

#### Use PM2 to do mangaement

	% pm2 start example.js
It will give you:

	┌──────────┬────┬──────┬──────┬────────┬───────────┬────────┬────────────┬──────────┐
	│ App name │ id │ mode │ PID  │ status │ restarted │ uptime │     memory │ watching │
	├──────────┼────┼──────┼──────┼────────┼───────────┼────────┼────────────┼──────────┤
	│ example  │ 0  │ fork │ 5871 │ online │         0 │ 0s     │ 9.012 MB   │ disabled │
	└──────────┴────┴──────┴──────┴────────┴───────────┴────────┴────────────┴──────────┘
The `startup` subcommand of PM2 generates and configures a startup script to launch PM2 and its managed processes on server boots. You must also specify the platform you are running on, which is ubuntu, in our case:

	% sudo pm2 startup ubuntu

The following commands of PM2 might be usefull:
- start
- stop
- restart
- delete

To see full list of options:

	% pm

### Setup Nginx
	% sudo apt-get install nginx
	% sudo vim /etc/nginx/sites-available/default
If there's only one app, you can just delete everything in the file, and replace it with:

    server {
        listen 80;

        server_name localhost;

        location / {
            proxy_pass http://127.0.0.1:1337;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }

Or you can just add a `location`, like:

    location /example {
        proxy_pass http://127.0.0.1:1337;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
Then restart nginx:

	% sudo service nginx restart
In the later case, you can access example throught: [http://localhost/example](http://localhost/example)