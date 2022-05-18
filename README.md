# autoRestartScript
Modify a running script quicker. Removes the process of stopping & starting a script after editing & saving it.

Run autoRestart.py while editing to auto kill script and re start it after an edit/save. Set path and filename.

```python3
#!/usr/bin/python3

import colorama
from colorama import Fore, Style
import subprocess
import time

fntc_path = "/path/to/script/"
filename_to_check = "script.py"

global saved_timestamp
saved_timestamp = 0
while True:
    timestamp = subprocess.getoutput("ls -lsa %s --full-time | cut -d\" \" -f8" % filename_to_check)
    if((saved_timestamp != 0) and (saved_timestamp != timestamp)):
        print (Fore.RED + "KILL IT, TIMESTAMP CHANGED!!")
        subprocess.run(["killall", "screen"])
        print (Fore.RED + "RESTARTING!!!!")
        time.sleep(1)
        subprocess.run(["screen", "-S", "m", "-dm", fntc_path + filename_to_check])
    else:
       print(Fore.GREEN + "Status: all good, move along")

    psf = subprocess.getoutput("ps aux | grep -m 1 'bot/M'")
    print (Fore.GREEN + "saved timestamp " + str(saved_timestamp))
    print ("new timestamp " + str(timestamp))
    print ("" + str(psf))
    print ("\n\n")
    saved_timestamp = timestamp
    time.sleep(2)
```
