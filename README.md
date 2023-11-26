# Auto_Git_Backup_Your_Klipper_Printer-
## Backups with a single click and automatically on the hour every hour!


Follow this guide for the best instructions, warning this is not for beginners or the faint of heart! You will be giving commands directly to your Pi via terminal!
Huge thanks for the Voron team for sharing the guide below!

https://docs.vorondesign.com/community/howto/EricZimmerman/BackupConfigToGithub.html

BUT FIRST!!! Make sure your Pi has the correct timezone/time & date! Google should help you do this.


If you do all the setup & go for your fist autocommit but get “Updates were rejected because the tip of your current branch is behind” error. 
To get it working in MAIN branch after setup steps but before first `sh autocommit.sh` do this:

```
git config pull.rebase true
git pull <Token@URL>
git branch -m main
git push origin HEAD:main
sh autocommit.sh
```

Reset Git Repo for backups if you made a mistake or need to change repo:
```
git remote set-url origin https://<YOUR_NEW_TOKEN>@<YOUR_NEW_GIT_URL>
```


If you get “error: failed to push some refs to 'https://…….” on your first autocommit edit the `Autocommit.sh` file:

In Push config section at the very bottom replace the last line where it say `git push origin $branch` with 
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
Then paste in this save & exit. Reboot.
```
0 * * * * /usr/bin/bash /home/pi/printer_data/config/autocommit.sh >/dev/null 2>&1
```
