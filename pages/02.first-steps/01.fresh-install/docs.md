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

>>> If you encounter any issues here, the formatting and imaging of your SD card might have been borked at some point; try again, following these instructions. If it still doesn't work, Google is your friend.

##### Basic configuration

You should be prompted to log in. Use the default credentials to do so.

```
raspberrypi login: pi
Password: raspberry
```
Neat, it's even got some colour to it! Now, we're going to run through Raspbian's built-in configuration tool `raspi-config` to start things off. Navigate using the **Arrow Keys**, **Enter** to select and go forward, **Esc** to go back, and **Ctrl-C** to cancel the program.

```bash
$ sudo raspi-config
```
>>>>>> This might be your first run-in with the phrase _"Script must be run as root. Try 'sudo raspi-config'"_. You can read all about running around as root elsewhere, but I'd recommend **not** tinkering with this; instead, get used to putting `sudo` in front of any commands requiring elevated privileges.

1. **Localisation Options** - it's imperative to start here if you don't live in the UK. By default, Raspbian's keyboard is set to a UK layout, which will likely totally mess up your password and leave you scratching your head when you can't log in. 
* **Change Keyboard Layout** > **Generic 105-key (Intl) PC** > **Other** > **English (US)** > Choose all the defaults. Alternately, choose whatever keyboard layout you're using.
* _At any point when changing these settings, a reboot may be required. I can't remember which options cause `raspi-config` to prompt you for a reboot, but if you ever have to do it yourself, you can use `sudo reboot` or `sudo shutdown -r now`_
* Test your **Shift-12345** keys at the command line to make sure the correct keyboard layout is applied.
* **Change Timezone** > Navigate to your location to select a timezone
* If you run the command `date` and it's wildly off, you may have to use `sudo service ntp restart` to get it back in sync
* **Change Locale** > Select a locale from the list, such as **en_CA.UTF-8 UTF-8**
2. **Update** > Let the tool update itself, since some options may not be present until you do so
3. **Advanced Options** > **Memory Split** > Type in **16** - Since we don't even have a GUI installed, we can use all that RAM for other cool stuff.
4. **Hostname** > Type in whatever you'd like. Be witty, be boring, whatever makes you sleep well at night. 
5. **Interfacing Options** > **SSH** > **Yes** - We really want to be connecting to this via SSH over your local network, rather than connecting all of these knarly wires. 
6. **Boot Options** > **Desktop / CLI** > **Console** - Let's make sure we're going to CLI on boot, even though that's how we connected in the first place.
7. **Change User Password** > Ideally you should use a randomized and unique password, but do the best you can.
* >>>>>> Password managers are awesome, arguably more important than anti-virus software, free in a variety of flavours, and make your life much easier. Need to make an account? No problem, whip up a 32+ character password using all character sets and you don't even have to remember it. Beats a collection of half-forgotten, hand-written passwords with reused, easily brute-forcable combinations.