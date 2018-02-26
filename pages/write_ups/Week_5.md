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

**Download for this project's code:** [Here](https://goo.gl/aumzpd)

#### Includes
No includes are required for this project.
#### Variables
For this project, variables containing the LED pins, the dice patterns, the button pin, and a variable that contains the row number in the dice pattern multidimensional array for turning off all of the LEDs will be needed throughout the program.
1. First, create an array of type integer that contains all of the LEDs pin numbers. The order the pin numbers should be in is: Bottom left, Left middle, Top left, Bottom right, Right middle, Top right, and Middle.
  * Because of the way the numbers appear on the die, bottom left and top right pin numbers can be swapped, along with the both the left and right middle, and the bottom right and top left.
  * Creating the array in this order makes creating the dice patterns much easier.
2. Create a variable of type integer that contains the pin number the button is connected to. Doing this makes it easier if you wire the project again and end up changing pin numbers, you only have to do it once.
3. The dice pattern array is a multidimensional array that is 7 rows by 7 columns. Each row's values will contain a 0, which means the corresponding led pin number at the same index in the led pin array will be turned off, or a 1, meaning that the led pin number at that index number in the led pin array will be on.
  * The order of each value will turn off or on the corresponding led in the led pin array.
  * Each row will correspond to the number to display on the dice, so for example, turning all of the leds off except the middle one will display the number 1 on the dice.
4. Finally, create an integer variable that contains the number 6, which will be the row number in the dice pattern array that turns off all leds.

```c++
int ledPins[7] = {16, 15, 8, 14, 10, 11, 9};//Order: bottomLeft, leftMiddle, topLeft, bottomRight, rightMiddle, topRight, middle
int buttonPin = 19; //pin button is connected to, in this case A5 or digital pin 19

int dicePatterns[7][7] = { //LOW value is equal to 0 and HIGH value is equal to 1, so these are patters of which pins to turn off (0) and on (1).
  {0, 0, 0, 0, 0, 0, 1}, // Dice number 1
  {0, 0, 1, 1, 0, 0, 0}, // Dice number 2
  {0, 0, 1, 1, 0, 0, 1}, // Dice number 3
  {1, 0, 1, 1, 0, 1, 0}, // Dice number 4
  {1, 0, 1, 1, 0, 1, 1}, // Dice number 5
  {1, 1, 1, 1, 1, 1, 0}, // Dice number 6
  {0, 0, 0, 0, 0, 0, 0}  // blank, turn all LEDs off
};

int blankPattern = 6; // row number in dicePatterns array of blank pattern
```
#### Functions
This project will make use of two functions, one that will turn on the leds to display the number that is given as an argument, and another that contains the logic for "rolling" the die.
##### Show function
The show function will have a return type of void. It has one argument of type integer that is the value of the number to show. The function will contain a for loop to iterate through the led pin array and the dice pattern array to turn on or off the leds. In this case, the number to show corresponds to the row number in the dice patterns array. The for loop will iterate through the columns of the dice patterns array.
  * In this case, the argument value should be a number from 0 to 6.

```c++
/*
 * This function takes the number to show on the dice, which is the corresponding row number in the dicePatterns array,
 * then turns on the corresponding LEDs.
 */
void show(int numberToShow){
  for(int i = 0; i < 7; i++){//iterate through all of the columns in the specified row
    digitalWrite(ledPins[i], dicePatterns[numberToShow][i]);//set current led to the value in the dicePattern row specified by numberToShow and the current column
}
```

##### Roll the dice function
This function contains the logic for rolling the die. It will have a return type of void since it is just running our code then stopping.
1. For the variables that will be used for the function, one will contain the result of our random number generation that picks the number to display on the dice, the other one will be a random number that calculates how long our "roll" will be.
  * The "roll" in this case will be a short animation that shows as random numbers appearing on the dice quickly then slowing down as it reaches the end of the roll. This is just for show and to make the die look like its being rolled instead of just displaying a random die number.
  * The length of roll can be adjusted to make it longer or shorter as wanted.
2. The actual roll logic is a for loop that runs until the iterating variable is greater than the length of roll variable.
  * The result variable is updated with a random number from 0 to 5 that corresponds with the numbers 1 to 6 in the dice pattern variables.
  * The show function will run after in order to show that dice number on the die.
  * Then the program is delayed in order to allow time for the leds to show. To make the animation seem more fluid, the delay needs to increase as the our iterative variable is getting closer to the value of the length of roll variable. To do this, the delay function is ran with a base number of 50ms plus the iterative variable times 10, so as the iterative variable gets larger the delay becomes longer.
3. After the animation, the final number in the result variable will be shown to the user to signify the dice number that the random function has chosen.
  * To do this, another for loop is used to flash the chosen dice number three times before turning off all of the leds.
  * To flash the chosen dice number, first must show the blankPattern, delay for some time, show the result pattern, then delay again. The for loop will iterate three times to flash this three times.
4. After flashing three times, the leds are cleared.

```c++
/*
 * This function plays a small animation that shows a bunch of random dice numbers for a few seconds, then randomly chooses a number to show and flashes it 3 times before turning off all LEDs.
 */
void rollTheDice(){
  int result; //placeholder for result of random number generation
  int lengthOfRoll = random(15,25); //randomly chooses the length of the roll animation from 15 - 24

  for(int i = 0; i < lengthOfRoll; i++){ //runs for the length of the lengthOfRoll variable
    result = random(0,6); //chooses a number 0-5 to display on the die.
    show(result); //displays result
    delay(50 + i * 10); //delays 50ms plus i * 10, which as i increases the time between each animation is slower.
  }

  for(int i = 0; i < 3; i++){ //shows the final pattern pick to be displayed by flashing it three times
    show(blankPattern);
    delay(500);
    show(result);
    delay(500);
  }
  show(blankPattern); //after showing pattern, turn off all LEDs
}
```
#### Setup
1. In the set up function, a for loop is used to iterate through the led pin array and set all of them to pin mode output and make sure they are turned off.
  * Quite like how the pins for the buttons were set up in the [Piano](Week_3.md) project.
2. After that, the pin mode of the button needs to be set to INPUT_PULLUP, so that when the button is pressed, the pin reads LOW (0v).
3. Finally, randomSeed(analogRead(0)) needs to be ran so that each time the random function is called, a different seed is used to generate different numbers.
  * For example, if instead randomSeed(3) is used, the random numbers will always be generated with a seed of 3. In this case, the same exact random number will be generated each time the random function is called.

```c++
void setup() {
  for(int i = 0; i < 7; i++){//set all LED pins to output and makes sure they are turned off
    pinMode(ledPins[i], OUTPUT);
    digitalWrite(ledPins[i], LOW);
  }
  pinMode(buttonPin, INPUT_PULLUP); //sets buttonPin mode to INPUT_PULLUP
  randomSeed(analogRead(0));//makes sure there is a different seed each time random() is called
}
```
#### Loop
In the loop function, the button must be check so when it is LOW the roll the dice function is called. After that, a delay of 100ms is used just to make sure the Arduino has time to update.

```c++
void loop() {
  if(digitalRead(buttonPin) == LOW){ //if button is pressed
    rollTheDice(); //roll the dice
  }
  delay(100);//delay 100ms
}
```
