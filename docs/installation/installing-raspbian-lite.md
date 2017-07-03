# Installing Raspbian Lite

## Download latest release

    $ wget --content-disposition https://downloads.raspberrypi.org/raspbian_lite_latest
    $ unzip 2017-06-21-raspbian-jessie-lite.zip


## Write system the SD Card

Note that minimum SD card capacity should be 8 GB and has to be on the
[compatibility list for Raspberry Pi](http://elinux.org/RPi_SD_cards).  The SD
card has to be freshly formatted prior dd command.

    $ sudo dd bs=4M if=2017-06-21-raspbian-jessie.img of=/dev/mmcblk0 status=progress


## Boot Raspberry Pi

1. Insert micro-SD card into the slot.
2. Attach keyboard to one of the USB slots.
3. Attach LCD to HDMI slot.
4. Attach 2.5A power adapter to the micro-USB slot.
5. Attach power adapter to the power socket.

Do not power Raspberry Pi via USB from laptop.  It really needs 2.5A to work
properly.  Though the original Raspberry Pi power adapter is recommended, any
2.5A adapter should work.

---

[Home](../README.md) > [Installation](README.md) > *Installing Raspbian Lite*
