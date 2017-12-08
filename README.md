# Pair Bose QuietComfort 35 with Ubuntu over Bluetooth

## Get back to a clean state
   
* On Ubuntu, remove the headphones from the Bluetooth paired list.

* On the headphones, hold the switch in Bluetooth pairing position for 10 seconds to delete all paired devices (You'll get a voice confirmation).

## Desactivate Bluetooth LE (Low Energy)

Edit bluetooth configuration file:
```bash
sudo nano /etc/bluetooth/main.conf
```

remove # at begin if it exist and replace:
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
with content:
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
With content:
```text
load-module module-switch-on-connect
```

Finished, you just have to reboot !
