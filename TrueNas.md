# TrueNas installation

## Get the image

```bash
https://www.truenas.com/truenas-community-edition/
```

Warning this image do not work on ventoy.
-> Flash with balenaEtcher

## Install

Disable secure boot (BIOS > Boot > Secure Boot > OS Type : Other OS
Restart with USB stick in the server

Select 1 - Administrative user (truenas_admin)
Warning ! Password will be QWERTY

# Retrieve config:

System > General Settings > Manage Configuration (top-right) > upload file (on the computer)


# Consumption

Check states
```bash
powertop --calibrate
---- REBOOT
powertop --calibrate
powertop --auto-tune
powertop
```
Check tab Idle stats > Pkg (HW) shall be above C3

Check devices that support ASPM
```bash
sudo lspci -vvv | grep "ASPM .*abled"
sudo lspci -vvv | grep "ASPM"
lspci -vv | awk '/ASPM/{print $0}' RS= | grep --color -P '(^[a-z0-9:.]+|ASPM )'
```

Check available governors
```bash
cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```

Add a postinit command in System Settings - Advanced - Init/Shutdown Scripts:
```bash
echo "performance" | tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```
