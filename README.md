# Pico-8: Desktop Console

![pico-8-paper-prototype](https://github.com/anthonycaccese/pico8-desktop-console/assets/1454947/4dd63356-214a-4558-97fe-020f0ad35769)

The code in this repo heavily based on and inspired by the work of [Astorek86](https://github.com/Astorek86) who created a way to turn the GPi case + RaspberryPi Zero into a dedciated Pico-8 handheld. You can reference their original work here: https://github.com/Astorek86/Pico8-Script-for-GPi-Case.  All credit to them for their original work and a huge thank you as well ❤️

In this repo are the changes I've made to that script while working on my own dedicated Pico-8 Console.  My changes primarily remove all the aspects that were specific to the GPi case and adds elements that are specific to the version I am building (things like configuration for the square screen I will be using). 

## Hardware

Essentially I'll be building an all in one desktop console (screen and speakers included) similar in style/function to the classic Coleco tabletop arcade systems, Mac Classic or Vetrex. Something you can plug in, turn on and start playing without any fuss.

This is very much a work in progress 😊 so here is what I have so far...
- `RaspberryPi Zero W:` you can use any RaspberryPi with this build. I just recommend that it has some way to access the internet as we'll be booting directly into Pico-8 Splore.
- `Screen:` Goal is to use a 1:1 aspect ratio screen to match Pico-8's native square aspect ratio.  I am using [this screen](https://a.aliexpress.com/_mtLhZdm) from AliExpress which is 4", 720x720 resolution and has a dedicated driver board with HDMI and power ports (making it super easy to set up)  This screen does require custom settings in /config.txt which I'll outline in the software section below.

I am working on the following...
- `Audio:` I have a few different approaches here that I want to experiment with.  As soon as I have hardware that I like; i'll add it to the list above.
- `Case:` I created a quick "paper prototype" (cardboard really) to get a sense for the 3d model I'll create.  I'll start working on that model over the next few weeks.  When I have something I think will work i'll make sure to post the STLs as part of this repo.  For now you can check out my initial prototype in this video: https://youtu.be/94ngQ8RXFYQ?feature=shared
- `Controls:` The case will include at least 2 usb ports to be able to plug in controllers (up to 2 players) or mouse and keyboard.
- `Power:` To keep things simple i'll just be focusing on creating a unit that can be plugged into the wall so... no battery (but I could explore that if the first version works out well)

## Software

The primary aim for the software is to boot directly into Pico-8 Splore with as minimal a footprint as possible (while still enabling things like wifi).  I also want to be able to shutdown the console directly from Pico-8 Splore and be able to control all aspects of the device with a usb controller. 

### Steps

1. Download the lateset Raspbian Buster "Lite" from [RaspberryPi.org](https://downloads.raspberrypi.org/raspbian_lite_latest).  We're using the lite version specifically because we'll be booting into Splore directoy and won't need a desktop environment.
2. Unzip and write that image to an SD card using your preferred image writing software (Win32DiskImager, Etcher, etc...) 
