---
title: 'Fresh Install'
---

>>> **Congrats on owning a Raspberry Pi. Let's start by getting Raspbian and installing it!**

## Prep work

##### Download the image file

Download Raspbian Lite from [https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/). I'd recommend using Lite, because by only including the necessary packages, it speeds up the installation process considerably. In addition, performance out of the box will be faster without a bunch of excess weighing you down.

Unzip that bad boy .img file. Now that we've got Raspbian, we'll need to get in onto a micro SD card. I'm assuming that you've got a laptop or some other device that can interact with SD cards, because you'll need to format it. Install the SD Association's Formatting Tool on whatever computer is connected to an SD card reader from [https://www.sdcard.org/downloads/formatter_4/](https://www.sdcard.org/downloads/formatter_4/).

>>>> Always be aware of drive letters when reformatting storage devices. **Only apply these tools to your SD card's drive letter.** Doing otherwise will attempt to reformat other connected devices attached to your computer, which is bad news.

##### If your SD card is 32GB or less:

Use the Formatting Tool to format as FAT32. Keep the default "quick format" on, and change the "FORMAT SIZE ADJUSTMENT" setting to "ON".

##### If your SD card is greater than 32GB:

In this case, your SD card is actually an SDXC card. Use the Formatting Tool as described above. At this point, the card is formatted as exFAT. Raspbian doesn't like exFAT, so the next step is to get that into FAT32 by downloading a tool called Fat32 Format from [http://www.ridgecrop.demon.co.uk/index.htm?guiformat.htm](http://www.ridgecrop.demon.co.uk/index.htm?guiformat.htm). Run it with its default settings on your SDXC card and it should now be formatted as FAT32.

##### Putting Raspbian on the SD card

Grab a tool called Win32DiskImager from [https://sourceforge.net/projects/win32diskimager/](https://sourceforge.net/projects/win32diskimager/). Select the Raspbian Lite .img file, your SD card, and click "Write". Once it's done you can exit safely.

>>>>> The [official instructions](https://www.raspberrypi.org/documentation/installation/installing-images/README.md) now suggest using a tool called Etcher from [https://etcher.io/](https://etcher.io/). Feel free to give that a try.

## Let's get physical

It's time to actually touch the Raspberry Pi! Connect it to a monitor, keyboard, mouse, and ethernet cable. Slide in your freshly minted Raspbian SD card. Connect the power adapter and it will boot immediately. 

>>>>> The [official installation instructions](https://www.raspberrypi.org/documentation/installation/noobs.md) say that Raspbian Lite requires an internet connection to install. I have a Pi 2, so a physical network connection is my only option. I'm unsure how installing Raspbian Lite over WiFi would work, so you're on your own for that.

Select Raspbian as the operating system and run through the installation. If you encounter any issues here, something might be wrong with your network connection, or the formatting and imaging of your SD card might have been borked at some point; try again, following these instructions, and if it still doesn't work, Google is your friend.

