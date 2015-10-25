# Work with express, nginx on ubuntu

@(Programming)[Express|Ubuntu|Node.js|Nginx]

## Install Node.js

``` text
% sudo apt-get update
% sudo apt-get install -y npm
```
By default, it's `nodejs`, but what we need is `node`, so we need to create a link:

``` text
% sudo ln -s /usr/bin/nodejs /usr/bin/node
```
## Test node
Create a folder to hold all the application files:

``` shell
% mkdir ~/example && cd $_
% vim example.js
```
Copy the example code from [Node.js](https://nodejs.org/) to the `example.js` just crated:

``` javascript
var http = require('http');
http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/plain'});
    res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```
Issue the following command to start the server:

``` text
% node example.js
Server running at http://127.0.0.1:1337/
```
See what we get from a browser:

``` shell
% curl http://localhost:1337
Output:
Hello World
```
**NOTE:** quit node before going forward

## Install PM2
**PM2** is a process manager for Node.js applications. It provides an easy way to manage and daemonize applications (run them as a service).

``` text
% sudo npm install pm2 -g
```
### Use PM2 to do mangaement

``` text
% pm2 start example.js
┌──────────┬────┬──────┬──────┬────────┬───────────┬────────┬────────────┬──────────┐
│ App name │ id │ mode │ PID  │ status │ restarted │ uptime │     memory │ watching │
├──────────┼────┼──────┼──────┼────────┼───────────┼────────┼────────────┼──────────┤
│ example  │ 0  │ fork │ 5871 │ online │         0 │ 0s     │ 9.012 MB   │ disabled │
└──────────┴────┴──────┴──────┴────────┴───────────┴────────┴────────────┴──────────┘
```
The `startup` subcommand of PM2 generates and configures a startup script to launch PM2 and its managed processes on server boots. You must also specify the platform you are running on, which is ubuntu, in our case:

``` text
% sudo pm2 startup ubuntu
```
The following commands of PM2 might be usefull:
- start
- stop
- restart
- list
- delete

To see full list of options:

``` text
% pm
```
## Setup Nginx

``` text
% sudo apt-get install nginx
% sudo vim /etc/nginx/sites-available/default
```
If there's only one app, you can just delete everything in the file, and replace it with:

``` nginx
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
```
Or you can just add a `location`, like:

``` nginx
location ^~ /example {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_set_header X-NginX-Proxy true;
    proxy_pass    http://127.0.0.1:1337/;
}
```
Then restart nginx:

``` text
% sudo service nginx restart
```
In the later case, you can access example throught: [http://localhost/example](http://localhost/example)

## Use yoeman to create express application

``` text
% npm install -g yo
% npm install -g generator-express
```
Use yoeman to create application:

``` text
% yo express
```
Then we can use rsyn to copy it to test/production environment hosted on a ubuntu:

``` text
% rsync -ar example jiang.hongfei@firefly:www
```
Since the entrance file created by `yo express` is `app.js`, so we have to delete old `example.js` app from PM2, and start with this `app.js`:

``` text
% pm2 delete example
% cd ~/example
% sudo pm2 start app.js 
```
Go to [http://localhost/example](http://localhost/example) and see what's there.
