# Flash Shelly devices

Firmware for the devices can be found here: <https://www.shelly-support.eu/filebase/index.php?filebase/6-shelly-firmware/>

1. Flash Shelly 1 binairy on the device (even if this is not the correct firmware for the device.
  - Backup: `esptool.py --port /dev/tty.usbserial-110 read_flash 0x00000 2097152 ./shelly-dump.bin`
  - Powercycle the device
  - Erase: `esptool.py --port /dev/tty.usbserial-1110 erase_flash`
  - Powercycle the device
  - Write: `esptool.py --port /dev/tty.usbserial-1110 write_flash 0x00000 ./tasmota.bin`
3. Find the correct device model using <https://kb.shelly.cloud/knowledge-base/shelly-1pm>.
4. Choose an over the air update with the Shelly firmware using <http://archive.shelly-tools.de>.

_Memo: For other shelly devices, like the Shelly 1PM, look up the device model at <https://kb.shelly.cloud/knowledge-base/shelly-1pm>. The Shelly 1PM is model `SHSW-PM`. Using the device model you can find the firmware URL at <http://archive.shelly-tools.de>._

All firmware versions for shelly: <http://api.shelly.cloud/files/firmware>

| thing-type        | Model                                                  | Vendor ID |
| ----------------- | ------------------------------------------------------ | --------- |
| shelly1           | Shelly 1 Single Relay Switch                           | SHSW-1    |
| shelly1l          | Shelly 1L Single Relay Switch                          | SHSW-L    |
| shelly1pm         | Shelly Single Relay Switch with integrated Power Meter | SHSW-PM   |
| shelly2-relay     | Shelly Double Relay Switch in relay mode               | SHSW-21   |
| shelly2-roller    | Shelly2 in Roller Mode                                 | SHSW-21   |
| shelly25-relay    | Shelly 2.5 in Relay Switch                             | SHSW-25   |
| shelly25-roller   | Shelly 2.5 in Roller Mode                              | SHSW-25   |
| shelly4pro        | Shelly 4x Relay Switch                                 | SHSW-44   |
| shellydimmer      | Shelly Dimmer                                          | SHDM-1    |
| shellydimmer2     | Shelly Dimmer2                                         | SHDM-2    |
| shellyix3         | Shelly ix3                                             | SHIX3-1   |
| shellyuni         | Shelly UNI                                             | SHUNI-1   |
| shellyplug        | Shelly Plug                                            | SHPLG2-1  |
| shellyplugs       | Shelly Plug-S                                          | SHPLG-S   |
| shellyem          | Shelly EM with integrated Power Meters                 | SHEM      |
| shellyem3         | Shelly 3EM with 3 integrated Power Meter               | SHEM-3    |
| shellyrgbw2-color | Shelly RGBW2 Controller in Color Mode                  | SHRGBW2   |
| shellyrgbw2-white | Shelly RGBW2 Controller in White Mode                  | SHRGBW2   |
| shellybulb-color  | Shelly Bulb in Color Mode                              | SHBLB-1   |
| shellybulb-white  | Shelly Bulb in White Mode                              | SHBLB-1   |
| shellybulbduo     | Shelly Duo White                                       | SHBDUO-1  |
| shellybulbduo     | Shelly Duo White G10                                   | SHBDUO-1  |
| shellycolorbulb   | Shelly Duo Color G10                                   | SHCB-1    |
| shellyvintage     | Shelly Vintage (White Mode)                            | SHVIN-1   |
| shellyht          | Shelly Sensor (temperature+humidity)                   | SHHT-1    |
| shellyflood       | Shelly Flood Sensor                                    | SHWT-1    |
| shellysmoke       | Shelly Smoke Sensor                                    | SHSM-1    |
| shellymotion      | Shelly Motion Sensor                                   | SHMOS-01  |
| shellymotion2     | Shelly Motion Sensor 2                                 | SHMOS-02  |
| shellygas         | Shelly Gas Sensor                                      | SHGS-1    |
| shellydw          | Shelly Door/Window                                     | SHDW-1    |
| shellydw2         | Shelly Door/Window 2                                   | SHDW-2    |
| shellybutton1     | Shelly Button 1                                        | SHBTN-1   |
| shellybutton2     | Shelly Button 2                                        | SHBTN-2   |
| shellysense       | Shelly Motion and IR Controller                        | SHSEN-1   |
| shellytrv         | Shelly TRV                                             | SHTRV-01  |

| device | model| url |
| --- | --- | --- |
| Shelly 1 | SHSW-1 | <http://archive.shelly-tools.de/version/v1.9.4/SHSW-1.zip> |
| Shelly 1PM | SHSW-PM | <http://archive.shelly-tools.de/version/v1.9.4/SHSW-PM.zip> |

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
