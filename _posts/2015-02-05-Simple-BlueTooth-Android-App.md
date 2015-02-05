---
layout: post
title:  "Intro to the Bicycle Embedded System"
date:   2015-01-27 16:34:04
categories: bicycle intro
--- 

After realising that I pretty much only used 5 gears out of the possible 21 on my bike, I did some research into what the optimum gear combination would be and found a bunch of research papers such as [this][paper1] and [this][paper2] and more informal articles like [this][paper3] that suggested there is an optimal cycling cadence that roughly suits everyone. As ridiculous as it seems, from reading all these papers I and two of my friends decided it was possible to build a system that could suggest such a gear combination.

The basic premise (for now) is that there are two surfaces: flat and inclined. The optimal RPM on a flat surface is 80-100 RPM and the optimal RPM on an incline is 65-80 RPM. Note that this refers to the RPM of the pedal (aka crank) and is commonly referred to as the cadence.

Thus if our embedded system knows the incline and cadence of a cyclist, it can suggest gear combinations that will keep the rider within the optimal cadence window. The system will display this information on a little display on the handlebars along with other data like speed, distance covered etc.

The system in its current form has the following components (vendor sites are linked):

* [2.2' TFT Display][display]
* [GPS Module][gps]
* 2 x Tachometer
	* [Neodymium magnets][magnets]
	* [Reed Switch][reed-switch]
* [Arduino Uno][arduino-uno]
* [BlueTooth Module][jy-mcu]
* [Accelerometer][accl]
* [Dynamo][dynamo]
* [Li-Po Battery][lipo]
* Power converters/regulators
	* Boost converters
	* Buck converters
* Android App

The overall plan for the system is for the Arduino (which will later be replaced by a custom PCB) to be the MCU which will measure metrics like pedalling cadence (with the tachometer) and terrain (with the acceleration) to determine what the optimal cadence window is and to compare it with how the rider is currently doing. If the rider is over the upper threshold, a lower gear will be suggested to try and bring the rider back into that window. A GPS module will be tracking the user's location the whole time and when the ride is completed, its data along with avg. speed, total distance, elevation profile etc. can be transfered to an Android app over bluetooth. The system will be powered by a 4400mAh Li-Po battery and will be charged by a dynamo that can be engaged and disengaged.

The next few posts will focus on developing each subsystem after which the focus will shift to consolidation. The next few steps after that will be sending in the PCB for manufacturing, testing our system on several different riders and packaging. The timeline for the project is now till the first week of May. 


[paper1]:	http://jap.physiology.org/content/51/2/447
[paper2]: 	http://link.springer.com/article/10.1007/s004210050634
[paper3]:	http://www.spinning.com/community/the-science-of-optimal-cycling-cadence/
[display]:	http://www.adafruit.com/product/1480
[gps]:	http://www.adafruit.com/product/746
[magnets]:	http://www.amazon.com/Neodymium-Magnets-inch-Disc-N48/dp/B001KV38ES/ref=sr_1_1?ie=UTF8&qid=1423125734&sr=8-1&keywords=neodymium+magnet
[reed-switch]:	https://www.sparkfun.com/products/8642
[arduino-uno]:	http://www.amazon.com/Arduino-UNO-board-DIP-ATmega328P/dp/B006H06TVG/ref=sr_1_1?ie=UTF8&qid=1423125777&sr=8-1&keywords=arduino+uno
[jy-mcu]:	http://www.amazon.com/JY-MCU-Arduino-Bluetooth-Wireless-Serial/dp/B009DZQ4MG/ref=sr_1_1?ie=UTF8&qid=1423125792&sr=8-1&keywords=jy-mcu
[accl]:	http://www.amazon.com/SainSmart-ADXL335-Accelerometer-Breakout-Arduino/dp/B006J4G4FQ/ref=sr_1_3?ie=UTF8&qid=1423125918&sr=8-3&keywords=arduino+accelerometer
[dynamo]:	http://www.amazon.com/Bicycle-Dynamo-Bracket-6V-3W/dp/B00ITTT94C/ref=sr_1_5?ie=UTF8&qid=1423125982&sr=8-5&keywords=bike+dynamo
[lipo]:	http://www.adafruit.com/products/354