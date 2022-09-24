# Flash Shelly 1

## Prep

1. Raspbian
2. Good power supply! Else the flash will randomly fail half way.
3. Jumper cables

## Enable the serial port

```
sudo raspi-config
```

- The serial login shell is disabled
- The serial interface is enabled

Reboot.

## Download Shelly firmware

```
wget https://github.com/bglnelissen/shelly_flash/raw/main/SHSW-1-restore-1.3.0.bin
```

## Install esptool (latest)

```
sudo apt-get upgrade
sudo apt-get --yes install git pip
python3 -m pip install pyserial
python3 -m pip install esptool
```

Logout / login to get the $PATH set.

## Connect the Shelly

```
GND    -   GND
3V3    -   3V3
TX     -   RX
RX     -   TX
GND    -   GPIO 0
``` 

1. Connect the Shelly with the GPIO 0 pin set to ground (connect wire 4 & 5). Now the Shelly boots in dev mode
2. Test the Shelly: `esptool.py --port /dev/ttyS0 chip_id`

## Flash the Shelly

_verwacht vaak connectie problemen, dit gaat niet echt in 1x vlekkeloos_

1. Connect the Shelly
2. Flash the Shelly with the downloaded firmware

```
cd ~
esptool.py --port /dev/ttyS0 write_flash 0x00000 ~/SHSW-1-restore-1.3.0.bin
```

Expect:

```
esptool.py v4.3
Serial port /dev/ttyS0
Connecting...
Failed to get PID of a device on /dev/ttyS0, using standard reset sequence.
.
Detecting chip type... Unsupported detection protocol, switching and trying again...
Connecting...
Failed to get PID of a device on /dev/ttyS0, using standard reset sequence.

Detecting chip type... ESP8266
Chip is ESP8266EX
Features: WiFi
WARNING: Detected crystal freq 42.28MHz is quite different to normalized freq 40MHz. Unsupported crystal in use?
Crystal is 40MHz
MAC: 68:c6:3a:fb:02:9d
Uploading stub...
Running stub...
Stub running...
Configuring flash size...
Flash will be erased from 0x00000000 to 0x001fffff...
Compressed 2097152 bytes to 505414...
Writing at 0x0005cac4... (48 %)
```

## Test the Shelly

The Shelly webserver/access point can be found after a disconnect/connect of the 3.3v pin.
