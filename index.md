# Knee Rehabilitation Device
The knee rehabilitation device is useful for ensuring the safety of the knee and preventing irritation by correcting a person's form when doing certain exercises. If incorrect form is detected, it emits a high pitched sound and vibration to notify the person that they are doing the exercise with incorrect form. An arduino setup is necessary as the sensor is needed to detect dangerous movements to have the output be the vibrations.

<!--You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:-->

```HTML 
```


| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Samvrat G | American High School | Computer Science | Incoming Senior


![Samvrat Gowda](/docs/assets/Samvrat_G.jpg)
  
<!--# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/y3VAmNlER5Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone 
-->
# First Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/watch?v=jykKqBEpDyc&list=PLe-u_DjFx7evDJ6N_vX36J16ru7SvHV5m&index=1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

My project is a knee rehabilitation device used specifically for squats. The device alerts the user to come up from a squat when their knee is bent 90 degrees. It also alerts the user when their knee is bending inwards during the squat movement as the bending can put stress on the knee joints and potentially the hip and ankle.

The device uses a flex sensor which is a variable resistor that increases resistance the more it is bent. By using the known 5V input along with a voltage divider, it is feasible to write code that determines the amount of resistance coming from the flex sensor. It is then possible to find the angle of the flex sensor based on the amount of resistance with the aid of some code. When the angle of the flex sensor is greater than 90 degrees, the buzzer goes off which alerts the user to come back up from the squat.

I am currently trying to get my Bluetooth module to connect to my phone by using MIT App Inventor to control several inputs through my phone. I am facing challenges with the print statements not working when connected to my Bluetooth module. Additionally, my phone cannot connect with my Bluetooth module which is hindering me from going any further with the mobile app.

The next step for this project is to use an accelerometer to detect if the knee is bending inwards during the squat. I need to run different trials with different knee positions to determine positions and accelerations that tell if the knee is bending inwards.

<!--
For your first milestone, describe what your project is and how you plan to build it. You can include:
- An explanation about the different components of your project and how they will all integrate together
- Technical progress you've made so far
- Challenges you're facing and solving in your future milestones
- What your plan is to complete your project
-->
## Schematic 
![Samvrat Gowda](/docs/assets/FirstMilestone.png)
_This is a schematic made by Tinkercad of my wiring that uses a flex sensor as an input to trigger a buzzer._

## Code
<!--
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 
-->

```c++
const int flexPin = A0;			// Pin connected to voltage divider output
const int buzzer = 9; //buzzer to arduino pin 9
unsigned long oldmillis;
float angle;

// Change these constants according to your project's design
const float VCC = 5;			// voltage at Ardunio 5V line
const float R_DIV = 15000.0;	// resistor used to create a voltage divider
const float flatResistance = 25000.0;	// resistance when flat
const float bendResistance = 100000.0;	// resistance at 90 deg

void setup() {
	Serial.begin(9600);
	pinMode(flexPin, INPUT);
  pinMode(buzzer, OUTPUT); // Set buzzer - pin 9 as an output
}

void loop() { 
  // Read the ADC, and calculate voltage and resistance from it
	int ADCflex = analogRead(flexPin);
	float Vflex = ADCflex * VCC / 1023.0;
	float Rflex = R_DIV * (VCC / Vflex - 1.0);
	Serial.println("Resistance: " + String(Rflex) + " ohms");

	// Use the calculated resistance to estimate the sensor's bend angle:
	float angle = map(Rflex, flatResistance, bendResistance, 0, 90.0);
	Serial.println("Bend: " + String(angle) + " degrees");
	Serial.println();
  delay(1000);
  
  // If the flex sensor is bent 90 degrees alert the user to go up
  if(angle >= 30) {
    tone(buzzer, 1000);
  } else {
    noTone(buzzer);
  }
}
```
# Bill of Materials
<!--
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 
-->

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Arduino Uno Rev3 | Uses sensor and accelerometer data to trigger outputs | $27.60 | <a href="https://store-usa.arduino.cc/products/arduino-uno-rev3?gclid=Cj0KCQjwqs6lBhCxARIsAG8YcDiiYxHKGDsX-mF8TC41sUTODtoox7OmWLwUQ-lk6qYo1u2AEEJZtOwaAqwcEALw_wcB"> Link </a> |
| HC-05 Bluetooth Module | Wireless communication with other devices | $10.39 | <a href="https://www.amazon.com/HiLetgo-Wireless-Bluetooth-Transceiver-Arduino/dp/B071YJG8DR"> Link </a> |
| MPU-6050 Accelerometer and Gyroscope | Measures translational and rotational movement of knee | $3.33 | <a href="https://www.amazon.com/HiLetgo-MPU-6050-Accelerometer-Gyroscope-Converter/dp/B00LP25V1A/ref=asc_df_B00LP25V1A/?tag=hyprod-20&linkCode=df0&hvadid=247487538123&hvpos=&hvnetw=g&hvrand=9404472296515307643&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9061320&hvtargid=pla-407209664611&th=1"> Link </a> |
| Flex Sensor(4.5”)	 | Measures the bend of the knee | $17.95 | <a href="https://www.sparkfun.com/products/8606"> Link </a> |
| Knee Compression Sleeve		 | Holds components in place and attached to the knee | $7.99 | <a href="https://www.amazon.com/Compression-Sleeve-Support-Running-Medium/dp/B0987XN6QH/ref=sr_1_1_sspa?crid=1YPAWDY72UPC4&keywords=knee%2Bsleeve&qid=1685066084&sprefix=knee%2Bsleev%2Caps%2C182&sr=8-1-spons&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUEzRjkzWEExWDZSMEEwJmVuY3J5cHRlZElkPUEwMDE2MDE5NjNVMTlUWTBNNUkzJmVuY3J5cHRlZEFkSWQ9QTAwNjE5MDYzTDhJTE1IWFZaRUNVJndpZGdldE5hbWU9c3BfYXRmJmFjdGlvbj1jbGlja1JlZGlyZWN0JmRvTm90TG9nQ2xpY2s9dHJ1ZQ&th=1"> Link </a> |
| 5000 mAh Power Bank (10cm x 3 cm)		 | Powers all the components | $17.99 | <a href="https://www.amazon.com/Anker-PowerCore-Ultra-Compact-High-Speed-Technology/dp/B01CU1EC6Y/ref=asc_df_B01CU1EC6Y/?tag=hyprod-20&linkCode=df0&hvadid=312111908612&hvpos=&hvnetw=g&hvrand=17851029625711693156&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9061320&hvtargid=pla-523807968135&psc=1"> Link </a> |
| Neoprene fabric (2” x 6”)		 | Attaches flex sensor to knee sleeve | $8.49 | <a href="https://www.amazon.com/lychee-Neoprene-Waterproof-Wetsuit-Stretch/dp/B07MCC3968/ref=sr_1_2?crid=2PLYALBAJAEP3&keywords=neoprene+fabric&qid=1689571945&sprefix=neoprene+fabri%2Caps%2C172&sr=8-2"> Link </a> |
| Sewing Kit	 | Sew all components onto knee sleeve | $6.99 | <a href="https://www.amazon.com/Coquimbo-Traveler-Beginner-Emergency-Organizer/dp/B01G3LOLD6/ref=sr_1_7?crid=27CESEBIVTX3Z&keywords=sewing%2Bkit&qid=1689572065&sprefix=sewing%2Bk%2Caps%2C192&sr=8-7&th=1"> Link </a> |
| Jumper Wires	 | Wires together electrical components | $2.33 | <a href="https://www.amazon.com/Elegoo-EL-CP-004-Multicolored-Breadboard-arduino/dp/B01EV70C78/ref=sr_1_3?crid=1GJIWX8C47LE6&keywords=jumper+wires&qid=1689572180&sprefix=jumper+wire%2Caps%2C200&sr=8-3"> Link </a> |
| USB Cable for Arduino	 | Connects power source to arduino | $7.00 | <a href="https://www.amazon.com/USB-2-0-Cable-Type-M000006/dp/B013EOQUAW/ref=sr_1_4?keywords=arduino+uno+cable&qid=1689572328&sr=8-4"> Link </a> |
| PCB Board	 | What the item is used for | $3.43 | <a href="https://www.amazon.com/ELEGOO-Prototype-Soldering-Compatible-Arduino/dp/B072Z7Y19F/ref=sr_1_12_sspa?crid=3OPU9MWHCVIKU&keywords=perf+board&qid=1689572424&sprefix=perf+boar%2Caps%2C174&sr=8-12-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9tdGY&psc=1"> Link </a> |
| Piezzo Buzzer	 | Buzzer goes off when improper form is detected | $1.33 | <a href="https://www.amazon.com/Stemedu-SFM-20B-Electric-DC3-24V-Continuous/dp/B0BFHBXKND/ref=sr_1_4?crid=61ZO1XM97XPB&keywords=piezo+buzzer&qid=1689572505&sprefix=piezo+buz%2Caps%2C236&sr=8-4"> Link </a> |
| 10K Ohm Resistor	 | Helps measure flex sensor resistance | $0.55 | <a href="https://www.amazon.com/EDGELEC-Resistor-Tolerance-Resistance-Optional/dp/B07HDGX5LM/ref=sr_1_2_sspa?crid=1QSGTNOJ9ZED&keywords=10k%2Bresistor&qid=1689572640&sprefix=10k%2Bresistor%2Caps%2C173&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| 100 Ohm Resistor	 | Regulates voltage to buzzer | $0.55 | <a href="https://www.amazon.com/EDGELEC-Resistor-Tolerance-Multiple-Resistance/dp/B07QKDSCSM/ref=sr_1_2_sspa?crid=1UNXWP9K5DL4&keywords=100+resistor&qid=1689572707&sprefix=100+resistor%2Caps%2C187&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |

# Arduino Starter Project

<iframe width="560" height="315" src="https://www.youtube.com/embed/_acy2DqKVyI?si=hsduXBcM8xsnrQ3L" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


I chose the Arduino starter project to prepare me for the knee rehabilitation device. It takes a button as the input and the two outputs are the green LED and the red LED. When the button is pressed, the red LED flashes and the green LED flashes when it is unpressed. It uses resistors and jumper wires in order the components to work and make up the final result. The pullup resistor allows the power pin to sense whether the button is pressed or unpressed which will determine the path of the current. This ultimately results in one LED flashing and the other not flashing.

It is necessary to use a voltage divider to get the LEDs to blink. A voltage divider involves the use of resistors to reduce the voltage in the circuit. The voltage divider is important in the starter project to regulate the amount of voltage the LED is recieving so it does not burn out. The voltage after a voltage divider is given by V<sub>out</sub> = V<sub>in</sub> * R2/(R1 + R2).


## How a Pullup Resistor Works (https://en.wikipedia.org/wiki/Pull-up_resistor)

![Samvrat Gowda](/docs/assets/Pullup_Resistor.png)

## Code for Arduino Starter
```c++
int buttonPin = 2;     // the number of the pushbutton pin
int greenLedPin = 4;   // the number of the green LED pin
int redLedPin = 7;    // the number of the red LED pin
int buttonState = 0;   // variable for reading the button status

void setup() {
  pinMode(greenLedPin, OUTPUT);  // the leds are the output
  pinMode(redLedPin, OUTPUT); 
  pinMode(buttonPin, INPUT); // button is the input
}

void loop() {
  // read the state of the button pin
  buttonState = digitalRead(buttonPin);

  // when the button is pressed, the pin is HIGH
  // our condition is checking if our state is equal to HIGH
  if (buttonState == HIGH) {
    // action 1 is to turn the led on
    digitalWrite(greenLedPin, HIGH);  // turn the LED on (HIGH is the voltage level)
    delay(100);                      // wait for a second
    digitalWrite(greenLedPin, LOW);   // turn the LED off by making the voltage LOW
    delay(100);

// when the state is not HIGH, it must be LOW. 
// in this case we turn the led off.
  } else {
    // action 2 is to turn the led off
    digitalWrite(redLedPin, HIGH);  // turn the LED on (HIGH is the voltage level)
    delay(100);                      // wait for a second
    digitalWrite(redLedPin, LOW);   // turn the LED off by making the voltage LOW
    delay(100);
  }
}


```

<!--# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.

-->
