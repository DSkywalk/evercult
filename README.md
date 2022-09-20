# evercult

![the evercult](https://user-images.githubusercontent.com/560310/191311366-bb2f7448-fb5f-4c1c-bcc2-0e604c8c28ef.png)

* https://github.com/strager/evercade-hacking
* https://github.com/RetroFailz/everfw

# patch FS to recover adb - tested on fw 2.0.2

    **********Partition Info(GPT)**********
    NO  LBA       Name
    00  00002000  uboot
    01  00002800  trust
    02  00003800  boot
    03  00007800  rootfs
    04  00028100  cfg
    05  00029100  userdata

use [rkdeveloptool](https://github.com/rockchip-linux/rkdeveloptool) for dump current FW:

    $ sudo rkdeveloptool rl 0x00007800 $((0x00028100 - 0x00007800)) 03-rootfs.img

decompress files into "rootfs" directory

    $ unsquashfs -i -f -d rootfs 03-rootfs.img

copy `adbservice` to `/usr/bin/` and `S55usbdevice` to `/etc/init.d/`. And set +x perm.

    $ chmod +x rootfs/usr/bin/adbservice
    $ chmod +x rootfs/etc/init.d/S55usbdevice

pack and upload new FS:

    $ sudo mksquashfs rootfs rootfs-patch.img
    $ sudo rkdeveloptool wlx rootfs rootfs-patch.img
