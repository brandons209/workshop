---
layout: page_layout
---
* [Home](../index.md)
* [About](About.md)       
* [Lectures](Lectures.md)
* [Write Ups](Write_Ups.md)
* [Downloads](Downloads.md)
* [Programming Tutorials](Programming_Tutorials.md)
* **Robot Resources**
* [Contact](Contact.md)

* * *
## Controlling your Robot
**I highly suggest you use a PS4 Bluetooth controller or any Xbox 360 or One controller.**  
Bluetooth controllers are easy to set up and use, at least the ones mentioned above.
* To do this, you need an [Arduino USB Host Shield](https://store.arduino.cc/usa/arduino-usb-host-shield) and accompanying [Bluetooth wireless adapter](https://github.com/felis/USB_Host_Shield_2.0/wiki/Bluetooth-dongles).
* Pairing the controller is easy, see Lecture 2 for Robot Programming Lectures under [Lectures](Lectures.md)

## Motor Controllers
**For motor controllers, I highly suggest the** [Adafruit motor shield](https://www.adafruit.com/product/1438?gclid=CjwKCAiAqIHTBRAVEiwA6TgJw5LRyxHq3K9mnEQkyjCQKqnoeI4orKzAAj2KrmRVDnoR4DovjPvrMBoC-WAQAvD_BwE
), **as it is easy to wire and code for.**  
* It can control 4 DC motors or 2 Stepper motors. That means it can control 2 DC motors and 1 Stepper motor.
* All it needs is a dedicated power supply and its good to go! See the Robot Programming Lectures for more details on wiring and coding.

## Example Scripts for Arduino Robots
* This example uses the Adafruit motor controller shield and a USB host shield with PS4 controller over Bluetooth to control some motors. It also shows how you can do a tank drive with the y axis on both analog sticks.
  * [Basic robot with ps4 BT controller.](https://goo.gl/P4gi3Q)
