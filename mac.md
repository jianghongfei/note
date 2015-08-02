# Basic
### How to force quit application
Press Command + Alt + Esc, to open 'Force Quit Applications' window

# Ports to install
- git
- mercurail
- grep
- nodejs
- npm
- tree
- vim
- wget
- zsh
- MacDown
- iTerm
- [MenuMeters](http://www.ragingmenace.com/software/menumeters/index.html)

### How to make finder search current folder only
Finder Preferences/Advanced tab/When performing a search:
chose *Search the Current Folder*

### How to set $JAVA_HOME
FROM [How To Set $JAVA_HOME Environment Variable On Mac OS X](http://www.mkyong.com/java/how-to-set-java_home-environment-variable-on-mac-os-x/)

	$ vim ./zsh_profile
	export JAVA_HOME=$(/usr/libexec/java_home)
	$ source .zsh_profile
	$ echo $JAVA_HOME

### How setup GlassFish
###### Install java\_ee_sdk

	sh glassfish-4.0-unix.sh

If get error *'This program requires DISPLAY envrionment variable to be set.'*, use the following command instead:

	DISPLAY=:0 ./java_ee_sdk-7-unix.sh

###### Start
	glassfish4/bin/asadmin start-domain

# Mac take screenshot

- `Command-Shift-3`, Take a screenshot of the screen, and save it as a file on the desktop
- `Command-Shift-4`, then select an area: Take a screenshot of an area and save it as a file on the desktop
- `Command-Shift-4`, then space, then click a window: Take a screenshot of a window and save it as a file on the desktop
- `Command-Control-Shift-3`, Take a screenshot of the screen, and save it to the clipboard
- `Command-Control-Shift-4`, then select an area: Take a screenshot of an area and save it to the clipboard
- `Command-Control-Shift-4`, then space, then click a window: Take a screenshot of a window and save it to the clipboard

In Leopard and later, the following keys can be held down while selecting an area (via `Command-Shift-4` or `Command-Control-Shift-4`):

- Space, to lock the size of the selected region and instead move it when the mouse moves
- Shift, to resize only one edge of the selected region
- Option, to resize the selected region with its center as the anchor point

# Find who's using a port
	lsof -i tcp:3000
	lsof -i :3000

# Find who's locking a file
	lsof <filename>

# Virtual machine network setting

## Virtualbox
1. Chose Setting/Network
2. Attached to: Bridged Adapter
3. Name: en0: Wi-Fi (AirPort)
4. In Unbunt, edit /etc/network/ineterfaces

		auto eth0
		
		iface eth0 inet static
		address 192.168.1.112
		gateway 192.168.1.1
		netmask 255.255.255.0
5. restart network service

		sudo /etc/init.d/networkding restart

## Vmware Fusion
1. Menu/Virtual Machine/Removable Devices/Network Adapter
2. Bridged Networking/Wi-Fi
3. In Unbunt, edit /etc/network/ineterfaces

		auto eth0
		
		iface eth0 inet static
		address 192.168.1.112
		gateway 192.168.1.1
		netmask 255.255.255.0
5. restart network service

		sudo /etc/init.d/networkding restart