# Knee Rehabilitation Device
The knee rehabilitation device is useful for ensuring the safety of the knee and preventing irritation by correcting a person's form when doing certain exercises. If incorrect form is detected, it emits a high pitched sound and vibration to notify the person that they are doing the exercise with incorrect form. An Arduino setup is necessary as the sensor is needed to detect dangerous movements to have the output be the vibrations.

<!--You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:-->

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Samvrat G | American High School | Computer Science | Incoming Senior


![Samvrat Gowda](/docs/assets/Samvrat_G.jpg)
  

# Final Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/iCLwPWmPwnw?si=bR2IIqH8gOqL9h_V" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For this milestone, I transferred the wiring from my breadboard to my Adafruit Protoshield V6 by changing the jumper wires to solid core wires and soldering them onto the respective pins. I sewed my Arduino and accelerometer onto my knee sleeve. Since the threads are a little thin, it needs a lot of reinforcement so it does not break. I also got my Bluetooth module to work after it was not working for my first milestone. The Bluetooth module is hooked to the 5V input, ground, RX, and TX. The RX receives data and sends it to the TX pin.  

One of my biggest challenges at BSE was calibrating my accelerometer for the first time since the instructions were not updated for the current Arduino IDE version. Another challenge was that I wasted a few days trying to set up my Bluetooth module when in reality, nothing was wrong and it was the inconsistency of the Bluetooth module which made it not work at times. Another challenge was sewing my Arduino to my knee sleeve due to the threads being too thin so I had to use a lot of thread to reinforce it.

Entering BlueStamp, I knew very little about circuits and hardware in general as I only focused on coding in the past. During my time at BlueStamp I learned how circuits work along with many components of circuits like voltage dividers. I became familiar with the Arduino and each of its pins including SCL, SDA, RX, and TX. I also learned many features on GitHub to make my portfolio look more refined. I learned how to code on the Arduino IDE which uses c++, similar to Java in its syntax and features.

In the future, I hope to have more exposure to mechanical projects which incorporate coding and even artificial intelligence. I would also like to work on something more dynamic like a type of robot that can perform certain tasks. I would prefer it be an autonomous robot so I can incorporate artificial intelligence allowing a vast amount of tasks being open to perform by the robot. A humanoid robot would be ideal to perform basic tasks that humans don't like to perform.

## Code
<!--
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 
-->

```c++
// SPDX-FileCopyrightText: 2020 Kattni Rembor for Adafruit Industries
//
// SPDX-License-Identifier: MIT

// Basic demo for accelerometer, gyro, and magnetometer readings
// from the following Adafruit ST Sensor combo boards:
// * LSM6DSOX + LIS3MDL FeatherWing : https://www.adafruit.com/product/4565
// * ISM330DHCX + LIS3MDL FeatherWing https://www.adafruit.com/product/4569
// * LSM6DSOX + LIS3MDL Breakout : https://www.adafruit.com/product/4517
// * LSM6DS33 + LIS3MDL Breakout Lhttps://www.adafruit.com/product/4485

// #include <Adafruit_LSM6DSOX.h>
// Adafruit_LSM6DSOX lsm6ds;

// To use with the LSM6DS33+LIS3MDL breakout, uncomment these two lines
// and comment out the lines referring to the LSM6DSOX above
//#include <Adafruit_LSM6DS33.h>
//Adafruit_LSM6DS33 lsm6ds;

// To use with the ISM330DHCX+LIS3MDL Feather Wing, uncomment these two lines
// and comment out the lines referring to the LSM6DSOX above
//#include <Adafruit_ISM330DHCX.h>
//Adafruit_ISM330DHCX lsm6ds;

// To use with the LSM6D3TR-C+LIS3MDL breakout, uncomment these two lines
// and comment out the lines referring to the LSM6DSOX above
#include <Adafruit_LSM6DS3TRC.h>
Adafruit_LSM6DS3TRC lsm6ds;

#include <Adafruit_LIS3MDL.h>
Adafruit_LIS3MDL lis3mdl;

sensors_event_t accel, gyro, mag, temp;

int buzzer = 9;

void setup(void) {
  Serial.begin(115200);
  while (!Serial)
    delay(10); // will pause Zero, Leonardo, etc until serial console opens

  Serial.println("Adafruit LSM6DS+LIS3MDL test!");

  bool lsm6ds_success, lis3mdl_success;

  // hardware I2C mode, can pass in address & alt Wire

  lsm6ds_success = lsm6ds.begin_I2C();
  lis3mdl_success = lis3mdl.begin_I2C();

  if (!lsm6ds_success){
    Serial.println("Failed to find LSM6DS chip");
  }
  if (!lis3mdl_success){
    Serial.println("Failed to find LIS3MDL chip");
  }
  if (!(lsm6ds_success && lis3mdl_success)) {
    while (1) {
      delay(10);
    }
  }

  Serial.println("LSM6DS and LIS3MDL Found!");

  // lsm6ds.setAccelRange(LSM6DS_ACCEL_RANGE_2_G);
  Serial.print("Accelerometer range set to: ");
  switch (lsm6ds.getAccelRange()) {
  case LSM6DS_ACCEL_RANGE_2_G:
    Serial.println("+-2G");
    break;
  case LSM6DS_ACCEL_RANGE_4_G:
    Serial.println("+-4G");
    break;
  case LSM6DS_ACCEL_RANGE_8_G:
    Serial.println("+-8G");
    break;
  case LSM6DS_ACCEL_RANGE_16_G:
    Serial.println("+-16G");
    break;
  }

  // lsm6ds.setAccelDataRate(LSM6DS_RATE_12_5_HZ);
  Serial.print("Accelerometer data rate set to: ");
  switch (lsm6ds.getAccelDataRate()) {
  case LSM6DS_RATE_SHUTDOWN:
    Serial.println("0 Hz");
    break;
  case LSM6DS_RATE_12_5_HZ:
    Serial.println("12.5 Hz");
    break;
  case LSM6DS_RATE_26_HZ:
    Serial.println("26 Hz");
    break;
  case LSM6DS_RATE_52_HZ:
    Serial.println("52 Hz");
    break;
  case LSM6DS_RATE_104_HZ:
    Serial.println("104 Hz");
    break;
  case LSM6DS_RATE_208_HZ:
    Serial.println("208 Hz");
    break;
  case LSM6DS_RATE_416_HZ:
    Serial.println("416 Hz");
    break;
  case LSM6DS_RATE_833_HZ:
    Serial.println("833 Hz");
    break;
  case LSM6DS_RATE_1_66K_HZ:
    Serial.println("1.66 KHz");
    break;
  case LSM6DS_RATE_3_33K_HZ:
    Serial.println("3.33 KHz");
    break;
  case LSM6DS_RATE_6_66K_HZ:
    Serial.println("6.66 KHz");
    break;
  }

  // lsm6ds.setGyroRange(LSM6DS_GYRO_RANGE_250_DPS );
  Serial.print("Gyro range set to: ");
  switch (lsm6ds.getGyroRange()) {
  case LSM6DS_GYRO_RANGE_125_DPS:
    Serial.println("125 degrees/s");
    break;
  case LSM6DS_GYRO_RANGE_250_DPS:
    Serial.println("250 degrees/s");
    break;
  case LSM6DS_GYRO_RANGE_500_DPS:
    Serial.println("500 degrees/s");
    break;
  case LSM6DS_GYRO_RANGE_1000_DPS:
    Serial.println("1000 degrees/s");
    break;
  case LSM6DS_GYRO_RANGE_2000_DPS:
    Serial.println("2000 degrees/s");
    break;
  case ISM330DHCX_GYRO_RANGE_4000_DPS:
    Serial.println("4000 degrees/s");
    break;
  }
  // lsm6ds.setGyroDataRate(LSM6DS_RATE_12_5_HZ);
  Serial.print("Gyro data rate set to: ");
  switch (lsm6ds.getGyroDataRate()) {
  case LSM6DS_RATE_SHUTDOWN:
    Serial.println("0 Hz");
    break;
  case LSM6DS_RATE_12_5_HZ:
    Serial.println("12.5 Hz");
    break;
  case LSM6DS_RATE_26_HZ:
    Serial.println("26 Hz");
    break;
  case LSM6DS_RATE_52_HZ:
    Serial.println("52 Hz");
    break;
  case LSM6DS_RATE_104_HZ:
    Serial.println("104 Hz");
    break;
  case LSM6DS_RATE_208_HZ:
    Serial.println("208 Hz");
    break;
  case LSM6DS_RATE_416_HZ:
    Serial.println("416 Hz");
    break;
  case LSM6DS_RATE_833_HZ:
    Serial.println("833 Hz");
    break;
  case LSM6DS_RATE_1_66K_HZ:
    Serial.println("1.66 KHz");
    break;
  case LSM6DS_RATE_3_33K_HZ:
    Serial.println("3.33 KHz");
    break;
  case LSM6DS_RATE_6_66K_HZ:
    Serial.println("6.66 KHz");
    break;
  }

  lis3mdl.setDataRate(LIS3MDL_DATARATE_155_HZ);
  // You can check the datarate by looking at the frequency of the DRDY pin
  Serial.print("Magnetometer data rate set to: ");
  switch (lis3mdl.getDataRate()) {
    case LIS3MDL_DATARATE_0_625_HZ: Serial.println("0.625 Hz"); break;
    case LIS3MDL_DATARATE_1_25_HZ: Serial.println("1.25 Hz"); break;
    case LIS3MDL_DATARATE_2_5_HZ: Serial.println("2.5 Hz"); break;
    case LIS3MDL_DATARATE_5_HZ: Serial.println("5 Hz"); break;
    case LIS3MDL_DATARATE_10_HZ: Serial.println("10 Hz"); break;
    case LIS3MDL_DATARATE_20_HZ: Serial.println("20 Hz"); break;
    case LIS3MDL_DATARATE_40_HZ: Serial.println("40 Hz"); break;
    case LIS3MDL_DATARATE_80_HZ: Serial.println("80 Hz"); break;
    case LIS3MDL_DATARATE_155_HZ: Serial.println("155 Hz"); break;
    case LIS3MDL_DATARATE_300_HZ: Serial.println("300 Hz"); break;
    case LIS3MDL_DATARATE_560_HZ: Serial.println("560 Hz"); break;
    case LIS3MDL_DATARATE_1000_HZ: Serial.println("1000 Hz"); break;
  }

  lis3mdl.setRange(LIS3MDL_RANGE_4_GAUSS);
  Serial.print("Range set to: ");
  switch (lis3mdl.getRange()) {
    case LIS3MDL_RANGE_4_GAUSS: Serial.println("+-4 gauss"); break;
    case LIS3MDL_RANGE_8_GAUSS: Serial.println("+-8 gauss"); break;
    case LIS3MDL_RANGE_12_GAUSS: Serial.println("+-12 gauss"); break;
    case LIS3MDL_RANGE_16_GAUSS: Serial.println("+-16 gauss"); break;
  }

  lis3mdl.setPerformanceMode(LIS3MDL_MEDIUMMODE);
  Serial.print("Magnetometer performance mode set to: ");
  switch (lis3mdl.getPerformanceMode()) {
    case LIS3MDL_LOWPOWERMODE: Serial.println("Low"); break;
    case LIS3MDL_MEDIUMMODE: Serial.println("Medium"); break;
    case LIS3MDL_HIGHMODE: Serial.println("High"); break;
    case LIS3MDL_ULTRAHIGHMODE: Serial.println("Ultra-High"); break;
  }

  lis3mdl.setOperationMode(LIS3MDL_CONTINUOUSMODE);
  Serial.print("Magnetometer operation mode set to: ");
  // Single shot mode will complete conversion and go into power down
  switch (lis3mdl.getOperationMode()) {
    case LIS3MDL_CONTINUOUSMODE: Serial.println("Continuous"); break;
    case LIS3MDL_SINGLEMODE: Serial.println("Single mode"); break;
    case LIS3MDL_POWERDOWNMODE: Serial.println("Power-down"); break;
  }

  lis3mdl.setIntThreshold(500);
  lis3mdl.configInterrupt(false, false, true, // enable z axis
                          true, // polarity
                          false, // don't latch
                          true); // enabled!

}
void loop() {
  
  sensors_event_t accel, gyro, mag, temp;

  //  /* Get new normalized sensor events */
  lsm6ds.getEvent(&accel, &gyro, &temp);
  lis3mdl.getEvent(&mag);

  /* Display the results (acceleration is measured in m/s^2) */
  Serial.print("\t\tAccel X: ");
  Serial.print(accel.acceleration.x, 4);
  Serial.print(" \tY: ");
  Serial.print(accel.acceleration.y, 4);
  Serial.print(" \tZ: ");
  Serial.print(accel.acceleration.z, 4);
  Serial.println(" \tm/s^2 ");

  /* Display the results (rotation is measured in rad/s) */
  Serial.print("\t\tGyro  X: ");
  Serial.print(gyro.gyro.x, 4);
  Serial.print(" \tY: ");
  Serial.print(gyro.gyro.y, 4);
  Serial.print(" \tZ: ");
  Serial.print(gyro.gyro.z, 4);
  Serial.println(" \tradians/s ");  
  Serial.println();

  Serial.print("\t\tTemp   :\t\t\t\t\t");
  Serial.print(temp.temperature);
  Serial.println(" \tdeg C");
  Serial.println();

  
  if (accel.acceleration.z > 9 || accel.acceleration.y > 3 && gyro.gyro.x > 0.5) {    
      tone(buzzer, 100);
  } 
  
  
  else {
    noTone(buzzer);
  }
  
  delay(10);

}

```

# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/4RHNaViHJuQ?si=TuoRljhpImbT1dVw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

This milestone signals the user that the knee is moving inward during a squat. The inward movement of the knee is undesirable during the squat as it puts pressure on the knee and causes tension in the hip. In order to signal the user that this is happening, I used an accelerometer to signal that the knee is moving inwards as the Y-axis reading of the accelerometer is in the direction perpendicular to the leg.

The accelerometer is hooked to power, ground, SCL, and SDA. SCL is the serial clock and SDA is serial data and they use Inter-Integrated Circuit (I2C) protocol which allows devices to communicate over short distances. SCL carries a clock signal which is a current that alternates between high and low to maintain voltage. SDA is the line that allows for the transfer of data for communication

One challenging part was the setup of the accelerometer as the example code on the setup instructions for my accelerometer may have not been updated. I had to find an example code on the Arduino IDE which was meant for a different accelerometer but still worked for my accelerometer. Another challenge was the inconsistency of the accelerometer which made it difficult to find values that signal that the knee is moving inwards. The only way to solve this was through trial and error and I eventually found the right values.

The next step for the project is to get my Bluetooth module to work, transfer my wiring from my breadboard to the Adafruit ProtoShield, and sew all the components to the knee sleeve.

## Code
<!--
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 
-->

```c++
// SPDX-FileCopyrightText: 2020 Kattni Rembor for Adafruit Industries
//
// SPDX-License-Identifier: MIT

// Basic demo for accelerometer, gyro, and magnetometer readings
// from the following Adafruit ST Sensor combo boards:
// * LSM6DSOX + LIS3MDL FeatherWing : https://www.adafruit.com/product/4565
// * ISM330DHCX + LIS3MDL FeatherWing https://www.adafruit.com/product/4569
// * LSM6DSOX + LIS3MDL Breakout : https://www.adafruit.com/product/4517
// * LSM6DS33 + LIS3MDL Breakout Lhttps://www.adafruit.com/product/4485

// #include <Adafruit_LSM6DSOX.h>
// Adafruit_LSM6DSOX lsm6ds;

// To use with the LSM6DS33+LIS3MDL breakout, uncomment these two lines
// and comment out the lines referring to the LSM6DSOX above
//#include <Adafruit_LSM6DS33.h>
//Adafruit_LSM6DS33 lsm6ds;

// To use with the ISM330DHCX+LIS3MDL Feather Wing, uncomment these two lines
// and comment out the lines referring to the LSM6DSOX above
//#include <Adafruit_ISM330DHCX.h>
//Adafruit_ISM330DHCX lsm6ds;

// To use with the LSM6D3TR-C+LIS3MDL breakout, uncomment these two lines
// and comment out the lines referring to the LSM6DSOX above
#include <Adafruit_LSM6DS3TRC.h>
Adafruit_LSM6DS3TRC lsm6ds;

#include <Adafruit_LIS3MDL.h>
Adafruit_LIS3MDL lis3mdl;

sensors_event_t accel, gyro, mag, temp;

int buzzer = 9;

void setup(void) {
  Serial.begin(115200);
  while (!Serial)
    delay(10); // will pause Zero, Leonardo, etc until serial console opens

  Serial.println("Adafruit LSM6DS+LIS3MDL test!");

  bool lsm6ds_success, lis3mdl_success;

  // hardware I2C mode, can pass in address & alt Wire

  lsm6ds_success = lsm6ds.begin_I2C();
  lis3mdl_success = lis3mdl.begin_I2C();

  if (!lsm6ds_success){
    Serial.println("Failed to find LSM6DS chip");
  }
  if (!lis3mdl_success){
    Serial.println("Failed to find LIS3MDL chip");
  }
  if (!(lsm6ds_success && lis3mdl_success)) {
    while (1) {
      delay(10);
    }
  }

  Serial.println("LSM6DS and LIS3MDL Found!");

  // lsm6ds.setAccelRange(LSM6DS_ACCEL_RANGE_2_G);
  Serial.print("Accelerometer range set to: ");
  switch (lsm6ds.getAccelRange()) {
  case LSM6DS_ACCEL_RANGE_2_G:
    Serial.println("+-2G");
    break;
  case LSM6DS_ACCEL_RANGE_4_G:
    Serial.println("+-4G");
    break;
  case LSM6DS_ACCEL_RANGE_8_G:
    Serial.println("+-8G");
    break;
  case LSM6DS_ACCEL_RANGE_16_G:
    Serial.println("+-16G");
    break;
  }

  // lsm6ds.setAccelDataRate(LSM6DS_RATE_12_5_HZ);
  Serial.print("Accelerometer data rate set to: ");
  switch (lsm6ds.getAccelDataRate()) {
  case LSM6DS_RATE_SHUTDOWN:
    Serial.println("0 Hz");
    break;
  case LSM6DS_RATE_12_5_HZ:
    Serial.println("12.5 Hz");
    break;
  case LSM6DS_RATE_26_HZ:
    Serial.println("26 Hz");
    break;
  case LSM6DS_RATE_52_HZ:
    Serial.println("52 Hz");
    break;
  case LSM6DS_RATE_104_HZ:
    Serial.println("104 Hz");
    break;
  case LSM6DS_RATE_208_HZ:
    Serial.println("208 Hz");
    break;
  case LSM6DS_RATE_416_HZ:
    Serial.println("416 Hz");
    break;
  case LSM6DS_RATE_833_HZ:
    Serial.println("833 Hz");
    break;
  case LSM6DS_RATE_1_66K_HZ:
    Serial.println("1.66 KHz");
    break;
  case LSM6DS_RATE_3_33K_HZ:
    Serial.println("3.33 KHz");
    break;
  case LSM6DS_RATE_6_66K_HZ:
    Serial.println("6.66 KHz");
    break;
  }

  // lsm6ds.setGyroRange(LSM6DS_GYRO_RANGE_250_DPS );
  Serial.print("Gyro range set to: ");
  switch (lsm6ds.getGyroRange()) {
  case LSM6DS_GYRO_RANGE_125_DPS:
    Serial.println("125 degrees/s");
    break;
  case LSM6DS_GYRO_RANGE_250_DPS:
    Serial.println("250 degrees/s");
    break;
  case LSM6DS_GYRO_RANGE_500_DPS:
    Serial.println("500 degrees/s");
    break;
  case LSM6DS_GYRO_RANGE_1000_DPS:
    Serial.println("1000 degrees/s");
    break;
  case LSM6DS_GYRO_RANGE_2000_DPS:
    Serial.println("2000 degrees/s");
    break;
  case ISM330DHCX_GYRO_RANGE_4000_DPS:
    Serial.println("4000 degrees/s");
    break;
  }
  // lsm6ds.setGyroDataRate(LSM6DS_RATE_12_5_HZ);
  Serial.print("Gyro data rate set to: ");
  switch (lsm6ds.getGyroDataRate()) {
  case LSM6DS_RATE_SHUTDOWN:
    Serial.println("0 Hz");
    break;
  case LSM6DS_RATE_12_5_HZ:
    Serial.println("12.5 Hz");
    break;
  case LSM6DS_RATE_26_HZ:
    Serial.println("26 Hz");
    break;
  case LSM6DS_RATE_52_HZ:
    Serial.println("52 Hz");
    break;
  case LSM6DS_RATE_104_HZ:
    Serial.println("104 Hz");
    break;
  case LSM6DS_RATE_208_HZ:
    Serial.println("208 Hz");
    break;
  case LSM6DS_RATE_416_HZ:
    Serial.println("416 Hz");
    break;
  case LSM6DS_RATE_833_HZ:
    Serial.println("833 Hz");
    break;
  case LSM6DS_RATE_1_66K_HZ:
    Serial.println("1.66 KHz");
    break;
  case LSM6DS_RATE_3_33K_HZ:
    Serial.println("3.33 KHz");
    break;
  case LSM6DS_RATE_6_66K_HZ:
    Serial.println("6.66 KHz");
    break;
  }

  lis3mdl.setDataRate(LIS3MDL_DATARATE_155_HZ);
  // You can check the datarate by looking at the frequency of the DRDY pin
  Serial.print("Magnetometer data rate set to: ");
  switch (lis3mdl.getDataRate()) {
    case LIS3MDL_DATARATE_0_625_HZ: Serial.println("0.625 Hz"); break;
    case LIS3MDL_DATARATE_1_25_HZ: Serial.println("1.25 Hz"); break;
    case LIS3MDL_DATARATE_2_5_HZ: Serial.println("2.5 Hz"); break;
    case LIS3MDL_DATARATE_5_HZ: Serial.println("5 Hz"); break;
    case LIS3MDL_DATARATE_10_HZ: Serial.println("10 Hz"); break;
    case LIS3MDL_DATARATE_20_HZ: Serial.println("20 Hz"); break;
    case LIS3MDL_DATARATE_40_HZ: Serial.println("40 Hz"); break;
    case LIS3MDL_DATARATE_80_HZ: Serial.println("80 Hz"); break;
    case LIS3MDL_DATARATE_155_HZ: Serial.println("155 Hz"); break;
    case LIS3MDL_DATARATE_300_HZ: Serial.println("300 Hz"); break;
    case LIS3MDL_DATARATE_560_HZ: Serial.println("560 Hz"); break;
    case LIS3MDL_DATARATE_1000_HZ: Serial.println("1000 Hz"); break;
  }

  lis3mdl.setRange(LIS3MDL_RANGE_4_GAUSS);
  Serial.print("Range set to: ");
  switch (lis3mdl.getRange()) {
    case LIS3MDL_RANGE_4_GAUSS: Serial.println("+-4 gauss"); break;
    case LIS3MDL_RANGE_8_GAUSS: Serial.println("+-8 gauss"); break;
    case LIS3MDL_RANGE_12_GAUSS: Serial.println("+-12 gauss"); break;
    case LIS3MDL_RANGE_16_GAUSS: Serial.println("+-16 gauss"); break;
  }

  lis3mdl.setPerformanceMode(LIS3MDL_MEDIUMMODE);
  Serial.print("Magnetometer performance mode set to: ");
  switch (lis3mdl.getPerformanceMode()) {
    case LIS3MDL_LOWPOWERMODE: Serial.println("Low"); break;
    case LIS3MDL_MEDIUMMODE: Serial.println("Medium"); break;
    case LIS3MDL_HIGHMODE: Serial.println("High"); break;
    case LIS3MDL_ULTRAHIGHMODE: Serial.println("Ultra-High"); break;
  }

  lis3mdl.setOperationMode(LIS3MDL_CONTINUOUSMODE);
  Serial.print("Magnetometer operation mode set to: ");
  // Single shot mode will complete conversion and go into power down
  switch (lis3mdl.getOperationMode()) {
    case LIS3MDL_CONTINUOUSMODE: Serial.println("Continuous"); break;
    case LIS3MDL_SINGLEMODE: Serial.println("Single mode"); break;
    case LIS3MDL_POWERDOWNMODE: Serial.println("Power-down"); break;
  }

  lis3mdl.setIntThreshold(500);
  lis3mdl.configInterrupt(false, false, true, // enable z axis
                          true, // polarity
                          false, // don't latch
                          true); // enabled!

}
void loop() {
  
  sensors_event_t accel, gyro, mag, temp;

  //  /* Get new normalized sensor events */
  lsm6ds.getEvent(&accel, &gyro, &temp);
  lis3mdl.getEvent(&mag);

  /* Display the results (acceleration is measured in m/s^2) */
  Serial.print("\t\tAccel X: ");
  Serial.print(accel.acceleration.x, 4);
  Serial.print(" \tY: ");
  Serial.print(accel.acceleration.y, 4);
  Serial.print(" \tZ: ");
  Serial.print(accel.acceleration.z, 4);
  Serial.println(" \tm/s^2 ");

  /* Display the results (rotation is measured in rad/s) */
  Serial.print("\t\tGyro  X: ");
  Serial.print(gyro.gyro.x, 4);
  Serial.print(" \tY: ");
  Serial.print(gyro.gyro.y, 4);
  Serial.print(" \tZ: ");
  Serial.print(gyro.gyro.z, 4);
  Serial.println(" \tradians/s ");  
  Serial.println();

  Serial.print("\t\tTemp   :\t\t\t\t\t");
  Serial.print(temp.temperature);
  Serial.println(" \tdeg C");
  Serial.println();


  if (accel.acceleration.y > 1) {
    tone(buzzer, 100);
  }
  if (accel.acceleration.z > 9) {
     tone(buzzer, 100);
  } 
  else {
    noTone(buzzer);
  }
  
  delay(10);

}
```

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/jykKqBEpDyc?si=MvhP0gcF03RH9gwV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

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
_Figure 1: This is a schematic I made using Tinkercad of my wiring that uses a flex sensor as an input to trigger a buzzer._

## Code
<!--
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 
-->

```c++
const int flexPin = A0;			// Pin connected to voltage divider output
const int buzzer = 9; //buzzer to Arduino pin 9
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
## Bill of Materials
<!--
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 
-->

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Arduino Uno Rev3 | Uses sensor and accelerometer data to trigger outputs | $27.60 | <a href="https://store-usa.Arduino.cc/products/Arduino-uno-rev3?gclid=Cj0KCQjwqs6lBhCxARIsAG8YcDiiYxHKGDsX-mF8TC41sUTODtoox7OmWLwUQ-lk6qYo1u2AEEJZtOwaAqwcEALw_wcB"> Link </a> |
| HC-05 Bluetooth Module | Wireless communication with other devices | $10.39 | <a href="https://www.amazon.com/HiLetgo-Wireless-Bluetooth-Transceiver-Arduino/dp/B071YJG8DR"> Link </a> |
| Adafruit LSM6DS3TR-C+LIS3MDL Accelerometer | Measures translational and rotational movement of knee | $3.33 | <a href="https://www.adafruit.com/product/5543?gad_source=1&gclid=Cj0KCQjwhb60BhClARIsABGGtw8-jY7wSMWS0FXfzOjqk8mHnPteYpW1C7iLXs6zfJK6qxsxANZ_MVoaAqhWEALw_wcB"> Link </a> |
| Flex Sensor(4.5”)	 | Measures the bend of the knee | $17.95 | <a href="https://www.sparkfun.com/products/8606"> Link </a> |
| Knee Compression Sleeve		 | Holds components in place and attached to the knee | $7.99 | <a href="https://www.amazon.com/Compression-Sleeve-Support-Running-Medium/dp/B0987XN6QH/ref=sr_1_1_sspa?crid=1YPAWDY72UPC4&keywords=knee%2Bsleeve&qid=1685066084&sprefix=knee%2Bsleev%2Caps%2C182&sr=8-1-spons&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUEzRjkzWEExWDZSMEEwJmVuY3J5cHRlZElkPUEwMDE2MDE5NjNVMTlUWTBNNUkzJmVuY3J5cHRlZEFkSWQ9QTAwNjE5MDYzTDhJTE1IWFZaRUNVJndpZGdldE5hbWU9c3BfYXRmJmFjdGlvbj1jbGlja1JlZGlyZWN0JmRvTm90TG9nQ2xpY2s9dHJ1ZQ&th=1"> Link </a> |
| 5000 mAh Power Bank (10cm x 3 cm)		 | Powers all the components | $17.99 | <a href="https://www.amazon.com/Anker-PowerCore-Ultra-Compact-High-Speed-Technology/dp/B01CU1EC6Y/ref=asc_df_B01CU1EC6Y/?tag=hyprod-20&linkCode=df0&hvadid=312111908612&hvpos=&hvnetw=g&hvrand=17851029625711693156&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9061320&hvtargid=pla-523807968135&psc=1"> Link </a> |
| Neoprene fabric (2” x 6”)		 | Attaches flex sensor to knee sleeve | $8.49 | <a href="https://www.amazon.com/lychee-Neoprene-Waterproof-Wetsuit-Stretch/dp/B07MCC3968/ref=sr_1_2?crid=2PLYALBAJAEP3&keywords=neoprene+fabric&qid=1689571945&sprefix=neoprene+fabri%2Caps%2C172&sr=8-2"> Link </a> |
| Sewing Kit	 | Sew all components onto knee sleeve | $6.99 | <a href="https://www.amazon.com/Coquimbo-Traveler-Beginner-Emergency-Organizer/dp/B01G3LOLD6/ref=sr_1_7?crid=27CESEBIVTX3Z&keywords=sewing%2Bkit&qid=1689572065&sprefix=sewing%2Bk%2Caps%2C192&sr=8-7&th=1"> Link </a> |
| Jumper Wires	 | Wires together electrical components | $2.33 | <a href="https://www.amazon.com/Elegoo-EL-CP-004-Multicolored-Breadboard-Arduino/dp/B01EV70C78/ref=sr_1_3?crid=1GJIWX8C47LE6&keywords=jumper+wires&qid=1689572180&sprefix=jumper+wire%2Caps%2C200&sr=8-3"> Link </a> |
| USB Cable for Arduino	 | Connects power source to Arduino | $7.00 | <a href="https://www.amazon.com/USB-2-0-Cable-Type-M000006/dp/B013EOQUAW/ref=sr_1_4?keywords=Arduino+uno+cable&qid=1689572328&sr=8-4"> Link </a> |
| PCB Board	 | What the item is used for | $3.43 | <a href="https://www.amazon.com/ELEGOO-Prototype-Soldering-Compatible-Arduino/dp/B072Z7Y19F/ref=sr_1_12_sspa?crid=3OPU9MWHCVIKU&keywords=perf+board&qid=1689572424&sprefix=perf+boar%2Caps%2C174&sr=8-12-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9tdGY&psc=1"> Link </a> |
| Piezzo Buzzer	 | Buzzer goes off when improper form is detected | $1.33 | <a href="https://www.amazon.com/Stemedu-SFM-20B-Electric-DC3-24V-Continuous/dp/B0BFHBXKND/ref=sr_1_4?crid=61ZO1XM97XPB&keywords=piezo+buzzer&qid=1689572505&sprefix=piezo+buz%2Caps%2C236&sr=8-4"> Link </a> |
| 10K Ohm Resistor	 | Helps measure flex sensor resistance | $0.55 | <a href="https://www.amazon.com/EDGELEC-Resistor-Tolerance-Resistance-Optional/dp/B07HDGX5LM/ref=sr_1_2_sspa?crid=1QSGTNOJ9ZED&keywords=10k%2Bresistor&qid=1689572640&sprefix=10k%2Bresistor%2Caps%2C173&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| 100 Ohm Resistor	 | Regulates voltage to buzzer | $0.55 | <a href="https://www.amazon.com/EDGELEC-Resistor-Tolerance-Multiple-Resistance/dp/B07QKDSCSM/ref=sr_1_2_sspa?crid=1UNXWP9K5DL4&keywords=100+resistor&qid=1689572707&sprefix=100+resistor%2Caps%2C187&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |

# Arduino Starter Project

<iframe width="560" height="315" src="https://www.youtube.com/embed/_acy2DqKVyI?si=hsduXBcM8xsnrQ3L" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


I chose the Arduino starter project to prepare me for the knee rehabilitation device. It takes a button as the input and the two outputs are the green LED and the red LED. When the button is pressed, the red LED flashes and the green LED flashes when it is unpressed. It uses resistors and jumper wires in order the components to work and make up the final result. The pullup resistor allows the power pin to sense whether the button is pressed or unpressed which will determine the path of the current. This ultimately results in one LED flashing and the other not flashing.

It is necessary to use a voltage divider to get the LEDs to blink. A voltage divider involves the use of resistors to reduce the voltage in the circuit. The voltage divider is important in the starter project to regulate the amount of voltage the LED is recieving so it does not burn out. The voltage after a voltage divider is given by V<sub>out</sub> = V<sub>in</sub> * R2/(R1 + R2).

![Samvrat Gowda](/docs/assets/Pullup_Resistor.png)

_Figure 2: How a Pullup Resistor Works (https://en.wikipedia.org/wiki/Pull-up_resistor)_

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
