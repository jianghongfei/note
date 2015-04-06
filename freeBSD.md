## FreeBSD ##

### Disable sendmail completely ###

Edit `/etc/rc.conf`:

    sendmail_enable="NO"  
    sendmail_submit_enable="NO"  
    sendmail_outbound_enable="NO"  
    sendmail_msp_queue_enable="NO"

### [High resolution console](http://www.freebsdwiki.net/index.php/High_Resolution_Console) ###

#### See Available Video Modes & Pick One ####

    vidcontrol -i mode | less

Say you want 1024x768x24, which is 280

    vidcontrol MODE_280

#### Tell FreeBSD to use the video mode all the time ####
Edit `/etc/rc.conf`:

    allscreens_flags="MODE_280"

### Search Package ###

    pkg search vim

### Install Package ###

    pkg install zsh vim git mercurial curl

