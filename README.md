# Pair Bose QuietComfort 35 with Ubuntu over Bluetooth

This project is aiming to resolve problems that you can encounter when pairing your headphone.

## 1) Get back to a clean state
   
* On Ubuntu, remove the headphones from the Bluetooth paired list.

* On the headphones, hold the switch in Bluetooth pairing position for 10 seconds to delete all paired devices (You'll get a voice confirmation).

## 2) Desactivate Bluetooth LE (Low Energy)

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

## 3) Enable A2DP sink for stereo sound

> Note: do this step only if you're using **gdm3**

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

## 4) Optional: Use hot-plugged devices like Bluetooth or USB automatically

This step aim to enable your headphone to auto-connect to your computer when you start it.

For Auto-connect A2DP, edit this file:
```bash
sudo nano /etc/pulse/default.pa
```

Insert following lines at the end:
```text
.ifexists module-switch-on-connect.so
	load-module module-switch-on-connect
.endif
```

After saving these changes, you must **reboot now**.

## 5) Optional: Install Blueman manager

> Note: I only tested this GUI but feel free to use whatever you want.

via package manager:
```bash
sudo apt install blueman
```

To pair your headphone:

1) Open blueman
2) Right click on your headphone
3) select "headset"
4) hover "Audio profile" and select "A2DP"

# Undo this configuration

If you want to reverse this configuration, you just need to follow steps in reverse order.

> Note: Thanks to issue [#2](https://github.com/Goldweavers/bose_qc35-ubuntu/issues/2), I added to repository ```pulseaudio.socket``` file which is removed at the end of step 3.
