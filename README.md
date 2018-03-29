# Pair Bose QuietComfort 35 with Ubuntu over Bluetooth

## Get back to a clean state
   
* On Ubuntu, remove the headphones from the Bluetooth paired list.

* On the headphones, hold the switch in Bluetooth pairing position for 10 seconds to delete all paired devices (You'll get a voice confirmation).

## Desactivate Bluetooth LE (Low Energy)

Edit bluetooth configuration file:
```bash
sudo nano /etc/bluetooth/main.conf
```

Replace the following (remove the "#" symbol at beginning of the line if it exist):
```text
ControllerMode = dual
```
With:
```text
ControllerMode = bredr
```

Restart bluetooth service:
```bash
sudo service bluetooth restart
```

## Enable A2DP sink for stereo sound

Edit or create this file:
```bash
sudo nano /var/lib/gdm3/.config/pulse/client.conf
```
Insert these following lines in this file:
```text
autospawn = no
daemon-binary = /bin/true
```

Grant access to GDM user:
```bash
sudo chown gdm:gdm /var/lib/gdm3/.config/pulse/client.conf
```

Disable pulseaudio startup:
```bash
sudo rm /var/lib/gdm3/.config/systemd/user/sockets.target.wants/pulseaudio.socket
```

For Auto-connect A2DP, edit this file:
```bash
sudo nano /etc/pulse/default.pa
```
Insert this line at the end of the file:
```text
load-module module-switch-on-connect
```

Reboot now

## Install Blueman manager

via package manager:
```bash
sudo apt install blueman
```

To pair your headphone:

1) Open blueman
2) Right click on your headphone
3) select "headset"
4) hover "Audio profile" and select "A2DP"
