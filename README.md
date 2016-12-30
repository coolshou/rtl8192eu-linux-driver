# rtl8192eu linux drivers

The official drivers for D-Link DWA-131 Rev E, with patches to keep it working on newer kernels.
Also works on Rosewill RNX-N180UBE v2 N300 Wireless Adapter.

**NOTE:** This is just a "mirror". I have no knowledge about this code or how it works. Expect no support from me or any contributors here. I just think GitHub is a nicer way of keeping track of this than random forum posts and precompiled binaries being sent by email. I don't want someone else to have to spend 5 days of googling and compiling with random patches until it works.

## Source for the official drivers

Official drivers were downloaded from D-Link Australia. D-Link USA and the european countries I checked only lists revision A and B. Australia lists all three.

* [Download page for DWA-131][driver-downloads]
* [Direct download link for Linux drivers][direct-download]
  * GitHub will not link to the `ftp://` schema. Raw link contents:

      `ftp://files.dlink.com.au/products/DWA-131/REV_E/Drivers/DWA-131_Linux_driver_v4.3.1.1.zip`

In addition, you can find the contents of this version in the initial commit of this repo: [1387cf623d54bc2caec533e72ee18ef3b6a1db29][initial-commit]

## Patches

You can see the applied patches, their sources and/or motivation by looking at the commits. The `master` branch will mostly be kept clean with a single commit per patch, except for Pull Requests. You can review commit by commit and then record the SHA in order to get a safe reference to use. As long as the SHA stays the same you know that what you get has been reviewed by you.

Note that updates to this README will show up as separate commits. I will not mix changes to this file with changes to the code in case you want to mirror this without the README.

## Building and installing using DKMS

This tree supports Dynamic Kernel Module Support (DKMS), a system for
generating kernel modules from out-of-tree kernel sources. It can be used to
install/uninstall kernel modules, and the module will be automatically rebuilt
from source when the kernel is upgraded (for example using your package manager).

1. Install DKMS and other required tools

    ```shell
    $ apt-get install git linux-headers-generic build-essential dkms
    ```

2. Add the driver to DKMS. This will copy the source to a system directory so
that it can used to rebuild the module on kernel upgrades.

    ```shell
    $ dkms add .
    ```

3. Build and install the driver.

    ```shell
    $ dkms install rtl8192eu/1.0
    ```

If you wish to uninstall the driver at a later point, use
_dkms uninstall rtl8192eu/1.0_. To completely remove the driver from DKMS use
_dkms remove rtl8192eu/1.0_.

## Submitting patches

1. Fork repo
2. Do your patch in a topic branch
3. Open a pull request on GH, or send it by email to `Magnus Bergmark <magnus.bergmark@gmail.com>`.
4. I'll squash your commits when everything checks out and add it to `master`.

## Copyright and licenses

The original code is copyrighted, but I don't know by whom. The driver download does not contain license information; please open an issue if you are the copyright holder.

Most C files are licensed under GNU General Public License (GPL), version 2.

[driver-downloads]: http://support.dlink.com.au/Download/download.aspx?product=DWA-131
[direct-download]: ftp://files.dlink.com.au/products/DWA-131/REV_E/Drivers/DWA-131_Linux_driver_v4.3.1.1.zip
[initial-commit]: https://github.com/Mange/rtl8192eu-linux-driver/commit/1387cf623d54bc2caec533e72ee18ef3b6a1db29

## EFuse setting (from [anykaguo][anykaguo-info])
  ```shell
  $ iwpriv wlan0 efuse_get realmap    //dump efuse table and check b8 value
  ```
```
wlan0     efuse_get:
0x00	29 81 00 7C 01 40 03 00 	40 74 04 50 14 00 00 00 
0x10	21 21 21 21 21 21 25 25 	25 25 25 EE 0F EF FF FF 
0x20	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x30	FF FF FF FF FF FF FF FF 	FF FF 1E 1E 1E 1E 1E 1E 
0x40	24 24 24 24 24 EE 0F EF 	FF FF FF FF FF FF FF FF 
0x50	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x60	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x70	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x80	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x90	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0xa0	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0xb0	FF FF FF FF FF FF FF FF 	21 36 21 00 00 00 FF FF 
0xc0	FF 01 00 10 00 00 00 FF 	00 00 FF FF FF FF FF FF 
0xd0	01 20 19 33 66 47 02 70 	62 B8 27 B9 94 09 03 52 
0xe0	65 61 6C 74 65 6B 1D 03 	57 69 72 65 6C 65 73 73 
0xf0	20 4E 20 4E 61 6E 6F 20 	55 53 42 20 41 64 61 70 
0x100	74 65 72 00 FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x110	FF FF FF FF FF FF FF 0D 	03 00 05 00 30 00 00 00 
0x120	00 93 FF FF FF FF FF 01 	D2 26 0F 74 93 FC 63 0B 
0x130	F6 A8 98 2D 03 92 98 00 	FC 8C 00 11 9B 44 02 0A 
0x140	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x150	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x160	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x170	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x180	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x190	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x1a0	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x1b0	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x1c0	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x1d0	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x1e0	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF 
0x1f0	FF FF FF FF FF FF FF FF 	FF FF FF FF FF FF FF FF
```
   ```shell
   $ iwpriv wlan0 efuse_set wmap,b8,20 //modify：efuse offset b8=0x20
   $ iwpriv wlan0 efuse_get realmap    //check to see b8 value had change
   ```
[anykaguo-info]: http://blog.chinaunix.net/uid-30113248-id-4774480.html
