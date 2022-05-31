# autoRestartScript -- Automatic script restarting.
Modify a running script quicker. This removes the process of stopping & starting a script after editing & saving it.

Run autoRestart.py while editing to auto kill script and re start it after an edit/save. Set path and filename.

```python3
#!/usr/bin/python3

import time
import subprocess
import colorama
from colorama import Fore, Style

filename = "/path/to/script.py"

saved_timestamp = 0
while True:
    timestamp = subprocess.getoutput("ls -lsa %s --full-time | cut -d\" \" -f8" % filename)
    ps = subprocess.getoutput("ps aux | grep -m 1 '%s' | tr -s ' ' | cut -d ' ' -f2" % filename) # UPGRADE

    if((saved_timestamp != 0) and (saved_timestamp != timestamp)):
        print (Fore.RED + "KILL IT, TIMESTAMP CHANGED!!")
        subprocess.run(["kill", "-9", ps])
        subprocess.run(["screen", "-wipe"], stdout=subprocess.DEVNULL, stderr=subprocess.STDOUT)
        print (Fore.RED + "RESTARTING!!!!")
        time.sleep(1)
        subprocess.run(["screen", "-S", "m", "-dm", filename])

    else:
       print(Fore.GREEN + "Status: all good, move along")

    psf = subprocess.getoutput("ps aux | grep -m 1 '%s'" % filename)
    print (Fore.GREEN + "Saved timestamp " + str(saved_timestamp))
    print ("New timestamp " + str(timestamp))
    print (str(psf) + "\n\n")
    saved_timestamp = timestamp
    time.sleep(2)
```
