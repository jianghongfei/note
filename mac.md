# Basic
### How to force quit application
Press Command + Alt + Esc, to open 'Force Quit Applications' window

### How to set $JAVA_HOME
FROM [How To Set $JAVA_HOME Environment Variable On Mac OS X](http://www.mkyong.com/java/how-to-set-java_home-environment-variable-on-mac-os-x/)
> `$ vim ./zsh_profile`
> `export JAVA_HOME=$(/usr/libexec/java_home)`
> `$ source .zsh_profile`  
> `$ echo $JAVA_HOME`

### How setup GlassFish
###### Install java\_ee_sdk
> `sh glassfish-4.0-unix.sh`

If get error *'This program requires DISPLAY envrionment variable to be set.'*, use the following command instead:

> `DISPLAY=:0 ./java_ee_sdk-7-unix.sh`
###### Start
> `glassfish4/bin/asadmin start-domain`