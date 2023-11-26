# Auto_Backup_Your_Klipper_Printer
## Backups with a single click and automatically on the hour every hour!


Follow the guide in the link below for the best instructions. 

### WARNING: 
This is not for beginners or the faint of heart! You will be giving commands directly to your Pi via SSH terminal & creating your own access tokens & filling important infomation that has to be correct for any of this to work.  
Huge thanks for the Voron team for sharing the guide below!

https://docs.vorondesign.com/community/howto/EricZimmerman/BackupConfigToGithub.html

## BUT FIRST!!! Make sure your Pi has the correct timezone/time & date! Google should help you do this.
# ALSO manually select all your files in Mainsail & download them to your computer so you have a current & local backup of your configs BEFORE YOU DO ANYTHING ELSE!

To log into ssh on a SV06/+ or SV07/+ Klipper screen the default user is mks & password is makerbase. 

### ...Oh, & whatever you do DO NOT, I repeat, DO NOT run any update commands via ssh on the SV06/+ or SV07/+ Klipper screens! You have been warned. 

If you do all the setup & go for your first autocommit but get “Updates were rejected because the tip of your current branch is behind” error. 
To get it working in MAIN branch after setup steps but before first `sh autocommit.sh` do this:

```
git config pull.rebase true
git pull <Token@URL>
git branch -m main
git push origin HEAD:main
sh autocommit.sh
```
be sure to add your token & url in the above commands! The last two lines will basically do the same thing but they are there to test they both work. 

use this to reset/change Git Repo for backups if you made a mistake or need to change repo. In the link below you must edit it to contain your correct/new access token & correct/new git url as it mentions in the linked guide:
```
git remote set-url origin https://<YOUR_NEW_TOKEN>@<YOUR_NEW_GIT_URL>
```


If you get “error: failed to push some refs to 'https://…….” on your first autocommit edit the `Autocommit.sh` file:

In Push config section at the very bottom replace the last line where it says `git push origin $branch` with 
```
git push origin HEAD:main
```



Edited config_backup.cfg for RPi based systems. Instead of using the macro in the guide use this, better backup name:
```
[gcode_shell_command backup_printer]
command: /usr/bin/bash /home/mks/printer_data/config/autocommit.sh
timeout: 30
verbose: True

[gcode_macro BACKUP_PRINTER]
description: Backs up config directory GitHub
gcode:
     RUN_SHELL_COMMAND CMD=backup_printer
```


Edited commands for Sovol SV07/+ & SV06/+ Klipper Screens the ones in the guide will not work use these:
```
 wget -O /home/mks/printer_data/config/autocommit.sh https://raw.githubusercontent.com/EricZimmerman/VoronTools/main/autocommit.sh
```
```
 nano /home/mks/printer_data/config/autocommit.sh
```
```
 wget -O /home/mks/klipper/klippy/extras/gcode_shell_command.py https://raw.githubusercontent.com/th33xitus/kiauh/master/resources/gcode_shell_command.py
```


Edited config_backup.cfg for Sovol SV07/+ & SV06/+ Klipper Screens. Instead of using the macro in the guide use this, as it wont work plus better backup name:
```
[gcode_shell_command backup_printer]
command: /usr/bin/bash /home/mks/printer_data/config/autocommit.sh
timeout: 30
verbose: True

[gcode_macro BACKUP_PRINTER]
description: Backs up config directory GitHub
gcode:
     RUN_SHELL_COMMAND CMD=backup_printer
```




Shell command for running auto backups hourly on the hour:
```
crontab -e
```
Then for RPi systems paste this in at the bottom of the new file:
```
0 * * * * /usr/bin/bash /home/pi/printer_data/config/autocommit.sh >/dev/null 2>&1
```
This version for the SV06/+ SV07/+
```
0 * * * * /usr/bin/bash /home/mks/printer_data/config/autocommit.sh >/dev/null 2>&1
```
Now save & exit. Reboot.

Be warned this is fairly technical & requires some knowledge to do, please be careful. Any mistakes resulting in damage to the system because you did something is bad & totally on you!


