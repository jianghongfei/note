## Linux SD卡
常用命令

- df -h
- fdisk -l
- mount -l
- mount /dev/mmcblk1 /mnt/sd

### To mount drive at boot
Edit /etc/fstab, and append this line:

	/dev/mmcblk1 /mnt/sd ext4 defaults 0 0
	
### To list all block devices
	lsblk