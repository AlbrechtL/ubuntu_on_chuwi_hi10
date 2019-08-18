# ubuntu_on_chuwi_hi10
Collection of Ubuntu 19.04 adaptions to CHWUI Hi10 (CW1515) tablet.

This README is based on https://github.com/danielotero/linux-on-hi10.

### Introduction
This is my attempt to gather all the information in one place on how to support the [Chuwi Hi10](https://www.chuwi.com/product/items/Chuwi-Hi10.html) tablet on Linux.

### Basic hardware specifications

| Part         | Component                                |
| ------------ | ---------------------------------------- |
| Processor    | Intel® Atom™ Z8300 CPU @ 1.44GHz         |
| Graphics     | Intel® HD Graphics 400                   |
| RAM          | DDR3 4096 MB @ 1066 MHz                  |
| Screen       | 10.1-inch IPS display @ 1920x1200 pixels |
| Touchscreen  | Chipone                                  |
| Connectivity | Realtek 8723BS (802.11n & Bluetooth® 4)  |
| Accelerometer| Bosch BMC150                             |
| Camera (?)   | ???                                      |
| Ports        | 2 USB, 1 microUSB, 1 microHDMI®          |

### Current status
This table currently refers to the Ubuntu 19.04 with kernel 5.0.0-25-generic

| Device                 | Status                    |
|------------------------|---------------------------|
| Internal flash storage | Yes                       |
| Graphics               | Yes                       |
| microUSB port          | Yes                       |
| Incorporated USB hub   | Yes                       |
| Keyboard               | Yes                       |
| Touchpad               | Yes                       |
| 802.11n wireless       | Yes                       |
| Speakers               | No                        |
| Headphone plug         | ???                       |
| Battery measurement    | Yes                       |
| Backlight control      | Yes                       |
| Power button           | Yes                       |
| Volume buttons         | Yes                       |
| Suspend                | Yes                       |
| Touchscreen            | Yes (see touchscreen section)         |
| microSD card slot      | ???                       |
| Accelerometer          | Yes (see accelerometer section)         |
| HDMI output            | Yes                       |
| HDMI audio output      | ???                       |
| Bluetooth              | Yes                       |
| Active stylus pen      | Yes                       |
| Front camera           | No                        |
| Back camera            | No                        |

### Touchscreen
The touchscreen firmware file missing. You will find the following messages inside the boot log.
   ```
[    7.515819] chipone_icn8505 i2c-CHPN0001:00: Direct firmware load for chipone/icn8505-HAMP0003.fw failed with error -2
[    7.515838] chipone_icn8505 i2c-CHPN0001:00: Firmware request error -2
[    7.515955] chipone_icn8505: probe of i2c-CHPN0001:00 failed with error -2
   ```
I used the firmware [HAMP0003.bin](https://github.com/JohnMH/chipone_ts/raw/master/orig_firmware/hi10/HAMP0003.bin) from the repo https://github.com/JohnMH/chipone_ts and renamed it into `icn8505-HAMP0003.fw` copied it into `/lib/firmware/chipone/`

### Accelerometer
By default the accelerometer is rotate by 90°. Fix is by copy the file `61-sensor-local.hwdb` to `/etc/udev/hwdb.d/`.
Afterwards run the commands `systemd-hwdb update` and `udevadm trigger -v -p DEVNAME=/dev/iio:device0`.
With the command `udevadm info -export-db | grep ACCEL` you can check if the new settings are deteced.
Reboot the system.

systemd integration status: https://github.com/systemd/systemd/pull/13351

### Linux console rotate
Put `GRUB_CMDLINE_LINUX="video=efifb fbcon=rotate_all:1"` into `/etc/default/grub` and run `update-grub`
Source: https://unix.stackexchange.com/questions/68369/rotate-console-on-startup-debian

### Notes
 Take also a look to the to the following repos
 * [willyneutron](https://github.com/willyneutron/lubuntu_in_chuwi_Hi10Pro) repo, which is more detailed!
 * https://github.com/danielotero/linux-on-hi10
 * https://github.com/Dax89/chuwi-dev
 * https://github.com/JohnMH/chipone_ts


