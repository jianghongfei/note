# Mac CH340
1. Download [driver](http://www.wch.cn/downloads.php?name=pro&proid=178)
2. Execute following command in terminal:

        sudo nvram boot-args="kext-dev-mode=1"

3. Reboot
4. After reboot, you should be able to see a new Serial deviced name:

        /dev/cu.wchusberialxxxxxx

5. Check `screen`:

        screen -v

6. If screen is not installed:

        brew install screen

7. Connect in terminal

        screen /dev/cu.wchusberialxxxxxx 115200