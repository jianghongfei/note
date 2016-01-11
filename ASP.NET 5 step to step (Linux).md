# ASP.NET 5 step to step (Linux)

## Install Node and npm
All releases of nodejs can be found at their [Previous Release](https://nodejs.org/en/download/releases/) section. Before download, you need to find out which version of linux you're working on:

    $ uname -p
    
 Say [node js v4.2.4](https://nodejs.org/download/release/v4.2.4/) is the one you need, and `uname -p` gives `x86_64`, then *[node-v4.2.4-linux-x64.tar.gz](https://nodejs.org/download/release/v4.2.4/node-v4.2.4-linux-x64.tar.gz)* is the one need to be downloaded.

Follow these steps:

    $ cd ~
    $ mkdir Downloads | cd $_
    $ wget https://nodejs.org/download/release/v4.2.4/node-v4.2.4-linux-x64.tar.gz
    $ tar zxf node-v4.2.4-linux-x64.tar.gz
    $ sudo mv node-v4.2.4-linux-x64 /usr/local/share/
    $ sudo ln -s /usr/local/share/node-v4.2.4-linux-x64/bin/node /usr/local/bin/node
    $ sudo ln -s /usr/local/share/node-v4.2.4-linux-x64/bin/npm /usr/local/bin/npm

`node` and `npm` command should work now:

    $ node -v && npm -v
    v4.2.4
    2.14.12

## Install dnvm

Follow instructions on [ASP.NET Document](https://docs.asp.net/en/latest/getting-started/installing-on-linux.html)

**NOTE:** make sure `Install libuv` step is done first. This caused me some truble at first, and I also saw Scott Hanselman complained this in ASP.NET Community Standup 

## Create an aspnet project

    $ npm install -g yo
    $ npm install -g generator-aspnet
    $ mkdir demo | cd $_
    $ yo aspnet
    # chose Web Application and enter demo as project name

To run this project, follow these commands:

    cd demo
    dnu restore
    dnx web

There're more convenient sub-generators available: [Sub Generators](https://github.com/omnisharp/generator-aspnet#sub-generators)

In this generated project, they showed how to use entity framework 7 with sqlite which is the only option for mac and linux currently, and how to read from `appsettings.json` instead of the old-fashioned `web.config`.

To publish this project to filesystem:

    dnu publish --configuration Release --no-source

Make sure to remove un-used scripts in `scripts/prepublish` section of project.json, otherwise the publish might fail.

By default, the plublished files will be save to `bin/output` folder, this behavior can be override by specify `--out` argument.

## Add typescript and jspm support
Since package.json has been created in Web Application template, you don't need to init npm, but if package.json wasn't been created in the chosen template, it can be created by the `init` command:

    $ npm init

**NOTE:** unwanted sections can be manully deleted from `package.json`.

There're already some dependencies in `package.json` like `gulp`, install them by:

    $ npm install

### Add typescript, tsd and jspm:

    $ npm install --save-dev typescript tsd jspm

### Init tsd

    $ $(npm bin)/tsd init
    $ $(npm bin)/tsd install angular-ui-router --save
    
**NOTE:** `tsd` can resolve dependencies by default, so angular and jquery will be installed automatically.

### Monitor typescript file change
    
Create `tsconfig.json` in project root:

```
{
    "compilerOptions": {
        "target": "es5",
        "module": "system"
    }
}
```

Then start `tsc` in watch mode to transpile typescript to javascript automatically if any change happens.

    $ tsc -p . -w


### Init jspm   
    
    $ `npm bin`/jspm init

**NOTE:** specify `wwwroot` for `public folder path`, `no` for `transpiler`, and default to all other options.

It seems jspm cannot resolve dependencies, so every package has to be installed manually.

    $ $(npm bin)/jspm install angular=npm:angular
    $ $(npm bin)/jspm install angular-ui-router=npm:angular-ui-router
    $ $(npm bin)/jspm install juqery=npm:jquery
    $ $(npm bin)/jspm install ocLazyLoad=npm:oclazyload
    $ $(npm bin)/jspm install ui-router-extras=npm:ui-router-extras

To avoid $(npm bin) everytime, follow `Tip 1`.

## Install vsoagent
As the vsoagent-installer document stated, it works for npm v0.12.0, so follow **Install Node and npm** section to install [nodejs v0.12.0](https://nodejs.org/download/release/v0.12.0/).

`vsoagent` cannot run under an elevated user such as root, so create a dedicated user `vso` for it:

```
useradd -m -g sudo -s $(which bash) vso
passwd vso
```

Then login with `vso` and follow the [instructions](https://www.npmjs.com/package/vsoagent-installer) to install and setup.

## Tips
1. Folder-specific profile for bash
    To add a specific folder to `$PATH` everytime you enter a folder, the `cd` command needs to be overridden in bash.
    Add the following code to `~/.profile`:
    
    ```
    export OLD_PATH=$PATH
    cd() { building cd "$@" && chpwd; }
    chpwd() {
         if [[ -e .profile ]]; then
              source .profile
         else
              export PATH=$OLD_PATH
         fi
    }
    ```
    
    This piece of code will check if there's `.profile` in the current folder after `cd`. If it exists, just execute it; otherwise restore `$PATH` to the saved one.
    
    Then in a `npm` project, you can add `.profile` to the root:

    ```
    if [ -d 'node_modules' ]; then
         export OLD_PATH=$PATH
         export PATH=$(pwd)/node_modules/.bin:$PATH
    fi
    ```

    This piece of code will add `node_modules/.bin` to `$PATH` if there's `node_modules` folder, then all commands under `node_modules/.bin` are available to you.
    
1. Microsoft.AspNet.Server.Kestrel.Networking.UvException: Error -48 EADDRINUSE address already in use...
    This is because 5000 port has been occupied by other process, usually another un-terminated dnx process. Find out the pid of it and kill it will do the job.
    
    ```
    $ lsof -i :5000
    COMMAND   PID          USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
    dnx     31466 jiang.hongfei  117u  IPv4 0x2fc5a5de1894145f      0t0  TCP localhost:commplex-main (LISTEN)
    
    $ kill -9 31466
    ```
1. How to make `<reference />` globally available in `code`
    Create `tsd.d.ts` at the project root, put all `<reference />` in it, then you don't need to add individual `<reference />` to each typescript file:
    
    ```
    /// <reference path="typings/angularjs/angular.d.ts" />
    /// <reference path="typings/jquery/jquery.d.ts" />
    /// <reference path="typings/angular-ui-router/angular-ui-router.d.ts" />
    ```
 
