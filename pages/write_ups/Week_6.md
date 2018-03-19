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

**Download for this project's code:** [Here](https://goo.gl/8maVK6)

#### Includes
For this project, no extra libraries are required.
#### Variables
1. First, three variables are needed to represent the red, green, and blue pin numbers of the RGB led. Feel free to use an array instead of three separate variables.
  * Remember to connect the RGB pins to PWM digital pins on the Arduino, they are marked with a tilde (~).
2. Next, an array of type integer is needed to represent the Red, Green, and Blue values we want the RGB led to flash at. each value is from 0 (0v) to 255(5v - max brightness for that color). Make sure to keep the order of Red, Green, and Blue.
3. Three variables that represent the delay time between dots, dashes, and words in Morse code are needed.
  * The delay between dots is usually between 50 to 200ms.
  * The dash delay is usually 3 times the length of the dot delay.
  * The delay between words is usually 4 - 7 times the dot delay.
4. In order to find letters and convert them to Morse, we need a String array that has each letter's Morse code in alphabetic order.
5. Then we need an array of type char that contains the char value of each letter in the alphabet, in alphabetic order.
6. For numbers, we need the same thing. A String array that contains the Morse code for numbers 0 to 9 in that order, followed by a char array that contains the char value of each number from 0 to 9 in that order.
  * We do this so when we iterate through these arrays with for loops, the alphabet letter at index i corresponds with the Morse code array for that letter at index i.

```c++
int redPin = 11;//pin number connected to red pin of RGB led
int greenPin = 10;//pin number connected to green pin of RGB led
int bluePin = 9;//pin number connected to blue pin of RGB led

int flashColor[3] = {50, 100, 255};//color for LED to flash when transmitting morse code, values are R, G, B with each one having 0(off) to 255(full color).

int dotDelay = 200; //dot duration can be adjusted for how fast or slow you want display speed to be. 50-200ms is good
int dashDelay = dotDelay * 3; //dash timing is length of 3 dots
int wordDelay = dotDelay * 5;//duration between words can be from 4 to 7 times the dot duration

String morseLetters[26] = {".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-", ".-..", "--", "-.", "---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-", ".--", "-..-", "-.--", "--."};//this contains the morse code of each letter in alphabetic order
char alphabet[26] = {'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'};//alphabet of chars to use for comparing to input values

String morseNumbers[10] = {"-----", ".----", "..---", "...--", "....-", ".....", "-....", "--...", "---..", "----."};//numbers 0-9 in morse code in that order
char numbers[10] = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};//numbers as char values in 0-9 order
```
#### Functions
This program utilizes four functions. The first function sets the color of the RGB led based on the inputted values of Red, Green, and Blue into the function. Another function handles the flashing and delaying of the led depending on whether the inputted character is a dash, a dot, or a space. The third function takes a String of Morse dashes and dots and runs the flashing function on each one. And the last function takes a message in plain text, converts it into Morse, then plugs the Morse code into the aforementioned function.
##### Set Color Function
1. The function needs three arguments of type integer that corresponds to the red, green, and blue value to set the RGB led to.
2. In order to write PWM voltage to a pin, the function analogWrite(pin, val) is used.
3. So the redPin will be written with the redValue, and the same for blue and green.

```c++
 /*
 * This function takes in the Red, Green, and Blue values that the user wants to display on the RGB led and displays them on the LED
 */
void setColor(int redValue, int greenValue, int blueValue) {
  analogWrite(redPin, redValue);//writes red value to redpin, analog write means the voltage can vary, not just being 0v or 5v as digital write does.
  analogWrite(greenPin, greenValue);//writes greenvalue to greenpin
  analogWrite(bluePin, blueValue);//writes bluevalue to bluepin
}
```
##### Flash dash or dot Function
1. This function has one argument of type char. This char will either be a dot, a dash, or a space.
2. Now the inputted char needs to be checked if its a space. If it is a space, then the program needs to be delayed by amount of word delay and not flash the led. To do this, inside the if statement and after the delay the return operator is used to end the function there.
3. Next, the LED is turned on using the setColor function with the 3 values in our flashColor array to set the color.
4. The char then needs to be checked if it is a dash or a dot, and delay the appropriate amount of time.
5. After those if statements and the delay, the led is turned off using setColor(0,0,0).
6. Finally, a delay of time dotDelay is ran as that is the time between flashes in Morse code.

```c++
/*
 *This function takes a single character that is either a dash, or dot, or a space, and flashes the led for the corresponding time that each character requires.
 */
void flashDashOrDot(char dashOrDot){
  if(dashOrDot == ' '){//if the char is a space, then that is the end of the word
    delay(wordDelay);//delays for the amount of seconds of wordDelay
    return;//this works like a break in a loop, return ends the function here and doesnt run the code after this. We want this because if it is a space, the led should not flash.
  }
  setColor(flashColor[0], flashColor[1], flashColor[2]); //turn LED on with specified color
  if(dashOrDot == '.'){//if the char is a dot
    delay(dotDelay);//delay for dotDelay
  }else if(dashOrDot == '-'){//if the char is a dash
    delay(dashDelay);//delay for dash delay
  }
  setColor(0, 0, 0);//turn off LED
  delay(dotDelay); //create gap between flashes
}
```
##### Flash Sequence Function
1. This function takes one argument of type String that is a sequence of Morse dots, dashes, and spaces.
2. For each character in the sequence String, the flashDashOrDot function is ran on it.
3. This is done by using a for loop that runs for the length of sequence String, using String.length() to find this value.
4. After the entire sequence has been shown on the led, the sequence is printed to the serial console to tell the user what was just displayed in Morse code.

```c++
/*
 * This function takes a sequence of morse code dashes and dots in the form a string and displays each one on the LED.
 */
void flashSequence(String sequence){
  for(int i = 0; i < sequence.length(); i++){//runs for the length of the sequence, using the length() function. Index starts at 0 and runs to length() - 1
    flashDashOrDot(sequence.charAt(i));//runs flashDashOrDot with the char at the current index of the string
  }
  Serial.println("Displayed: " + sequence);//show user morse code displayed after led is done flashing
}
```
##### Write Morse String Function
1. This function has one argument, a String of plain text that needs to be converted to Morse and then ran with the flashSequence function to display it on the LED.
2. First, the inputted String is trimmed and converted to lower case to make sure white space before and after the message is removed and that all of the characters are lower case. We convert the String to lower case because otherwise we need to check for the lowercase and uppercase value of each letter.
3. Next, a placeholder string that will contain the message converted into Morse is created and set to a String of nothing.
  * **DO NOT** leave it as String messageInMorse; without setting it to the value of "". If you do, you will not be able to add Strings to the this String using the plus sign (+).
4. Each character in the plain text message needs to be checked, so a for loop that iterates through each character of the String is used. The loop runs till the end of messageToDisplay.length().
5. Inside this for loop will be two separate for loops. One for loop that iterates through the numbers array and checks the current character in the messageToDisplay string against each number, and another for loop that iterates through the alphabet array and checks each letter against the current character.
6. The first loop has a single if statement that checks the current character of the messageToDisplay string using the charAt() function against the current number in the numbers array. So the messageToDisplay index value will be from the outer for loop, and the numbers index will be from the inner for loop.
7. If it finds a match, then it adds the Morse equivalent of that number to the messageInMorse string.
8. The next for loop will iterate through the alphabet array, and will check each letter in the alphabet against the current character in the messageToDisplay String. If it finds a match, it adds the Morse equivalent of that letter to he messageInMorse String.
9. After the end of that for loop, one final if statement is need to check if the current character is a space. If it is a space, then it adds a space to the messageInMorse String to indicate the beginning of the next word.
10. After all of the for loops, the messageInMorse String now contains the entire messageToDisplay String converted into Morse code. This String is then ran with the flashSequence function to flash the Morse code to the led.

```c++
/*
 * This functions takes a message in plain text as a string as an argument, then converts that message to morse. Then it runs the flashSequence function to flash the sequence of morse code.
 */
void writeMorseString(String messageToDisplay){
  messageToDisplay.trim();//trim() functions removes spaces at the beginning and end of the string, so " h b " becomes "h b"
  messageToDisplay.toLowerCase();//toLowerCase() changes all characters in the string to lowercase, so we dont have to check for a lowercase or uppercase character

  String messageInMorse = "";//empty string that will hold the morse code - and . of the characters in the plain text string

  for(int currentCharacter = 0; currentCharacter < messageToDisplay.length(); currentCharacter++){//runs for the length of the messageToDisplay string.

      for(int numCheck = 0; numCheck < 10; numCheck++){//checks the current character in the messageToDisplay string against all 10 numbers in the numbers array and if it finds one, add the morse code equivalent to the messageInMorse string

          if(messageToDisplay.charAt(currentCharacter) == numbers[numCheck]){//if the currentCharacter in the messageToDisplay string is the same as the number in the number array at index numCheck

            messageInMorse = messageInMorse + morseNumbers[numCheck];//add morse code for that number to the messageInMorse string

          }

      }

      for(int charCheck = 0; charCheck < 26; charCheck++){//checks current character in the messageToDisplay string against all 26 letters in alphabet until its finds a match, then adds that letter's morse code to the messageInMorse string.

          if(messageToDisplay.charAt(currentCharacter) == alphabet[charCheck]){//if the current character equals a letter in the alphabet array

            messageInMorse = messageInMorse + morseLetters[charCheck];//add that letter's morse code to messageInMorse string

          }

      }//end for loop

      if(messageToDisplay.charAt(currentCharacter) == ' '){//if the current character is a space

        messageInMorse = messageInMorse + " ";//add space to messageInMorse string

      }

  }

  flashSequence(messageInMorse);//flash the entire messageInMorse string to LED
}
```
#### Setup
1. The three RGB led pins need to be set to pin mode output.
2. The setColor(0, 0, 0) function is than ran to much sure the LED is off.
3. Serial is then started with a baud rate of 115200.
4. Finally, a line is printed to the serial buffer to prompt the user to enter the text they wish to be translated into Morse code.

```c++
void setup() {
  pinMode(redPin, OUTPUT); //set pin modes of the 3 RGB led pins to output
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
  setColor(0, 0, 0);//make sure RGB LED is off
  Serial.begin(115200);//start serial with baud rate 115200
  Serial.println("Enter the text you want to be translated into morse code.");//prompt the user to enter a message to be translated into morse
}
```
#### Loop
1. When there is data available to be read from the Serial buffer, it must be converted into a String ran as an input into the writeMorseString function.
2. Serial.available() is used to check if there is available characters to read from Serial.
3. A String variable is created that contains the data from the Serial buffer as a String.
4. A line is printed back to the user to tell them that the Arduino received their input.
5. Finally, the writeMorseString function is ran with the input variable created above.

```c++
void loop() {

  if(Serial.available() > 0){//if there is data to read from the user entering in data
    String input = Serial.readString();//set input to string value of user's input
    Serial.println("Recieved: " + input);//tell user they recieved the string
    writeMorseString(input);//convert input to morse and display it to rgb LED
  }

}
```
