---
layout: post
title:  "Using the JY-MCU Bluetooth Module with the Arduino"
date:   2015-02-07 16:34:04
categories: bicycle bluetooth arduino
--- 

Another component of the project aims to give the system the ability to stream data over bluetooth (BT) and ideally give the flexibility to have a phone act as a more advanced dashboard. The cheapest and most well documented bluetooth module out there is definitely the HC-06 JY-MCU module. Getting it to work isn't that hard and the connection is pretty good upto 30ft which makes it perfect for DIY projects.

The module has 4 pins:

* VCC (5V)
* GND
* TX
* RX

The module is interesting because the module is rated upto 5V but it's TX and RX communication lines are only rated upto 3.3V. This means that using the module isn't as easy as connecting jumper wires straight from the arduino to the module (although it is still very easy).

The circuit will use the following components:

* [JY-MCU bluetooth module][jy-mcu]
* [Arduino Uno][arduino-uno]
* 1 x 2.2kΩ resistor
* 1 x 1kΩ resistor   

The JY-MCU has a quirk where it works better when communicating with the Arduino over the [SoftwareSerial library][soft-serial] rather than the hardware Serial pins (pins 1 and 0).

<h1> Wiring up the JY-MCU </h1>

Since the TX pin of the Arduino will be outputting 5V onto the RX pin of the BT module, there will need to be a resistor divider circuit to step down this voltage to prevent damaging the BT module. The RX pin from the Arduino can be connected straight to the TX pin of the BT module as the module will output 3.3V onto that line and the Arduino is fortunately calibrated to read 3.3V TTL logic levels. VCC can be connected to either 3.3V or 5V (I've checked and both work reliably) and GND can be connected to any of the GND pins on the arduino.

[soft-serial]:	http://arduino.cc/en/Reference/SoftwareSerial
[arduino-uno]:	http://www.amazon.com/Arduino-UNO-board-DIP-ATmega328P/dp/B006H06TVG/ref=sr_1_1?ie=UTF8&qid=1423125777&sr=8-1&keywords=arduino+uno
[jy-mcu]:	http://www.amazon.com/JY-MCU-Arduino-Bluetooth-Wireless-Serial/dp/B009DZQ4MG/ref=sr_1_1?ie=UTF8&qid=1423125792&sr=8-1&keywords=jy-mcu