---
title: Setting up the pi
layout: default
---
# This Project is protected under the MIT License
# At the moment this is for the software side, this will be updated with pictures soon. 
### For Touch Screen Rotation
If you want to flip your screen along with its touch configuration, for example your setup is meant for the device to work upsides down, open the terminal window and type: ``` sudo nano /boot/config.txt``` and type in on the forth line : ```lcd_rotate=2```. 



So here's a link to download the operating system onto the SD card/micro SD card if it doesn't come with it installed or it manages to go very wrong. 
The link also helps with your first install of the operating system.
[Click here for the download](https://www.raspberrypi.org/help/noobs-setup/2/)

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
```sudo apt-get install python-dev ```then ``` sudo python setup.py install``` while you're in still in the same library, type ``` sudo apt-get update```. Reason is that when you try to run a script with the Adafruit_DHT Library, most of the time it'll say 'No import module named Adafruit_DHT' 

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

If you need to do the live graph auto updating on your screen, then you're going to need to install some more libraries. The next library you'll need to install is: ``` sudo apt-get install python-matplotlib```. Then copy this code into a python 2.7 script: 

```
import Adafruit_DHT
import time
from datetime import datetime
import csv
import sys
csvfile = "temp.csv"
als = True
while als:
    humidity, temperature = Adafruit_DHT.read_retry(Adafruit_DHT.DHT22, 4) 
    if humidity is not None and temperature is not NOne:
        humidity = round(humidity,2)
        temperature = round(temperature,2)
        print("Temperature = {0:0.1f}*F Humidity = {1:0.1f}" .format(((temperature*9)/5)+32 , humidity)
    else: 
        print('can not connect to sensor')
    timeC = time.strftime("%I")+':'+time.strftime("%M")+':'+time.strftime("%S")
    data = [((temperature*9)/5)+32 , humidity, timeC, time.strftime("%m-%d-%Y")]
    #This just puts into a file later that's being read constantly. 
    with open(csvfile, "a") as output:
        writer = csv.writer(output, delimiter = "," , lineterminator = '\n')
        writer.writerow(data)
    #this makes the script take measurements every minute, it's counted in seconds. Modify to your own timestamps of recording
    time.sleep(60)
 ```
 ### Hardware setup for DHT 22 Sensor: 
 You're going to need to know a little bit about the GPIO pins for the raspberry pi. Here's a chart displaying what each pin is used for. 
 
 ![GPIO Pin layout](http://blog.mcmelectronics.com/image.axd?picture=%2f2016%2f03%2fGPIO-Chart2.jpg)
 
 Now here's a picture of the raspberry pi itself with the GPIO pins. The right side of the diagram is going to be the top side of this picture. 
 ![Picture of Pi Pins](http://i.imgur.com/Y82rJjR.jpg)
 
 Now to introduce the DHT 22 sensor itself along with 3 female to female wires. 
![DHT 22](http://i.imgur.com/O4aikLg.jpg) 

The left side is the positive power connector, middle is the data send, and the right pin is the ground/negative pin for the DHT 22 sensor. 
Following the chart as it follows, we're going to connect it to suite the code above. 

I suggest plugging in the ground wire first before anything else is plugged in, that way you have a less likely chance of frying your first sensor. I highly suggest though that you have a spare or two because it is likely you might fry at least one sensor. 
![Connecting negative/ground to the pi](http://i.imgur.com/IsMgnXz.jpg)
You can pick any ground pin. Here we connect it to the 3rd pin on the top/right. Next we're connecting the postive power to one of the 5v pins on the pi, which is the first pin on the top/right of the pi. 
![Positive to positive pin](http://i.imgur.com/Cltoi90.jpg)

Last pin we're going to connect is the data pin, which connects to the 4th pin on the bottom/left of the raspberry pi, since our code is reading from GPIO pin 4, we have to match it.( You can change the code for the data pin placement, just remember to connect it to the right GPIO pin)
![Data pin to pi](http://i.imgur.com/oJ1ZbXu.jpg)

Here's how it should look with all 3 plugged into the raspberry pi
![All 3](http://i.imgur.com/GKRa5Fd.jpg)

Now that's all setup, you can now test the code to see if it works. Once that is done, you can extend the cable by attaching it to a solid core of ethernet cable by soldering, though attaching a resistor may be needed. For a cable about 50-80 feet long, you're going to need to solder a  2.2k ohm resistor somewhere along the lines of. Play around with it a bit with length and resistors to see your readings. 

So now that we have our file recording data at intervals, next is the script that'll update and provid you with a live graph of the data. 
    
```
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
import matplotlib.animation as animation
from datetime import datetime

fig = plt.figure()
rect = fig.patch
rect.set_facecolor('#0079E7')
def animate(i):
    ftemp = 'temp.csv'
    fh = open(ftemp)
    temp = list()
    timeC =list()
    humidity = list()
    for line in fh:
        pieces = line.split(',')
        degree = pieces[0]
        timeB= pieces[2]
        waterstuff= pieces[1]
        timeA= timeB[:8]

        time_string = datetime.strptime(timeA,"%H:%M:%S")
        try:
            temp.append(float(degree))
            timeC.append(time_string)
            humidity.append(waterstuff)
        except:
            print('What, not working')

        ax1 = fig.add_subplot(1,1,1,axisbg='white')
        ax1.xaxis.set_major_formatter(mdates.DateFormatter('%H:%M:%S'))
        ax1.clear()
        ax1.plot(timeC,temp,'c', linewidth = 3.3, color = "red", label= "Temp in *F")
        ax1.plot(timeC,humidity,'c',linewidth = 3.3, color = "blue", label="Humidity in %")
        plt.ylim(0,100)
        plt.title('Temperature&Humidity')
        plt.xlabel('Time')
        plt.legend(loc = "upper right")
ani = animation.FuncAnimation(fig,animate, interval= 6000)
plt.show()
        
```
### Setting up the ground sensor(Previous mentioned libraries will also need to be installed): 
You're going to need to install the library in the terminal window with the command: ```sudo apt-get install python-w1thermsensor```. Within the same terminal window, type in : ```debuild -us -uc```, then after that's done :```dpkg -i ./python-w1-thermsensor_*.deb```.

Now for the code you'll be typing into a python script: 

```
import os
import glob
import time
import csv
import paho.mqtt.client as mqtt
import json
csvfile='ground.csv'

Thingsboard = "IP ADDRESS LISTED FOR THE WEBSERVICE"
# Setup the thingsboard to your liking or contact the provider for assistance in setting it up via thingsboard.
Access_token = "ACCESS_TOKEN" #Just a temperorary password, set it to what you have it as listed on the webservice site

client = mqtt.Client()
client.username_pw_set(Access_token)
client.connect(Thingsboard,18883,20)
client.loop_start()

os.system('modprobe w1-gpio')
os.system('modprobe w1-therm')
base_dir = '/sys/bus/w1/devices/'
device_folder = glob.glob(base_dir +'28*')[0]
device_file = device_folder + '/w1_slave'

def read_temp_raw():
    f =open(device_file,'r')
    lines = f.readlines()
    f.close
    return lines
def read_temp():
    lines = read_temp_raw()
    while lines[0].strip()[-3] !='YES':
        time.sleep(0.2)
        lines = read_temp_raw():
    equal_pos = lines[1].find('t=')
    if equals_pos !=1:
        temp_string = lines[1][equals_pos+2:]
        temp_c = float(temp_string)/1000.0
        temp_f = temp_c *9.0 /5.0 +32
        return temp_c, temp_f

while True:
    system_data ={'temperature' : read_temp()[0]}
    client.publish('v1/devices/me/telemetry', json.dumps(system_data),1)
    print(read_temp())
    with open(csvfile, "a") as output:
        data = (read_temp(), time.strftime("%m-%d-%Y))
        writer = csv.writer(output,delimiter = "," , lineterminator = '\n')
        writer.writerow(data)
        time.sleep(1)
```

After that's done, go back into the terminal window and type : ```sudo nano /boot/config.txt```, scroll to the bottom and add this line to it : ```dtoverlay=w1-gpio```

