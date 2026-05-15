> [!NOTE]
> Friday, 15/05/2026, the issues seem to be fixed, I don't notice any crash with Jamesdsp nor need to restart pipewire

# Jamesdsp / Pipewire randomly cut off / crackling
 System Info:
 - Distro: Fedora Linux 42 (KDE Plasma Desktop Edition)
 - Kernel: 6.17.7

> [!WARNING]
> Be warned, that those lines prevent underruns by highering the output latency alot. 
https://github.com/Audio4Linux/JDSP4Linux/issues/47#issuecomment-2565036420

> [!CAUTION]
> Please read through pipewire config docs with ```man pipewire.conf``` before doing any changes.
> 
> Do not simply copy and paste the provided values into the config file
> 
> Configuration files are structured to expect specific values in specific areas, and placing them in the wrong section could lead to unexpected behavior (trust me bro 💀)

## Fix:
### Step 1:
Install realtime-setup:
```sudo dnf in realtime-setup```

Add your user into realtime group with:
```sudo usermod -aG realtime <user>```

### Step 2:
Copy /usr/share/pipewire/pipewire.conf to /etc/pipewire/pipewire.conf

And then uncomment & edit these properties: 
```
    loop.rt-prio = -1
    default.clock.quantum       = 2048 #1024
    default.clock.min-quantum   = 1024 #32
    default.clock.max-quantum   = 4096 #2048
```
Finally, reboot your system to apply changes.

