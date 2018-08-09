# microbadge
functional badge in 1 square centimeter

The [Shitty Add-On(SAO) spec](https://hackaday.io/project/52950-defcon-26-shitty-add-ons) made it even easier to build badges, boards, and all sorts of assorted flashy swag without the commitment to building a full badge.

To comlpiment the assortment of SAO boards I was working on, I decided to create the smalles possible functional 'badge' with a single SAO connector.

[Hardware design](#hardware-design) will tell you more about how the badge was designed and built. [Other Components](#other-components) will list the other boards available that can be combined to make up a full 'badge'. [Building your Badge](#building-your-badge) covers how to put pieces together and still have a good chance of them working properly. Finally,  [Programming your Badge](#programming-your-badge) covers how to set up the IDE and rewrite the code on your microbadge via USB.

# Stupid SAO tricks
To push the limits of the SAO, I have two example sketches that output mostly-valid IIC data, but manage to push out completely different protocols at the same time.

## IIC Protocol Polyglots
IIC is a two-wire, multi-master protocol. Devices 'claim' the bus with a start condition, and then transmit an address of a slave which is expected to respond. 
However:
1. The start condition prohibits others from speaking
2. everyone is supposed to ignore the data line while clock is low
This means we can do whatever the heck we want with the data line at that point in time. My two POCs are driving Neopixel displays and transmitting UART, potentially for an opticspy to pick it up over an LED.

[NeoPixel Over IIC](https://github.com/securelyfitz/Adafruit_NeoPixel_Over_IIC)

[SAO OpticSpeaker]

# Hardware Design
The microbadge is based on the [Digispark](http://digistump.com/), a small attiny85 badged arduino-compatible development board that uses the [micronucleus usb bootloader](https://github.com/micronucleus/micronucleus).

## Trimming down the design
The fun objective was to see how small the board really could be, while still being programmable with no special hardware:
1. The Digispark had a USB-A connector made from a thick PCB. This took up space and required especially thick boards. I replaced it with a much smaller microUSB header
1. The Digispark has a large 3.3v regulator to power the attiny85 as well as some add-on devices. It also needed zener diodes to make sure the USB data lines did not exceed 3.3v. I removed both. The board no longer gets power from USB and must have an exernal 3.3v power supply
## adding to the design
1. The Digispark could get power from USB, or from 3.3 or 5v pins. Without the regulator and no room for additional pins, I added pads on the end to edge-mount a vertical cr2032 holder. It's rather difficult to attach and potentially weak unless reinforced, so several battery-bearing add-ons seem to be a better choice.
1. The digispark had a 6-pin header. the SAO header only needs power, ground, and 2 data pins for I2C.

The SAO connector took the most space and is placed on the same side as the necessary resistors and capacitors, while the attiny and usb micro connctor could fit on the other side.

# Other components
Building the microbadge was inspired by all the add-ons that were available. I started with a series of arms, then added a few faces, and finally a few addons and adapters:
## [Arms](https://github.com/securelyfitz/sao-arms) 
* human
* robo1 - straight roboto arms
* robo2 - bent robot arms
* tent1 - straight tentacles
* tent2 - bent tentacles
* bug1 - straight bug arms
* bug2 - bent bug arms
## [Faces](https://github.com/securelyfitz/sao-faces)
* terminator to terminate your I2C signals with pullups
* badgewife for badgelife diehards
* smiley for dade murphy fans
## addons/adapters
* [sao-iic]() to attach many common iic breakout boards
* [sao-flipper]() to flip a backwards SAO
* [sao-stripper](https://github.com/securelyfitz/sao-stripper) to attach neopixel strips over SAO
* [sao-rotate]() to rotate an sao 90 degrees
* [sao-45]() to rotate an sao 45 degrees
## bonus badges 
* rpi-shat SAO Hat for raspberry pi
* [hdmi3c](https://github.com/securelyfitz/hdmi3c) hdmi-to-sao version

# Building your Badge
1. Aquire parts. You need:
* a face and a long 4-pin header
* 1 or more arms
* a microbadge
* any other LEDs or addons
* a battery holder for your face or badge
2. Solder the long header onto the backside of your face
3. Solder the appropriate battery holder to your badge (challenging) or the back of your face (much easier)
3. Skewer your arms onto the long header
4. plug the long header onto the microbadge
5. add some LEDs or addons
6. Take a picture and thank [@securelyfitz](https://twitter.com/securelyfitz) and/or [https://twitter.com/securinghw](@securinghw) on twitter.

Due to size, space, and flexibility, the orientations of the headers are not all clearly marked.
## microbadge
Hold the badge with the SAO facing you and the usb port pointed down. VCC is the upper left
## arms
Line one of the arm's SAOs up with the SAO on your badge. The other SAO is wired in the same orientation, so VCC would be upper left on both SAOs no matter how you orient it.
## faces
With the face looking at you, VCC is in the upper left. Remember that the face's SAO port is supposed to be pins on the bottom that attach to the microbadge.

# Programming your Badge
The microbadge is, for all intents and purposes, a digispark clone. The proper micronucleus bootloader and a flashy light firmware has already been flashed.

To get set up, follow the [directions on digispark's website](https://digistump.com/wiki/digispark/tutorials/connecting) to set up your digispark board.

The only difference is that, since the microbadge does NOT get power from usb, it needs a battery to get programmed. The best approach I have found is:
1. remove the battery from your microbadge, or detach the battery-holding face
2. plug in a usb cable to your microbadge
3. click upload in arduino
4. when the prompt says 'Plug in device now', either pop in the battery or attach the face again.

