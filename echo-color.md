#Echo color

[More could be found](http://misc.flogisoft.com/bash/tip_colors_and_formatting)

	# Reset
	Reset_all='\033[0m'
	Reset_bold_bright='\033[21m'
	Reset_dim='\033[22m'
	Reset_underlined='\033[24'
	Reset_blink='\033[25'
	Reset_reverse='\033[27'
	Reset_hidden='\033[28'

	# Regular Colors
	Black='\033[30m'        # Black
	Red='\033[31m'          # Red
	Green='\033[32m'        # Green
	Yellow='\033[33m'       # Yellow
	Blue='\033[34m'         # Blue
	Purple='\033[35m'       # Purple
	Cyan='\033[36m'         # Cyan
	White='\033[37m'        # White

	# Bold
	BBlack='\033[30m'       # Black
	BRed='\033[31m'         # Red
	BGreen='\033[32m'       # Green
	BYellow='\033[33m'      # Yellow
	BBlue='\033[34m'        # Blue
	BPurple='\033[35m'      # Purple
	BCyan='\033[36m'        # Cyan
	BWhite='\033[37m'       # White

	# Underline
	UBlack='\033[30m'       # Black
	URed='\033[31m'         # Red
	UGreen='\033[32m'       # Green
	UYellow='\033[33m'      # Yellow
	UBlue='\033[34m'        # Blue
	UPurple='\033[35m'      # Purple
	UCyan='\033[36m'        # Cyan
	UWhite='\033[37m'       # White

	# Background
	On_Black='\033[40m'       # Black
	On_Red='\033[41m'         # Red
	On_Green='\033[42m'       # Green
	On_Yellow='\033[43m'      # Yellow
	
### Example
	echo "H\033[32mello stackover\033[0mflow"
![][0]
	
	
<!-- images -->
[0]:data:image/png;base64,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