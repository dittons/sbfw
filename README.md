## Welcome to my Squeezbox Firmware Infomation Page

You can use the [editor on GitHub](https://github.com/dittons/sbfw/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Products

A number of different products were released.  Details of the relevant hardware can be found on the [wiki](http://wiki.slimdevices.com/index.php/Hardware_comparison).

In terms of player firmware, however, there are 3 platforms of relevance.

|Platform|Products(s)|
|:-:|:-:|
|PIC16|SlimP3|
|IP2K|Squeezebox(G)|
|IP3K|Squeezebox2/3,Transporter, Receiver, Boom|

Slimdevices released their firmware source code for the PIC16-based SlimP3 and this can be found in their [subversion repository](http://svn.slimdevices.com/repos/).

However, no firmware source code has been release for the IP2K or IP3K devices as it appears to be prohibited by Ubicom as it contains, presumably, some code from their IP3K SDK.  See extract from `Licence.txt` -
> - Squeezebox firmare may not be redistributed under any 
circumstances. It is (c) Slim Devices, and additonally contains
code which is (c) Ubicom. Ubicom's code is licenced to us only
for direct distribution in binary form. You may use the firmware
only on hardware manufactured by Slim Devices.

However, the license makes no mention of prohibiting the disassembly of or reverse engineering of the firmware.


### Server Versions

The name of the server software was changed a number of times though the lifetime of the various products.

|Version|Name|
|:-:|:-:|
|1.0 - 4.2|Slimp3Server|
|5.0 - 6.5|SlimServer|
|7.0 - 7.3|SqueezeCentre|
|7.4 - 7.6|SqueezeboxServer|
|7.7 - 7.9|LogitechMediaServer|

Historic versions of the server can be downloaded from the [official site](http://downloads.slimdevices.com/).

### Firmware Versions

Updated versions of the firmware were released as part of server updates. 
The tables below shows which firmware versions were released with which server version.

Note that there was at least one custom firmware (for a SB3) produced for a third-party company. Some details can be found on this forum [thread](https://forums.slimdevices.com/showthread.php?95025-Proprietary-SB3-Firmware-quot-Turnkey-Media-Solutions-quot).


#### IP2K Platform

|Server|Squeezebox|
|:-:|:-:|
|5.1.4|8,10(*),20,21|
|5.1.5|22-27|
|5.3.0b1|28-31|
|5.3.0|32-38|
|5.4.0|40|

(*) due to a change in the firmware update process, version 10 is used as a stepping stone from older to newer versions.

#### IP3K Platform
|Server|Squeezebox2/3|Transporter|Receiver|Boom|
|:---:|:---:|:---:|:---:|:---:|
|6.0b1|4|(n/a)|(n/a)|(n/a)|
|6.0b2 - 6.0b3|6|(n/a)|(n/a)|(n/a)|

x

|Server|Squeezebox2/3|Transporter|Receiver|Boom|
|:---:|:---:|:---:|:---:|:---:|
|6.0|8|-|-|-|
|6.0.1|9|-|-|-|
|6.0.2|11|-|-|-|
|6.1.b1 - 6.1.b2|14|-|-|-|
|6.1.0 - 6.1.1|15|-|-|-|
|6.2.b2|25|-|-|-|
|6.2.0|26|-|-|-|
|6.2.1|28|-|-|-|
|6.2.2fc1|48|-|-|-|
|6.3.0 - 6.3.1|55|-|-|-|
|6.5.0.b2|62|14|-|-|
|6.5.0|64|18|-|-|
|6.5.1|72|27|-|-|
|6.5.2 - 6.5.4|81|31|-|-|
|7.0|86|36|22|-|
|7.0.1|86|37|23|-|
|7.1|101|50|36|-|
|7.2|112|62|47|32(*)|
|7.2.1|113|63|48|33|
|7.3|120|70|55|40|
|7.3.1|121|71|56|41|
|7.3.2|123|73|58|43|
|7.3.3 - 7.3.4|127|77|62|47|
|7.4.0 - 7.5.1|130|80|65|50|
|7.5.2|131|81|66|51|
|7.5.3|132|82|67|52|
|7.5.4|133|83|68|53|
|7.5.5 - 7.5.6|134|84|69|54|
|7.6 +|137|87|77|57|

(*) It appears that pre-released versions of the Boom firmware used the same hardware id as Squeezebox2s.  The first official release of firmware for the Boom, in server version 7.2, included both firwamre versions 30 and 32.  Version 30 is used as a stepping stone to correct the hardware id from older firmwares before upgrading to later versions.

## IP3K Firmware Updates

The rest of this document relates solely to the IP3K-based products.

### Structure of a FIrmware Update File/Image
* Header
* Update Stub Code
* Decompression Code
* Compressed Data Stream
* Trailer

### Update Process

At a very high level, the firmware update process seems to be as follows -
1. the server sends the update file to the player using slimproto messages.
1. the player writes the file image to flash memory at the address specified in the update file and overwrites the active firmware image's magic number.
1. the player is rebooted
1. the player's bootloader scans the flash memory looking for a magic number and finds the 'update image'
1. the boot loaded some very basic validation of the image and assuming its OK, passes control to the update stub code in the update image 
1. this code copies the decompression code to RAM and executes it from there
1. the decompression code decompresses the data stream, writing it to the address range specified in the update image
1. if this is successful, it writes the 'active image' magic number at the start of the decompressed data
1. the player is rebooted
1. the player's bootloader scans the flash memory looking for a magic number and finds the 'active image'
1. then passes control to it.

### IP3K Program Memory
The IP3K does not have on-chip flash, only external flash of *up-to* 4MB mapped as addresses `20000000 - 203FFFFF`.

As external flash is relative slow to access, it also provides 256KB of on-chip program SRAM mapped as addresses `40000000 - 4003FFFF`.

Hence, any code where it's desirable to run fast will often be copied from Flash to SRAM before being executed.

### SB Flash Usage
Squeezebox appears to have 2MB of flash and organises it as follows -
* the bootstrap code regoin is 128K and starts at `20000000`.  
* the active image starts at `20020000`.
* any update image is written at the end of flash.

e.g. the flash layout of a boom running firmware version 57 is -

#### *Example - Boom v57*
|Address Range|Use|
|:---:|:---:|
|20000000 - 2001FFFF|bootstrap/debug monitor code|
|20020000 - 2014C2FF|active firmware image|
|2014C300 - 2015FFFF|unused|
|20160000 - 201FFFFF|firmware update image|

### Update Image Structure
* Header
* Update Stub Code
* Decompression & Update Code
* Compressed Data Stream
* Trailer

### Header

Each firmware update file begins with a 128-byte header.
All multi-byte values are stored in big-endian format.
Any offsets not shown contain 0 in all firmwares seen.

    Offset    Hex     Description
    ======  ========  ===============
     0000   FA320080  magic number (looked for by bootloader)
     0004   DDDD****  Hardware ID (see below)
     000C   ********  Checksum/CRC ?
     0010   20020000  Start Address 
     0014   20200000  End Address (+1)
     0018   000*0000  Size of Update File/Image (5 < * <  A)
     001C   00000001  (unknown)
     0040   00001***  offset of compressed firmware data in update image
     0044   000*****  length of compressed firmware data in update image
     0048   20020000  memory address where firmware should be decompressed to
     004C   00******  length of decompressed firmware data

    
      HWID    Product
    ========  ====================================
    DDDD1111  Squeezebox2/3 (& Boom v30 and prior)
    DDDD2222  Transporter
    DDDD3333  Receiver
    DDDD4444  Boom

#### *Example - Boom v57*

    Offset    Hex     Description
    ======  ========  =============
     0000   FA320080  magic number
     0004   DDDD4444  Hardware ID - Boom
     000C   D44A8D6C  Checksum/CRC
     0010   20020000  Start Address 
     0014   20200000  End Address (+1)
     0018   000A0000  Size of Update File/Image
     001C   00000001  (unknown)
     0040   00001420  offset of compressed firmware data in update image
     0044   0009829A  length of compressed firmware data in update image
     0048   20020000  memory address where firmware should be decompressed to
     004C   0012C300  length of decompressed firmware data


### Update Stub

The update stub can be found in the update image at offsets `00000080 - 00000197` and will be run from it's position in flash (e.g. at `20160080`) during the update process.

This does some initial limited hardware setup (related to IP3K timer and flash programmer parameters), copies the decompression code to SRAM and then transfers control to it.

This is very constistent between products and versions.  The only differences to note are -
1. Slight differences in hardware parameters and in timing loop values used (on all Transporter firwmares due to higher 325MHZ compare to the 250MHz of other products)
1. Trivial differences in register allocations (on SB2 FW verions 28 and prior), perhaps due to an earlier compiler or SDK version being used?

### Decompression & Update Code

The decompression & update code starts at offset `00000198` in the firmware update file and continues until the offset specified in the header at offset `000001C`.  This will be copied from its position in flash (e.g. at `20160198`) to SRAM at address `40000000` before being executed.

It processes the compressed data stream, decompressing it and writing it to flash starting at the prescribed address `20020000` as it goes.

It is largely consistent between versions, with there are some variations.
Reasons for these seem to include -
* compression method differences (SB2 FW versions 28 and prior use a slightly different method to others.)
* ? compiler/data layout changes
* updates to hardware ids (e.g. boom v30)

On successful completion of decompression, its final job is to write the active firmware magic constant of `C90055AA` at address `20020000` to indicate to the bootloader that the decompressed flash code can be used on the next boot.

### Compressed Data Stream

The compressed data is in the form of a bit-stream.

As noted above, there was a small change was made to the compression used sometime between squeezebox2 firmware versions 28 and 48.   This section describes the newer (de)compression algorithm which applied to SB2 firmware 48 and above and all  other IP3K-based players.

[TODO - write this up properly!]
