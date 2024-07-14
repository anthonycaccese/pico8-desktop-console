# Pico-8: Desktop Console

![pico-8-paper-prototype](https://github.com/anthonycaccese/pico8-desktop-console/assets/1454947/4dd63356-214a-4558-97fe-020f0ad35769)

The code in this repo heavily based on and inspired by the work of [Astorek86](https://github.com/Astorek86) who created a way to turn the GPi case + RaspberryPi Zero into a  dedicated Pico-8 handheld. You can reference their original work here: https://github.com/Astorek86/Pico8-Script-for-GPi-Case.  All credit to them for their original work and a huge thank you as well ❤️

In this repo are the changes I've made to that script while working on my own dedicated Pico-8 Console.  My changes primarily remove all the aspects that were specific to the GPi case and adds elements that are specific to the version I am building (things like configuration for the square screen I will be using). 

## Hardware

Essentially I'll be building an all in one desktop console (screen and speakers included) similar in style/function to the classic Coleco tabletop arcade systems, Mac Classic or Vetrex. It will be dedicated to playing the awesome [Pico-8 fantasy console](https://www.lexaloffle.com/pico-8.php) and its amazing library of community developed games. Something you can turn on and start playing without any fuss.

This is very much a work in progress 😊 so here is what I have so far...
- `RaspberryPi Zero W:` you can use any RaspberryPi with this build. I just recommend that it has some way to access the internet as we'll be booting directly into Pico-8 Splore.
- `Screen:` Goal is to use a 1:1 aspect ratio screen to match Pico-8's native square aspect ratio.  I am using [this screen](https://a.aliexpress.com/_mtLhZdm) from AliExpress which is 4", 720x720 resolution and has a dedicated driver board with HDMI and power ports (making it super easy to set up)  This screen does require custom settings in /config.txt which I'll outline in the software section below.
- `Audio:` I am using this [amp](https://www.adafruit.com/product/3346) and these [speakers](https://www.adafruit.com/product/1669) its super easy to set up and fits well with the rest of the boards I am using.  This does require you to have a raspberrypi with a header built in.  I've added the configuration for these speakers into the steps below if you plan to use the same.

I am working on the following...
- `Case:` I created a quick "paper prototype" (cardboard really) to get a sense for the 3d model I'll create.  I'll start working on that model over the next few weeks.  When I have something I think will work i'll make sure to post the STLs as part of this repo.  For now you can check out my initial prototype in this video: https://youtu.be/94ngQ8RXFYQ?feature=shared
- `Controls:` The case will include at least 2 usb ports to be able to plug in controllers (up to 2 players) or mouse and keyboard.
- `Power:` To keep things simple i'll just be focusing on creating a unit that can be plugged into the wall so... no battery (but I could explore that if the first version works out well)

## Software

The primary aim for the software is to boot directly into Pico-8 Splore with as minimal a footprint as possible (while still enabling things like wifi).  I also want to be able to shutdown the console directly from Pico-8 Splore and be able to control all aspects of the device with a usb controller. 

### Steps

1. Download the lateset Raspbian Buster "Lite" from [RaspberryPi.org](https://downloads.raspberrypi.org/raspbian_lite_latest).  We're using the lite version specifically because we'll be booting into Splore directly and won't need a desktop environment.
2. Unzip and write the Raspbian Buster "Lite" image to an SD card using your preferred image writing software (Win32DiskImager, Etcher, etc...)
4. After that's complete; plug the SD card back into your computer (you will need to make some changes on the `boot` partition before booting for the first time)
5. Download PICO-8 for RaspberryPi from https://www.lexaloffle.com/games.php?page=updates
6. Unzip the files from that archive to the `boot` partition of your SD card into a folder called `pico-8`.  Your folder structure should look like this afterwards:
```
boot/
  ├─ pico-8/
  │  ├─ license.txt
  │  ├─ pico-8_manual.txt
  │  ├─ pico8
  │  ├─ pico8_64
  │  ├─ pico8_dyn
  │  ├─ pico8_gpio
  │  ├─ pico8.dat
  │  └─ readme_raspi.txt
  └─ (other files from the boot directory such as config.txt and cmdline.txt)
```
6. *Optional but recommended*: Create a Folder named "carts" in the pico-8 directory you just made above and add some of your favourite cartridges in there. This will give you a direct list of games you can play in Splore when you don't have an internet connection.  Your folder structure should look like this afterwards:
```
boot/
  ├─ pico-8/
  │  ├─ carts/
  │  │  └─ (add your downloaded carts in .png or .p8 format here)
  │  ├─ license.txt
  │  ├─ pico-8_manual.txt
  │  ├─ pico8
  │  ├─ pico8_64
  │  ├─ pico8_dyn
  │  ├─ pico8_gpio
  │  ├─ pico8.dat
  │  └─ readme_raspi.txt
  └─ (other files from the boot directory such as config.txt and cmdline.txt)
```
7. Edit `cmdline.txt` and replace this specific text `init=/usr/lib/raspi-config/init_resize.sh` with the following:
```
init=/bin/bash -c "mount -t proc proc /proc; mount -t sysfs sys /sys; mount /boot; source /boot/makepico8"
```
> This change sets up the first boot to call a script called makepico8 which will set up everything up.  You'll add that script in the next step.
8. Download the `makepico8` script from this repo [here](https://raw.githubusercontent.com/anthonycaccese/pico8-desktop-console/main/makepico8) and copy it to your `boot` partition.  Your folder structure should look like this afterwards:
> Note: There is a section below that outlines what the makepico8 script does to help answer any questions.
```
boot/
  ├─ pico-8/
  │  ├─ carts/
  │  │  └─ (add your downloaded carts in .png or .p8 format here)
  │  ├─ license.txt
  │  ├─ pico-8_manual.txt
  │  ├─ pico8
  │  ├─ pico8_64
  │  ├─ pico8_dyn
  │  ├─ pico8_gpio
  │  ├─ pico8.dat
  │  └─ readme_raspi.txt
  ├─ makepico8
  └─ (other files from the boot directory such as config.txt and cmdline.txt)
```
9. Edit `config.txt` and replace its contents with the following.
> [!IMPORTANT]
> You should only do this step if you are using the same 720x720 screen that I listed in the hardware section.  If you going to plug this into a regular monitor or tv you won't need to make this change to config.txt and you can skip this step.
```
[all]
# 720x720 display config 
hdmi_force_hotplug=1
config_hdmi_boost=10
hdmi_group=2
hdmi_mode=87
hdmi_timings=720 0 100 20 100 720 0 20 8 20 0 0 0 60 0 48000000 6
start_x=0
```
10. *Optional but recommended*: If you want wifi to be set up out of the box; Create a file called `wpa_supplicant.conf` with the following content and replace all values between `< >` with your own.  Add this file to the `boot` partition.
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=<Your Country-Code>

network={
  ssid="<Your SSID>"
  psk="<Your Password>"
  key_mgmt=WPA-PSK
}
```
11. *Optional but recommended*: If you want to be able to SSH into your device; create a filed called `ssh` (no extention) and add it to the `boot` partition.

### What does the `makepico8` script do?

- It creates a new user called "pico8" and sets the home directory to `/boot/pico-8`
- It changes the permission on the "boot" partition in /etc/fstab, because in Linux, only root is allowed to write on VFAT-Partitions. This script changes that and gives permission to the "pico8" user and the "pico8" group.
- It modifies systemd-files to allow the "pico8" user to autologin (so a keyboard is not needed).
- It modifies /etc/sudoers to enable to the "pico8" user to trigger a device shutdown (so you can shutdown the device directly from Splore).
- It sets values to reduce/remove console output during boot.

This script only runs on first boot to essentially "make" a dedicated Pico-8 device (hence its name 😀) and on subsequent boots its not called again.  You can see that by checking how cmdline.txt is updated after your first boot.

### First Boot

- After you have completed the above steps you should be able to plug your SD card into your RaspeberryPi and turn it on.
- On first boot the makepico8 script will conduct all the setup described above and when finished your raspberrypi should reboot directly into Pico-8 Splore.
- At this point, if everything worked correctly, you will be able to navigate splore, download carts from the BBS (if you set up networking) and see your manually added carts in the file menu.  You should also be able to shutdown the device directly from splore.

### Audio
> [!IMPORTANT]
> You should only follow these steps if you are using the same amp that I listed in the hardware section above.  These steps will only work with that specific amp. The source for the steps are located here https://learn.adafruit.com/adafruit-speaker-bonnet-for-raspberry-pi/raspberry-pi-usage
1. Add the following to config.txt
```
# adafruit speaker bonnet
dtoverlay=hifiberry-dac
dtoverlay=i2s-mmap
```
2. add an `asound.conf` file to /etc with the followig contents
```
pcm.speakerbonnet {
   type hw card 0 
}
pcm.!default { 
   type plug 
   slave.pcm "dmixer" 
}
pcm.dmixer { 
   type dmix 
   ipc_key 1024
   ipc_perm 0666
   slave { 
     pcm "speakerbonnet" 
     period_time 0
     period_size 1024
     buffer_size 8192
     rate 44100
     channels 2 
   } 
}
ctl.dmixer { 
  type hw card 0 
}
```

## Additional Notes

### Videos (In Progress)
- Screen & Software Test: https://youtu.be/JKJjj8BBlpU
- Paper Prototype: https://youtu.be/94ngQ8RXFYQ

### Boot Time
- First boot is the longest as it runs through the script to set things up. Subsequent boots take between 30 to 40 seconds from my testing so far.  There may be ways to shorten this further but its not so long that I am going to focus on it just yet (there are lots of more fun things to do first).

### Getting rid of the password warning
- Login over SSH with username `pi` / password `raspberry`
- type `passwd` and follow the prompts

## Credits

Thank you to [Astorek86](https://github.com/Astorek86) for their original work here: https://github.com/Astorek86/Pico8-Script-for-GPi-Case
Thank you to [NerdyTeachers](https://nerdyteachers.com/PICO-8/) for their tips on how to reduce boot times.
