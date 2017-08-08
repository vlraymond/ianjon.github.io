---
Title: 2017 Ice Cellar Progress update(s)
layout: default
---
# This project is protected under the MIT License. 
June 14, 2017: 
Managed to create a external power source for my raspberry pi to power it. The down arrow indicates that the temperature is lower than the last recorded temp. 
![Here's the image](http://i.imgur.com/ujtKcgH.jpg)  


June 14,2017:
Update on Battery Pack: 4 D sized duracell batteries( To my conclusion that they're 14,000 mAh batteries) last about 19 hours while doing two recordings. One to a text file while the other uploaded it to an underground weather station. 

Got a graph working at the moment for temperature and time. Working on making it so that it'll update the graph by itself instead of having to re-run the program to get an updated graph. 

![Raspberry Pi Image](http://i.imgur.com/MFdY0VO.png) 


June 16, 2017: Social meadia post. Got contacted from an NSB-Mayors office persononel asking to send as a widespread email across the northslope

Alaska Eskimo Whaling Commission manager/employee asking if she could try and get us on the agenda for a meeting being held her in fairbanks in the month of July about the projects. As in try and present it at AEWC. 

June 19,2017: Figuring out why matplotlib.pyplot isn't functioning on the pi. Function runs but the inteded source that I followed shows a live graph, while mine just shows a blank screen. 

June 20, 2017: With the help of Ross a bit, I got the code to work on python 2.7.9 for a live graph running but not for python 3. 

June 21, 2017: Copy and pasted the code for python 3 into a python 2 script and changed a weee bit on the code to get proper temperature.
![Image of Live Graph](http://i.imgur.com/kUcKDAH.jpg) 

June 26,2017: First time messing around with a raspberry pi bread board and getting to know some of it's features. Trying/going to implement a cat 5 cable so that we can put a sensor into the cellars without the pi having to be down there itself. Also finished my first draft for a questionaire to ask the public about their ice cellars so that we can get an idea of what materials we need for some and what materials we need for the length of cables. 

June 29, 2017: Messing around with the DHT11 sesnor and kinda fried it because I didn't know the breadboard needed a resistor. Met up with Vladimir R. . Back and forth on the project and ideas of to exapnd into different projects, such as permafrost ice. Was a little surprised when I told him I got a large community feedback that people are interested. Overall said this project looks very good. So woo! Looking at my presentation and I'm wondering what else to add to it.

July 06 2017: Finished my draft presentation. Updating the presentation and making an outline to what I'll be saying on paper for the presentation. 

July 13 2017: Learned about a application/web service called thingsboard.io. Essentially it is a very friendly user interface that anyone can navigate and apply to your needs, as long as you have a network connection and the device(s) needed to start monitoring. 

Sam and I setup a raspberry pi with a DHT11 sensor and managed to get it running on the webservice in about 15 minutes. This will help out with the Ice Cellar Project for cellars near a home, which hopefully has a network. With those that are near a network, it will be able to send data to the webservice. Depending on the setup of the webservice, it could send an alert to the client/owner of the cellar when the temperature dips/passes a certain threshold, which in terms can save the meat through an automated alert system. The alarm can be email, text, or however you setup. Though it can get a bit complicated if it's other than those two. The draw back on using this system though is that some of the cellars will not be near a network. Through modifying the scripts, we then can have it log into a text file and plot a live graph.It's lags about 5-10 minutes on updating to the site, so give it a little time and you'll see the change if there is any. Held a lighter to check and see if the temperature changed on the sensor, behold it does affect the temperature sensor. Here's what the initial widget looks like without changing anything to it: 

![Widget image of Thingsboard](http://i.imgur.com/l5zSY8E.png) 

July 18 2017: Looking over the presentation a bit and practiced some live runs with the staff today and on July 14, 2017. Great feedback, though on the 14th the practice run I used a piece of paper. Today I didn't use it and it went more smoothly. Started working on a brand new pi to write down some of the work it takes to get to work at the moment. 

July 19 2017: Went in and did the presentation infront of the AEWC board. They seemed very interested in the project and pleased with the presentation that I gave. Offered to follow out on a village trip to talk to students/kids about the S.T.E.M. program. I said yes and will be heading out on July 30th to Barrow for a day then off to Point Hope the next day. Will be meeting up with some of the other workers and give them.

July 26 2017: Almost done with a live demo kit to bring along with me to the village of Point Hope. 
