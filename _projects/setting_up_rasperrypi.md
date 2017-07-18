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
Here is a video tutorial of how to setup the GPIO pins( wiring of the breadboard to the PI, most if not all kits will have the T-Cobbler attached. So for instance if he says negative to ground, just plugged the wire into the ground section of the T-Cobbler and so forth of that.) 
https://www.youtube.com/watch?v=DPvxsHoD7kc&t=694s  

Next we're going to install some necessary libraries. In the same command window you're going to type these commands to installt the libraries. I'll also post a link for where I got the command to install these, as they also have guides for these device(s). 

Thingsboard library install for DHT Sensors:
This library needs a DHT sensor along with the breadboard and some male to female jumper wires. So the commands to input are:

The MQTT library installation:``` sudo pip install paho-mqtt```

Adafruit DHT library installation for the command line window 

```git clone https://github.com/adafruit/Adafruit_Python_DHT.git``` , 
then after it's done installing, 

type ```cd Adafruit_Python_DHT``` in the command line window to change to that folder directory. Then in the directory, type:
``` sudo python setup.py install``` while you're in still in the same library, type ``` sudo apt-get update```. Reason is that when you try to run a script with the Adafruit_DHT Library, most of the time it'll say 'No import module named Adafruit_DHT' 

This next part is where you're going to have to open a python script to copy and paste into script window. You can option to type in in yourself, but you'll most likely end up misspelling a some words and it won't work until you fix it. The code you'll have to put in the script is: 
```
import os
import time
import sys
import Adafruit_DHT as dht
import paho.mqtt.client as mqtt
import json

THINGSBOARD_HOST = 'demo.thingsboard.io'
ACCESS_TOKEN = 'DHT22_DEMO_TOKEN'

# Data capture and upload interval in seconds. Less interval will eventually hang the DHT22.
INTERVAL=2

sensor_data = {'temperature': 0, 'humidity': 0}

next_reading = time.time() 

client = mqtt.Client()

# Set access token
client.username_pw_set(ACCESS_TOKEN)

# Connect to Thingsboard using default MQTT port and 60 seconds keepalive interval
client.connect(THINGSBOARD_HOST, 1883, 60)

client.loop_start()

try:
    while True:
        humidity,temperature = dht.read_retry(dht.DHT22, 4)
        humidity = round(humidity, 2)
        temperature = round(temperature, 2)
        print(u"Temperature: {:g}\u00b0C, Humidity: {:g}%".format(temperature, humidity))
        sensor_data['temperature'] = temperature
        sensor_data['humidity'] = humidity

        # Sending humidity and temperature data to Thingsboard
        client.publish('v1/devices/me/telemetry', json.dumps(sensor_data), 1)

        next_reading += INTERVAL
        sleep_time = next_reading-time.time()
        if sleep_time > 0:
            time.sleep(sleep_time)
except KeyboardInterrupt:
    pass

client.loop_stop()
client.disconnect() 
```
You're going to want to run this script in python 2. Script isn't reading that adafruit library if it's running in python 3
