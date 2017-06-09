---
title: Setting up the pi
layout: default
---

So here's a link to download the operating system onto the SD card/micro SD card if it doesn't come with it installed or it manages to go very wrong. 
The link also helps with your first install of the operating system.
(https://www.raspberrypi.org/help/noobs-setup/2/)

(Picture below is reference for what to pick as the operating system)
![Picture for reference on what to pick](https://www.raspberrypi.org/wp-content/uploads/2013/06/mainwindow.png)
Install the raspbian OS and there you have it, a working little mini computer.

The next step is to open the home button, which should be at the top left side of the screen. Scroll to preferences and change your keyboard layout to English US (Default is british layout, which can make you go a little crazy.)



![Image of what the screen should look like when you are changing the keyboard layout](http://guides.cyntech.co.uk/wp-content/uploads/sites/8/2015/09/raspi-configgui.png) 

After that, open up the terminal window (which should be the monitor icon on the top bar of the screen). 

![Image of what the terminal should look like](http://osoyoo.com/wp-content/uploads/2016/09/2-1.png)

Then type in the terminal : sudo apt-get update

Then when it's done, type in : sudo apt-get upgrade

After that's all said and done, we need to install the packages for Sense HAT. Type in the terminal after the update is done: sudo apt-get install sense-hat

Once that is done, you'll need to make a directory folder. We'll continue through this from the terminal. 
type in the terminal 
: mkdir pi_weather_station

This should create a folder in the home, with the folder named pi_weather_station
