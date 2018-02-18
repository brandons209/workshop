---
layout: default
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

**Download for this project's code:** [Here](https://goo.gl/JVeKwG)

#### Includes
This project needs to include the pitches library, which defines all the notes that can be played on the buzzer.  
Download it [here.](https://goo.gl/P8tdAc)
```c++
#include "pitches.h" //include pitches library to define notes
```
#### Variables
1. An array of type integer is needed to contain 5 notes from the pitches library. The notes are all integers.  
2. A variable that contains the duration for the note to be played is needed as well, set it to 100 for now.  
3. To define the pins being used, create an array of type integer that contains the pin numbers of the 5 buttons. Then a separate variable of type integer that contains the pin number of the buzzer.

```c++
int notes[5] = {NOTE_C5, NOTE_D5, NOTE_E5, NOTE_F5, NOTE_G5}; //create five notes for buzzer to play, notes are of type int
int duration = 100; //duration of the note to be played

int buttons[5] = {2, 3, 4, 5, 6}; //buttons pins, in order of left to right
int speakerPin = 7; //pin of buzzer
```
#### Functions
For this project, no extra functions need to be created.
#### Setup
1. In set up, set the pinMode of each of the button pins to INPUT_PULLUP. Use a for loop to access each value of the button pin array.  
	- This effectively inverts the behavior of the INPUT mode, where HIGH(5v) means the button is not being pressed, and LOW(0v) means the button is being pressed.

```c++
void setup() {
  for(int i = 0; i < 5; i++){//set button pins mode to INPUT_PULLUP using a for loop
    pinMode(buttons[i], INPUT_PULLUP);
  }
}
```
#### Loop
1. The function digitalRead(pinNum) returns whether the pin is currently HIGH or LOW, use this to determine if the button is being pressed.  
2. When the pinNumber is reading LOW, the button is being pressed.  
3. If the button is returning LOW, we run the tone() function to have the buzzer play the tone we specified.

```c++
tone(speakerPinNum, note[index of note], duration);
```
4. Now, an if else if statement chain can be used to check each button for being pressed.

```c++
void loop() {  
  
  if(digitalRead(buttons[0]) == LOW){//if the pin reads LOW (0v), then play the tone
    
    tone(speakerPin, notes[0], duration);//plays first note with duration defined above.
    
  }else if(digitalRead(buttons[1]) == LOW){//next ones are the same as above except with next button and note.
    
    tone(speakerPin, notes[1], duration);
    
  }else if(digitalRead(buttons[2]) == LOW){
    
    tone(speakerPin, notes[2], duration);
    
  }else if(digitalRead(buttons[3]) == LOW){
    
    tone(speakerPin, notes[3], duration);
    
  }else if(digitalRead(buttons[4]) == LOW){
    
    tone(speakerPin, notes[4], duration);
    
  }
}
```
