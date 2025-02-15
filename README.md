# Windows Sub System (WSS) 

You're a secret neckbeard and want to seem normal to your colleagues? Reddit is your main source of news? 
Keep reading!

Prereqs: Qemu, KVM, perms, windows ISO file

1. Create VM disk
      
```qemu-img create -f qcow2 myvm.qcow2 60G```

2. Boot
      
```qemu-system-x86_64   -enable-kvm   -m 8096   -cpu host   -smp 4   -hda myvm.qcow2   -cdrom ~/Downloads/tiny.iso   -boot d``` 

3. Start

```qemu-system-x86_64   -enable-kvm   -m 6144   -cpu host   -smp 4   -hda myvm.qcow2   -boot c``` 

4. Have fun! If you ever break something you can always re-install using the boot d (floppy).

I tried getting WSL running and that wasnt a good idea. 

**WARNING: DO NOT TOUCH VIRTUALIZATION SETTINGS AS THAT WILL BREAK YOUR VM**

CTRL ALT G to lock instance + CTRL ALT F for fullscreen


----

I tested up to 8 and 10 vCPUs and my laptop barely broke the 40% CPU/ 50% RAM point using the Tiny11 install
Also does eat up quite a bit of ram but that was expected... On install it will break the 90%.

![Screenshot from 2025-01-22 03-54-26](https://github.com/user-attachments/assets/54353e1a-fde2-4465-abe5-6ec55060734e)

**1. Python executable**

I recommend to not use the Windows Store as that will install it in a weird path that is not recognized.
Instead go to python.org and tick all the Admin required options when running the installer.

**2. Python extensions on VSCode**

Get them so that your venvs activate by themselves.


To share a drive with the guest machine:

      sudo apt-get install samba

```qemu-system-x86_64 -enable-kvm -m 6144 -cpu host -smp 8 -hda myvm.qcow2 -boot c -net nic -net user,smb=/home/hadepop/Desktop/vm/shared``` 

Then naviguate in file explorer to: ```  \\10.0.2.4\qemu ``` 

This way you don't even need clipboard share.

![image](https://github.com/user-attachments/assets/c256c442-0cf6-48d6-b8c7-562038389b09)

```  
import random

x = "Hello from Python in Windows on Linux"
#print(f'{x}')
y = random.choice(x)
print(f'{y}')

import sys

print(f"Exec: {sys.executable}")

## Use absolute paths as they are more likely not to change. 
import subprocess
#subprocess.run(['C:\\Program Files\\Mozilla Firefox\\firefox.exe'])

### Run powershell scripts directly
#subprocess.run(['powershell.exe', 'Start-Process firefox'])

## Simplest Hello World Search

import time
#import pyautogui as pag

#time.sleep(2)  # Wait for Firefox to open
#pag.write('Hello World')
#time.sleep(1)
#pag.press('enter')

```

If the whole set-up works it's very cool! 

You a truly on shared system between the Windows and Linux:

``` 
If you see this you're a G : (.venv) PS Microsoft.PowerShell.Core\FileSystem::\\10.0.2.4\qemu> 
```

On the guest system you will have to enable PS scripts in settings (Settings > Then type in the search bar Powershell)

Then run the script again and get output:
```
(.venv) PS Microsoft.PowerShell.Core\FileSystem::\\10.0.2.4\qemu> & //10.0.2.4/qemu/.venv/Scripts/python.exe //10.0.2.4/qemu/hello.py       
H
Exec: \\10.0.2.4\qemu\.venv\Scripts\python.exe
```  

If you got here congrats :)

If you didn't you need to run a PowerShell in admin then:

```Set-ExecutionPolicy RemoteSigned``` 
Yes the good things behind a single toggle or several... And a few downloads. 

![image](https://github.com/user-attachments/assets/68dd3dfc-7f01-4f79-98e9-a34f3e26cc36)

I don't know how safe any of this is in practice but since this is a VM set-up with a barebones iso... Well you get the idea. 

Now if you want to learn about Powershell commands the easiest way is simply to run a powershell and enter ISE.
This opens a graphical interface with every single command possible...

Then click the little help icons that will link to Microsoft website for usage information. 

## For further Windows clean-up:

Settings > Privacy & Security > Search Permissions
Tick off everything but local search. 

Go to Microsoft Store > Search WinToys
Browse wintoys tweaks 

Finally I hate the shortcuts on Windows: You can remap them, of course behind a download:
Go to Microsoft Store > Search PowerToys
Browse keyboard enable then remap

## Clean directory:
----

```
📦 VM
├─ myvm.qcow2 # Reserved space for VM
├─ tiny.iso # Bootable W11 ISO
└─ shared
   ├─ .venv
   └─ scripts
```

**Notes:**

Windows will reboot 3 times during install make sure to not close the VM and lit it do it's thing until the very end. It will also consume unhealthy amounts of RAM/CPU during install. All normal. 

Make sure to change boot order to not install twice once you're in. 

Create a desktop shortcut to your shared folder is practical! You can also directly open it in VSCode. 

Scripts
---


I've included some bonuses in the repo:
1. CTRL + H to go to desktop and hide current active windows (Inspired by HideFromMyBoss with some optimization)

This can be done by default using SUPER key + D (but it doesn't actually hide your windows) 

Tool for better UI:
---

https://www.startallback.com/


# WSL On another machine

Go to PowerShell in Admin > wsl --install Ubuntu --web-download

You can also use debian instead of ubuntu.

Install https://vcxsrv.com/

It will prompt you to reboot at some point. 

Then run again: wsl --install Ubuntu

THis time it will say something different: 
```
PS C:\WINDOWS\system32> wsl --install Ubuntu
Ubuntu is already installed.
Launching Ubuntu...
Installing, this may take a few minutes...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username:
```

Set a username and password.

Do the regular update, upgrade.

## Try Wubuntu !
