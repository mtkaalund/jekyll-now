---
title : Adding an USB hub to keyboard
layout : post
---
So long time a go, I saw this keyboard with an usb hub build in, I think it was apple that had made it, I really can't remember it. But anyway I wanted a keyboard with an usb hub buildin, not that it would make my life easier but because then I would have one less thing on my table.

I have two usb hub, one with an external power supply and one with out the external power supply, but this one has tree port in front, and one in the back, which suites the purpurs of this mod, just fine.
So the plan is use the one in the back for the keyboard, an the tree others for other usages.

### Hardware
* Keyboard [Logitech Internet 350 USB Keyboard](http://www.logitech.com/en-roeu/for-business/products/keyboards/devices/585) 

![Logitech Internet 350 USB Keyboard](/images/2011-11-09-adding-an-usb-hub-to-keyboard/logitech_350_keyboard.png)

* USB Hub [Targus USB Hub ACH93EU](http://www.targus.com/uk/drivers_manuals.asp?SKU=ACH93EU)

![Targus USB Hub ACH93EU](/images/2011-11-09-adding-an-usb-hub-to-keyboard/targus_usb_hub.jpg)

## The plan
I have put my plan into steps:
1. Disassemble the usb hub and keyboard
2. Disconnect the usb cable from the keyboard
3. Disconnect the usb cable from the hub and disconnect one of the usb ports
4. See where the usb hub pcb can fit in the keyboard
5. Drill/cut 3 ports into the keyboard's case
6. Wire the keyboard directly to the disconnected port on the usb hub
7. Wire the keyboards old usb cable to the usb hub
8. Check if it works
9. Close it up
So this is the plan, and the time for implementing it.

## Implementing the plan
First disassemble the usb hub and found this:

![Usb hub disassemble picture 1](/images/2011-11-09-adding-an-usb-hub-to-keyboard/usb-dis-1.jpg)

![Usb hub disassemble picture 2](/images/2011-11-09-adding-an-usb-hub-to-keyboard/usb-dis-2.jpg)

Take note of where the wires are connected (taking a picture, is a good way to do it), as we need to make sure where they connect. This is done because some of the connections can be altere as of the PCB design. Not to solder one of the usb header, power connector and the wires off, like this.

![Usb hub after soldering picture 1](/images/2011-11-09-adding-an-usb-hub-to-keyboard/usb-solder-1.jpg)

![Usb hub after soldering picture 2](/images/2011-11-09-adding-an-usb-hub-to-keyboard/usb-solder-2.jpg)

Now to take the keyboard apart. As you properly can see from the pictures above I took both the usb hub and the keyboard apart at the same time. In this keyboard there is a lot of screws, it is almost silly that there is so many screws. There is two screws under the keys "<b>Caps-lock</b>" / "<b>Tab</b>" (you need to remove those two to get to one screw) and one screw under the "<b>+</b>" on the <i>numberpad</i>. On the bottom there is, about 18 screws, I told you that is silly how many screws there is. Remove those screws.
Now you should be able to open the keyboard, there you can see the workings of the keyboard, the one thing we are interested in is the PCB where the usb connection are connected to.
Again take note of where the wires are connected, are going to remove the wires:

![Keyboard pcb disassemble picture 1](/images/2011-11-09-adding-an-usb-hub-to-keyboard/keyboard-dis-1.jpg)

After removing the wires, remove the silicone buttons. Now it is time to layout where the usb hub, is going to be:

![Keyboard and Usb hub layout picture 1](/images/2011-11-09-adding-an-usb-hub-to-keyboard/keyboard-usb-layout-1.jpg)

When you have found the place where, you'll want to have the usb switch, then mark where the usb ports are, and start cutting about half of the usb ports. Here I will suggest that you use an dremel cutting tool or something like it. I used an knife, and it took a long time and I got some cuts on my fingers. 
After being finished with the bottom half, then proceed to the top half, and mark an cut it.

Then it is time, to add the new wires. My first thought was to use the usb cable from the usb switch, but I was'nt able to resolder them on, not with out some of the wires, pulling out when soldering a wire. So I did use some scrap wires, that I had lying around, for the usb connection for the usb keyboard.

![Keyboard wires solder](/images/2011-11-09-adding-an-usb-hub-to-keyboard/wire-solder-1.jpg)

![Usb hub wires solder](/images/2011-11-09-adding-an-usb-hub-to-keyboard/wire-solder-2.jpg)

As there can be seen on the last picture above, I have already solder the usb cable from the usb keyboard on the usb switch, that cable was a better quality than the one from the usb switch.
Some pictures of the finished product in use:

![Finished picture 1](/images/2011-11-09-adding-an-usb-hub-to-keyboard/finished_1.jpg)

![Finished picture 2](/images/2011-11-09-adding-an-usb-hub-to-keyboard/finished_2.jpg)

It works, quite well, but I have noticed that it gets hot, where the usb switch is placed, so perhaps I'll have to do something about it at some point.

### Reference:
* [USB Pinout](http://pinouts.ru/Slots/USB_pinout.shtml)
* [USB Cable Pinout](http://pinouts.ru/SerialPortsCables/usb_cable_pinout.shtml)
