---
layout: page_layout
---
* [Home](../../index.md)
* [About](../About.md)       
* [Lectures](../Lectures.md)
* [Write Ups](../Write_Ups.md)
* [Downloads](../Downloads.md)
* [Programming Tutorials](../Programming_Tutorials.md)
* [Robot Resources](../Robot_Resources.md)
* [Contact](../Contact.md)

* * *

**Download for this project's code:** [Here](https://goo.gl/9HeVAP)  

#### Includes
For this project, there are no required includes.  
#### Variables
Only one variable is needed, an integer that contains the pin number the LED is connect to.  
```c++
int ledPin = 3;
```
#### Functions
For this project, no extra functions need to be created.
#### Setup
In the setup function, the pin mode of the led pin needs to be set to output, which means the pin will send out voltage when the code tells it to. To do this, use the pinMode(pin, OUTPUT) function.
```c++
void setup() {
  pinMode(ledPin, OUTPUT); //set pinmode of the ledpin to output.
}
```
#### Loop
1. In order to make the LED flash, it needs to be turned on, then the program needs to wait, then the led needs to be turned off, followed by another wait.  
2. This will give the appearance of the LED flashing. To set the output of the pin to HIGH (5v) or LOW (0v), use the digitalWrite(pin, HIGH or LOW) function.  
3. To make the program wait, use the delay(millisecondsToDelay) function.

```c++
void loop() {
  digitalWrite(ledPin, HIGH); //turn on led
  delay(50);//wait 50 milliseconds
  digitalWrite(ledPin,LOW);//turn off led
  delay(50);//wait 50 milliseconds
}
```
