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

However, no firmware source code has been release for the IP2K or IP3K devices.



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
|:-:|:-:|:-:|:-:|:-:|
|6.0b1|4|-|-|-|
|6.0b2 - 6.0b3|6|-|-|-|
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

(*) It appears that pre-released versions of the Boom firmware used the same hardware id as Squeezebox2s.  The first official release of firmware for the Boom in server version 7.2, included both firwamre versions 30 and 32.  Version 30 being used as a stepping stone to correct the hardware id from older firmwares for upgrading to later versions.

## IP3K Firmware Update File Format
### Structure
* Header
* Update Stub
* Compressed Image

### Header

... to fill in

### Update Stub

The update stub is largely constistent between products and versions.  However, there appaers to be 6(tbc) variations.
Reasons for these seem to include -
* hardware differences (e.g. transporter)
* compression method differences
* ? compiler/data layout changes
* updates to hardware ids (e.g. boom v30)

The variations are -
*
*
*
*
*
*

### Compression

A small change was made to the compression used sometime between squeezebox2 firmware versions 28 and 48.
This section describes thew newer (de)compression algorithm which applied to SB2 firmware 48 and above and add other IP3K-based players.

...
