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

You should be prompted to log in. Use the default credentials to do so.

```
raspberrypi login: pi
Password: raspberry
```

##### Basic configuration using raspi-config

Neat, it's even got some colour to it! Now, we're going to run through Raspbian's built-in configuration tool `raspi-config` to start things off. Navigate using the **Arrow Keys**, **Enter** to select and go forward, **Esc** to go back, and **Ctrl-C** to cancel the program.

```bash
$ sudo raspi-config
```
>>>>>> This might be your first run-in with the phrase _"Script must be run as root. Try 'sudo raspi-config'"_. You can read all about running around as root elsewhere, but I'd recommend **not** tinkering with this; instead, get used to putting `sudo` in front of any commands requiring elevated privileges.

1. **Update** > Let the tool update itself, since some options may not be present until you do so
* _At any point when changing these settings, a reboot may be required. I can't remember which options cause `raspi-config` to prompt you for a reboot, but if you ever have to do it yourself, you can use `sudo reboot` or `sudo shutdown -r now`_
2. **Localisation Options** - it's imperative to start here if you don't live in the UK. By default, Raspbian's keyboard is set to a UK layout, which will likely totally mess up your password and leave you scratching your head when you can't log in. 
* **Change Keyboard Layout** > **Generic 105-key (Intl) PC** > **Other** > **English (US)** > Choose all the defaults. Alternately, choose whatever keyboard layout you're using.
* Test your **Shift-12345** keys at the command line to make sure the correct keyboard layout is applied.
* **Change Timezone** > Navigate to your location to select a timezone
* If you run the command `date` and it's wildly off, you may have to use `sudo service ntp restart` to get it back in sync
* **Change Locale** > Select a locale from the list, such as **en_CA.UTF-8 UTF-8**
3. **Advanced Options** > **Memory Split** > Type in **16** - Since we don't even have a GUI installed, we can use all that RAM for other cool stuff.
4. **Hostname** > Type in whatever you'd like. Be witty, be boring, whatever makes you sleep well at night. 
5. **Interfacing Options** > **SSH** > **Yes** - We really want to be connecting to this via SSH over your local network, rather than connecting all of these knarly wires. 
6. **Boot Options** > **Desktop / CLI** > **Console** - Let's make sure we're going to CLI on boot, even though that's how we connected in the first place.
7. **Change User Password** > Ideally you should use a randomized and unique password, but do the best you can.
>>>>>> **Password managers** are awesome, arguably more important than anti-virus software, free in a variety of flavours, and make your life much easier. Need to make a new account for something? No problem, whip up a 32+ character password using all character sets and you don't even have to remember it. Beats a collection of half-forgotten, hand-written passwords with reused, easily brute-forcable combinations.

8. Exit raspi-config and reboot gracefully to make sure this configuration is functional

##### Configure your networking

>>>>> I'm assuming that your home network is in the local range 192.168.1.0/24. If you don't know, log into your router's web interface (usually [http://192.168.1.1](http://192.168.1.1) or written on your router) and figure it out. Replace any IPs I use with what's appropriate for your network.

Let's get your raspi set up on your network. To start, visit your router's web interface and determine what your DHCP range is. This is the set of addresses your router will automatically hand out to devices that connect to it. We'll be setting your raspi's IP to a static value **outside** this DHCP range, so that any firewall rules we make later on can stay the same.

Now, let's make the appropriate configuration changes:

1. Run `ifconfig` to determine the name of your raspi's network adapter. There will be a Loopback address titled **lo**; ignore this. The other one, for me, is **eth0**. If you're using wireless, it's likely different. Make a note of this adapter name.

>>>>> For a text editor I find it easiest to use the pi's default text editor `nano`. If you want to get into a circlejerk about how great `vim` is, why are you reading this guide?

2. Run `sudo nano /etc/dhcpcd.conf` and navigate to the bottom with the arrow keys and using your keyboard just like a normal text editor. Write in the following configuration, substituting details where appropriate for your own network (I'm using Google's DNS servers for simplicity):

```
# Static IP details (today's date)
interface eth0
static ip_address=192.168.1.101
static routers=192.168.1.1
static domain_name_servers=8.8.8.8 8.8.4.4
```

When done, press **Ctrl-X**, **y**, and **Enter** to save your changes and exit.

3. Reboot your raspi with `sudo reboot`
4. Once your raspi is online again, open a command prompt or terminal on a different computer and try to ping its new static IP (usually `ping 192.168.1.101`). If it works, log back into the raspi and try to ping out to the internet using IP (`ping 8.8.8.8`) and name (`ping google.ca`). If all three tests work, great! If not, check your work for errors. Otherwise, there may be a firewall blocking traffic in some way. Troubleshoot this before continuing.

##### Sitting around headless

Physically connecting to the raspi using a monitor and keyboard is awkward. It's much easier to plug it into your network and power, then leave it tucked away. That's what **headless** is all about - connecting to your raspi's CLI using another computer on the same network, over the network using SSH.

>>>If you plan on being able to connect directly to your raspi CLI over the internet for some reason (not recommended), or are on a shared local network with nefarious individuals, I'd recommend changing away from the default SSH port. To do so, run `sudo nano /etc/ssh/sshd_config` and look for the first uncommented line that says **Port 22**. Change 22 to something of your choosing, save & exit, then restart the SSH service using `sudo service ssh restart`.

Install a program on your usual computer capable of SSH connections. Traditionally this has been [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), but many others exist such as [SuperPutty](https://github.com/jimradford/superputty) or [mRemoteNG](https://mremoteng.org/download). Use this program to connect to `pi@192.168.1.101`, leaving the port to 22 unless you decided to change it. You should be prompted for the password you set earlier on using `raspi-config`, and gain access.

If you could connect, congrats! You can now disconnect the monitor, keyboard and possibly mouse from your raspi and connect solely using your usual computer. If you couldn't connect, start troubleshooting. You might have the IP or port wrong (check `ifconfig` and `sudo less /etc/dhcpcd.conf`).

## Review your accomplishments

* Successfully booted your raspi using Rasbian Lite on an SD card
* Run through some basic configuration
* Made sure you were connected to the local network using a static IP
* Enabled SSH connectivity from a different computer
