---
title: 'Fresh Install'
---

**Congrats on owning a Raspberry Pi. Let's start by getting Raspbian and installing it!**

##### Getting an image

Download Raspbian Lite from [https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/). I'd recommend using Lite, because by only including the necessary packages, it speeds up the installation process considerably. In addition, performance out of the box will be faster without a bunch of excess weighing you down.

Unzip that bad boy .img file. Now that we've got Raspbian, we'll need to get in onto a micro SD card. I'm assuming that you've got a laptop or some other device that can interact with SD cards, because you'll need to format it. Install the SD Association's Formatting Tool on whatever computer is connected to an SD card reader from [https://www.sdcard.org/downloads/formatter_4/](https://www.sdcard.org/downloads/formatter_4/).

##### If your SD card is 32GB or less:

Use the Formatting Tool to format as FAT32. Keep the default "quick format" on, and change the "FORMAT SIZE ADJUSTMENT" setting to "ON".

##### If your SD card is greater than 32GB:

In this case, your SD card is actually an SDXC card. Use the Formatting Tool as described above. At this point, the card is formatted as exFAT. Raspbian doesn't like exFAT, so the next step is to get that into FAT32 by downloading a tool called Fat32 Format from [http://www.ridgecrop.demon.co.uk/index.htm?guiformat.htm](http://www.ridgecrop.demon.co.uk/index.htm?guiformat.htm). Run it with its default settings on your SDXC card and it should now be formatted as FAT32.

>>> Note