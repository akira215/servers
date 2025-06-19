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

Mettre le shell en clavier francais
```bash
setxkbmap fr
```

# Consumption

Check states
```bash
powertop --calibrate
---- REBOOT
powertop --calibrate
powertop --auto-tune
powertop
```
Check tab Idle stats > Pkg (HW) shall be above C3. Check on server, disconnecting the network cable for a while !

Check devices that support ASPM
```bash
sudo lspci -vvv | grep "ASPM .*abled"
sudo lspci -vvv | grep "ASPM"
lspci -vv | awk '/ASPM/{print $0}' RS= | grep --color -P '(^[a-z0-9:.]+|ASPM )'
```

Find a driver corresponding on a device using `/sys`
```bash
$  lspci
...
02:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168B PCI Express Gigabit Ethernet controller (rev 01)
$ find /sys | grep drivers.*02:00
/sys/bus/pci/drivers/r8169/0000:02:00.0
```

Optimize realtek drivers
Refer to script
find ID of te network adapter running `lspci`
```bash
sudo echo 1 > /sys/bus/pci/devices/0000:01:00.0/link/l1_aspm
```


Check available governors
```bash
cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```

Add a postinit command in System Settings - Advanced - Init/Shutdown Scripts:
```bash
echo "performance" | tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```
