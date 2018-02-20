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

**Download for this project's code:** [Here](https://goo.gl/fMkBW6)

#### Includes
There are two libraries needed for the project. The pitches library for the buzzer, which the download link can be found [here](https://goo.gl/JVeKwG), and the SevSeg library, which can be downloaded from the Arduino library manager.

```c++
#include <SevSeg.h> //include required libraries
#include "pitches.h"
```
#### Variables
Two variables are needed for use in the rest of the project, a variable that contains the pin number of the buzzer and a SevSeg object. An object is simply variable that contains functions that are pre written.

```c++
SevSeg sevseg; //create a SevSeg display object
int buzzerPin = 19; //create a variable for the buzzer pin number
```
#### Functions
This program requires two functions, a function that delays the program for a certain amount of **seconds**, and a function that contains the code used to run the count down timer.  
Both of these functions will have a return type of void.  
##### Delay Function
The delay function will make use of the Arduino's millis() function, which returns the time since the Arduino started running the code in milliseconds.  
1. The function needs one argument: the amount of seconds to be delayed.
2. Inside the function, a variable that contains the start time in seconds must be created, of type double. 
	- To get seconds, milliseconds must be divided by 1000, so this variable will equal millis()/1000.  
3. Create a loop that runs until the current time in seconds (millis()/1000) minus the startTime variable is less than the number of seconds of the amount of seconds delayed argument.
4. Inside the loop, put sevseg.refreshDisplay() to make sure the numbers are displayed properly.

```c++
void secondDelay(int numOfSeconds){
  double startTime = millis() / 1000; //start time in seconds
  while( (millis()/1000) - startTime < numOfSeconds){ //runs while the difference between the current time and start time in seconds is less than the delay seconds
    sevseg.refreshDisplay();//refreshes display
  }
}
```
##### Count Down Function
This function will hold all of the logic for displaying the count down on the display.
1. The function needs two integer arguments: one that contains the minutes of the countdown and one that contains the seconds.
2. All of the code will be put into a while loop that runs forever, to do this write while(true). Break statements will be used to end the loop when needed.
3. Inside the loop, an if statement must check whether another count down as been entered, using the Serial.available() function. If there has been another count down entered then the loop is exited out of. 
4. If there hasn't been another count down entered, then the count down logic is ran.
5. A placeholder variable is created of type string to hold the full count down number.
6. Then, an if statement is needed to check whether the inputted seconds is less than 10. This is because an integer variable with a number 1 to 9 does not have a leading zero, like 01 or 09.
	- If the seconds is less than 10, then the fullNumber variable will be set with the minutes in String, plus a 0 in string form, and the seconds variable in String.
7. However, if the seconds is greater than 10 then the full number is just the minutes in String plus the seconds in String.
8. Now that the fullNumber in a String variable is created, sevseg.setNumber() is used to set the number on the seven segment display.
	- Since the number is a String, we can convert it to an integer using the toInt() function.
	- So, for example: sevseg.setNumber(fullnum.toInt(), 2) The 2 lights up the second decimal on the board.
9. Once the display is set, the delay function created above is used to delay the program for 1 second.
10. Then, the program must know when the timer is up or the end of the minute is reached. To do this, an if-else-if statement is used.
11. If the seconds is between 60 and 0, then 1 is subtracted from seconds and the display is updated.
12. If the seconds is 0 but the minutes is greater than 0, then the next minute is reached. So 1 is subtracted from the minutes variable and seconds is set to 59 to start the next minute.
13. If both minutes and seconds is zero, then the count down is done. A tone is played and the loop ends.

```c++
void countDown(int minutes, int seconds){
  while(true){ //runs "forever"
    if(Serial.available() > 0){//if the user enters a new time while a countdown is active, end the current countdown.
      break; //ends loop
    }else{
      String fullNumber; //placeholder for the full number 
      if(seconds < 10){ //if seconds is less than 10, the seconds variable will be 9 instead of 09, so it needs to be fixed to display properly.
        fullNumber = String(minutes) + "0" + String(seconds); // adds a zero before the current second.
      }else{
        fullNumber = String(minutes) + String(seconds); //doesn't add a zero if seconds is greater than or equal to 10
      }
      sevseg.setNumber(fullNumber.toInt(), 2);//sets the sevseg display to the current countdown, with the decimal point at position 2
      secondDelay(1);//delays for one second.
      if(seconds <= 60 && seconds > 0){ //if seconds is between 0 and 60
        seconds--;//subtract one from seconds variable.
      }else if(minutes > 0 && seconds == 0){//if seconds equals 0 and minutes is greater than 0.
        minutes--;//subtract one from minutes.
        seconds = 59;//set seconds to 59 to countdown the next minute.
      }else if(minutes == 0 && seconds == 0){//if minutes and seconds both equal zero
        tone(buzzerPin, NOTE_C5, 1000);//play a tone to signal end of countdown.
        break;//end loop.
      }
    }
    sevseg.refreshDisplay();//needed to refresh the display properly.
  }
}
``` 
#### Setup
1. The set up for the seven segment display is a little complicated, so just use the code here and replace the port numbers with the ports you used in your setup.  
2. After that, the serial communication must be started using Serial.begin() with a baud rate of 115200.
3. A prompt is then printed to ask the user to enter in the count down time in MinuteMinuteSecondSecond.
4. **PLEASE NOTE! My display is COMMON_ANODE, however, yours should be COMMON_CATHODE. Please change COMMON_ANODE to COMMON_CATHODE.**

```c++
void setup() {
  /*
   * This is for the initialization of the seven segment display, pulled from the example testWholeDisplay in the SevSeg library.
   */
  byte numDigits = 4;//byte is just a generic datatype, contains the amount of digits our display has.  
  byte digitPins[] = {2, 3, 4, 5}; //Digits: 1,2,3,4 <--put one resistor (ex: 220 Ohms, or 330 Ohms, etc, on each digit pin)
  byte segmentPins[] = {6, 7, 8, 9, 10, 11, 12, 13}; //Segments: A,B,C,D,E,F,G,Period

  sevseg.begin(COMMON_CATHODE, numDigits, digitPins, segmentPins, false, false, true); //COMMON_CATHODE is the type of display we have, first false is setting resistors are on digit pins, second false is saying don't update with delays, and last true is saying to keep leading zeros
  sevseg.setBrightness(10); //not really the brightness, but how fast the display refreshes, which affects brightness.
  Serial.begin(115200);//start serial communication with baud rate of 115200
  Serial.println("Enter the time you want to countDown from as MMSS"); //prompts the user to enter a countdown time in form minute minute second second
}
```
#### Loop
In loop, the program checks for user input via serial communication. Then creates two integer variables for minutes and seconds based on the input and runs the countDown() function.
1. Serial.available() is used to check if there has been an input.
2. To tell the user the Arduino received their input, a line is printed telling them they received whatever has been inputted.
3. The substring() function is used to split up the minutes and seconds into separate variables, then toInt() is ran on the substring to turn it into an integer.
4. Lastly, the countDown function is ran.

```c++
void loop() {

  if(Serial.available() > 0){//if there are available bytes to read from serial
    String input = Serial.readString(); //set input to what the user entered as a string.
    Serial.println("Recieved: " + input);//tell user the Arduino Recieved their input.
    int minutes = input.substring(0,2).toInt(); //create an int variable for minutes equal to the first two characters in the input string and convert it to int.
    int seconds = input.substring(2).toInt(); //create an int variable for seconds equal to the last two characters in the input string and convert it to int.
    countDown(minutes, seconds); //run our countDown function.
  }
  
  sevseg.refreshDisplay(); //refreshes display
}
```
