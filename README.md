# ebc7702-dev-board-ubuntu
Ubuntu Releases for EBC7702 Series Board.

## Description

Ubuntu Image releases for EBC7702 Series Board.
- Based on Ubuntu 24.04.2 LTS.
- Prebuilt Ubuntu image in compressed format named `d560-ubuntu-24.04-preinstalled-server-riscv64.img.zst`.
- Please ensure that the validated combination of the bootloader image and the Ubuntu image are flashed to the board. The release notes provide the version and validation details.
- The latest images release is available [here](https://github.com/eswincomputing/ebc7702-dev-board-ubuntu/releases/tag/2025.07.30).

## Hardware preparation
- One Type-C USB cable (for serial port monitor and uboot command)
- One USB flash driver that at least 16G (image source)

## Flashing images
Copy the bootloader and uncompressed Ubuntu image to an **ext4** formatted the USB flash driver

For example
```
USB    /- bootloader_EBC7702-D01_die0.bin
        |- bootloader_EBC7702-D01_die1.bin
        |- d560-ubuntu-24.04-preinstalled-server-riscv64.img
```
connect the USB flash driver to the **bottom** usb port of the  board and power up and wait for the uboot shell through the serial port.

### Bootloader flashing

If your device already has a compatible bootloader installed, you can skip this step.
```
=> usb reset
=> ls usb 0
=> ext4load usb 0 0x100000000 bootloader_EBC7702-D01_die0.bin
=> es_burn write 0x100000000 flash 0
=> ext4load usb 0 0x100000000 bootloader_EBC7702-D01_die1.bin
=> es_burn write 0x100000000 flash 1
```

Demo output
```

=> usb reset
resetting USB...
Bus usb1@50490000: Register 2000140 NbrPorts 2
Starting the controller
USB XHCI 1.10
scanning bus usb1@50490000 for devices... 4 USB Device(s) found
       scanning usb for storage devices... 1 Storage Device(s) found
=>
=> ls usb 0
<DIR>       4096 .
<DIR>       4096 ..
      7778653696 d560-ubuntu-24.04-preinstalled-server-riscv64.img
         5567208 bootloader_EBC7702-D01_die0.bin
         1447436 bootloader_EBC7702-D01_die1.bin
=> ext4load usb 0 0x100000000 bootloader_EBC7702-D01_die0.bin
4496952 bytes read in 27 ms (158.8 MiB/s)
=>  es_burn write 0x100000000 flash 0
BOOT_SPI_BASE 51800000 disable flash wp
 85 65 18 85 65 18
FIRMWARE writing...
Erase progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 90c bytes @ 50000 Erased: OK
Write progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 0x90c bytes @ 50000 Written: OK
DDR writing...
Erase progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: acf40 bytes @ 51000 Erased: OK
Write progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 0xacf40 bytes @ 51000 Written: OK
D2D writing...
Erase progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: b158c bytes @ fe000 Erased: OK
Write progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 0xb158c bytes @ fe000 Written: OK
BOOTLOADER writing...
Erase progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 3eb508 bytes @ 1b0000 Erased: OK
Write progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 0x3eb508 bytes @ 1b0000 Written: OK
Erase progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 1000 bytes @ 0 Erased: OK
Write progress: 100%:++++++++++++++++++++++++++++++++++++++++++++++++++
SF: 0x1000 bytes @ 0 Written: OK
bootloader write OK with logo
=>
```

### Ubuntu image burning
```
=> es_fs write usb 0 d560-ubuntu-24.04-preinstalled-server-riscv64.img mmc 0
=> reset
```
Demo output
```
=> es_fs write usb 0 d560-ubuntu-24.04-preinstalled-server-riscv64.img mmc 0
Write progress:  87%:+++++++++++++++++++++++++++++++++++++++++++
```

### Essdk deb packeges install

If you find that installing essdk deb packages using `apt install` is too slow, you can download the essdk and ffmpeg deb packages from essdk_ffmpeg_0730.zip [here](https://github.com/eswincomputing/ebc7702-dev-board-ubuntu/releases/tag/2025.07.30) and install them by `dpkg -i XXXX.deb`.

## Download from network disk

If you are unable to download images from GitHub and you are in China, you can try downloading them [here](https://pan.baidu.com/s/1gUJC8K_bSrhRO6P63BYV_w?pwd=p41j).

## Login to the board Using Serial Console

Default username and password for ubuntu image is `ubuntu`.
On first boot, user will be prompted to change the default password.
