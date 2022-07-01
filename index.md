---
permalink: /
title: Obstacle Avoiding Robot!
---
# Introduction
I designed a 4 wheel, autonomous Arduino robot that can detect and maneuver around obstacles using an ultrasonic sensor. From this base project, I implemented voice control via HC-05 Bluetooth module and edge-avoiding features with an infrared sensor. I hope to add more modifications in the future and apply this to practical, real-life situations.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Anvitha M. | Monta Vista High School | Computer Science | Incoming Junior

![Headstone Image](/robot.png)
-------------------------------------------------------------------------------------------  
# Final Milestone
My final milestone is adding modifications to improve my obstacle avoiding robot! First, I implemented voice control via the HC-05 Bluetooth module. 
> This HC-05 Bluetooth module is able to connect to any Android device. 

Through the "BT Arduino Voice Control" app on Google Play Store, the user can speak any command, and it will be received by the robot. As of date, the robot is able to stop, turn left, turn right, move forward, and move backward with voice commands. Usually, an Arduino will immediately begin running the code uploaded onto it upon being powered. I added an extra "start" command so the robot will only begin to move once the user says "start" into their devide.

My second modification was edge-avoiding features. The servo allows the ultrasonic sensor to turn left and right, but not up and down. As a result, the robot is unable to determine if it is about to fall off an edge. To fix this, I incorporated an HW201 infrared sensor. 

> The HW201 infrared sensor functions similar to the ultrasonic sensor, emitting and receiving infrared radiation that bounces off of the nearest obstacle. 

I pointed this sensor downwards so the sensor will always report the distance between the robot and the ground. The infrared sensor returns either LOW or HIGH, with LOW representing a low distance and HIGH representing a high distance. If there is a high distance between the robot and the ground, the robot is about to fall off an edge, so the robot moves backward and turns to the left or right depending on which direction has the clearer path.

[![Final Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1656691350/video_to_markdown/images/youtube--11uMgP9Z7bo-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/11uMgP9Z7bo "Final Milestone"){:target="_blank" rel="noopener"}
-------------------------------------------------------------------------------------------
# Third Milestone

My third milestone is completing the obstacle avoiding robot! I attached the new motor to the wheel and soldered all the wires in place. First, I programmed the robot to be able to turn left and right. To turn right, the left two motors are turned on and to turn left, the right two motors are turned on. Next, I coded the robot to be able to decide whether to turn left or right when confronted with an obstacle depending on which direction has the clearer path. To do this, I attached the ultrasonic sensor on top of an SG90 micro servo. 
> An SG90 micro servo is able to rotate a specified number of degrees. 

When the robot stops, the servo will turn 45 degrees to the left and right. In each direction, the ultrasonic sensor returns the distance between the robot an the nearest obstacle. The robot turns in the direction with the farther obstacle and continues moving forward until the next obstacle is reached.

[![Third Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1656691328/video_to_markdown/images/youtube--gs6b2CiMf_4-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/gs6b2CiMf_4 "Third Milestone"){:target="_blank" rel="noopener"}
-------------------------------------------------------------------------------------------
# Second Milestone

My second milestone is assembling the robot as well as programming it to move forward/backward and stop if an obstacle is within 10 inches. I connected the back DC motors to the front DC motors using wires and connected the front motors to the L298N motor driver. 

> The L298N motor driver can control the speed and direction of up to 4 motors.

To do this, the wires have to be attached to a thin copper ring on the DC motor. For one of my motors, this ring was broken, causing the wheel to remain stationary. I will solve this issue by attaching a new motor. The motor driver is then attached to a battery pack, and the Arduino is attached to a 9V battery; as a result, the robot does not depend on a laptop as a power source. 

Certain motors are turned on and off through the digitalWrite() method in order to move forward and backward and to stop. For every iteration, before the moveForward() method is called, the ultrasonic sensor returns the distance between the robot and the nearest obstacle. If the distance is within 10 inches, the robot will stop. For my next milestone, I will add the decision-making feature to turn left or right after stopping at an obstacle.

[![Second Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1656691173/video_to_markdown/images/youtube--BR7kAzuG0cM-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/BR7kAzuG0cM "Second Milestone"){:target="_blank" rel="noopener"}
-------------------------------------------------------------------------------------------
# First Milestone
  
My first milestone is plugging in and connecting the HC-SR04 ultrasonic sensor. The HC-SR04 ultrasonic sensor emits sound waves at a frequency above human hearing through the trig pin. The sound waves bounce off of the nearest obstacle and are received again by the echo pin of the ultrasonic sensor. If the sensor is not properly functioning, call the delay() method in between outputting and inputting the sound. This duration of emitting and receiving soundwaves can be converted into the distance the robot is from the nearest obstacle by using the formula:

> distance = (speed of sound)(time taken)/2

I inserted the ultrasonic sensor into the breadboard and connected the male-to-male jumper wires into the Arduino. Later, I will remove the ultrasonic sensor from the breadboard and use male-to-female jumper wires so the ultrasonic sensor can be in any location.

[![First Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1655485582/video_to_markdown/images/youtube--KcmZJILX97M-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=KcmZJILX97M "First Milestone")
{:target="_blank" rel = "noopener"}
-------------------------------------------------------------------------------------------
# Reflection

Through BlueStamp Engineering, I was able to learn more about robotics and create the obstacle avoiding robot. Sometimes, I had to troubleshoot, but I was able to persevere, understand the root cause of the issues, and think out-of-the-box to solve them. I thought like an engineer -- brainstorming new solutions and going one step at a time -- in order to finish and add modifications to this project. In the future, I will apply both my passion and my acquired knowledge to add more modifications and create new projects!

-------------------------------------------------------------------------------------------
# Bill of Materials
![Materials](/materials.png)

-------------------------------------------------------------------------------------------
# Schematic
![Schematic](/schematic.png)

-------------------------------------------------------------------------------------------
# Source Code
``` java
#include <Servo.h>

const int leftMotorFor = 7; /** The forward left motor is connected to Arduino digital pin 7 */ 
const int leftMotorBack = 6; /** The backward left motor is connected to Arduino digital pin 6 */
const int rightMotorFor = 4; /** The forward right motor is connected to Arduino digital pin 4 */
const int rightMotorBack = 5; /** The backward right motor is connected to Arduino digital pin 5 */

const int trigPin = 9; /** The trig pin of the ultrasonic sensor is connected to Arduino digital pin 9 */
const int echoPin = 8; /** The echo pin of the ultrasonic sensor is connected to Arduino digital pin 8 */

const int ir = 13; /** The infrared sensor is connected to Arduino digital pin 13 */
const int piez = 12; /** The piezo buzzer is connected to Arduino digital pin 12 */

Servo servo; /** The servo */
String command; /** The voice command inputted by the user through an Android device */
boolean move = true; /** Whether the robot should be moving or not */

/** Sets up the hardware, labelling them as output or input */
void setup()
{
  Serial.begin(9600);
  
  servo.attach(10); /** The servo is connected to Arduino digital pin 10 */
  servo.write(45); /** The servo is at 45 degrees, which is straight */
  
  /** The four motors are output */
  pinMode(rightMotorFor, OUTPUT);
  pinMode(leftMotorFor, OUTPUT);
  pinMode(rightMotorBack, OUTPUT);
  pinMode(leftMotorBack, OUTPUT);

  pinMode(trigPin, OUTPUT); /** The trig pin of the ultrasonic sensor emits soundwaves */
  pinMode(echoPin, INPUT); /** The echo pin of the ultrasonic sensor receives soundwaves */

  pinMode(piez, OUTPUT); /** The piez buzzer is an output */
  checkStart();
}

/** The robot will only begin moving once the user says "start" */
void checkStart()
{
  do
  {
    command = "";
    while(Serial.available()) /** The user is speaking */
    {
      delay(10);
      char c = Serial.read();
      command += c; /** Add each character one by one to the command field */
      if(c == '#') /** '#' is the end character, representing that the user has stopped speaking */
        break;
    }
  } while(command != "*start#"); /** Exit the method once the user says "start" */
}

/** 
 * Continuously executes the command inputted by the user. If there is an obstacle within 5 inches, 
 * the buzzer will ring. The robot looks to the right and left and goes in the direction with the clearer path.
 */
void loop()
{ 
  while(Serial.available()) /** The user is speaking */
  {
    delay(10);
    char c = Serial.read();
    command += c; /** Add each character one by one to the command field */
    if(c == '#') /** '#' is the end character, representing that the user has stopped speaking */
      break;
  }
  /** Calls the respective method based on the inputted command */
  if(command == "*forward#")
  {
    moveForward();
    move = true;
  }
  else if(command == "*backward#")
  {
    moveBackward();
    move = true;
  }
  else if(command == "*left#")
  {
    turnLeft();
    move = true;
  }
  else if(command == "*right#")
  {
    turnRight();
    move = true;
  }
  else if(command == "*stop#")
  {
    moveStop();
    move = false; /** Prevents the autonomous characteristics of the robot from overriding the user's "stop" command */
  }
  delay(1000);
  
  if(move)
  {
    float val = ultraSonicMeasure();
    if(val <= 10 && val > 0 || digitalRead(ir) == HIGH) /** If obstacle within 10 inches or on the edge of the table */
    {
      tone(piez,200); /** Piezo buzzers buzzes */
      moveStop();
      delay(200);
      noTone(piez);
      moveBackward(); /** Moves backward so not to run into the obstacle */
      moveBackward();
      delay(200);
      moveStop();
      delay(200);
      /** Determine the clearer path by choosing the larger distance between the robot and the nearest obstacle */
      if(lookRight() >= lookLeft()) 
          turnRight();
      else
          turnLeft();
    }
    moveForward();
  }
  command = ""; /** Reset the command for each iteration */
}

/** Uses the ultrasonic sensor to determine the distance between the robot and the nearest obstacle */
float ultraSonicMeasure()
{
   float duration, inches, cm;
   digitalWrite(trigPin, LOW);
   delayMicroseconds(2);
   digitalWrite(trigPin, HIGH); /** Emits a soundwave through the trig pin */
   delayMicroseconds(10);
   digitalWrite(trigPin, LOW);
   duration = pulseIn(echoPin, HIGH); /** Finds the time taken to receive the soundwave through the echo pin */
   inches = duration / 74.0 / 2; /** Converts the duration into inches using the speed of sound */
   return inches;
}

/** 
 * The servo turns to the right for the ultrasonic sensor to determine the distance between the robot and the 
 * nearest obstacle from the right direction 
 */
float lookRight()
{
  servo.write(0);
  delay(500);
  int dist = ultraSonicMeasure();
  delay(100);
  servo.write(90);
  delay(100);
  return dist;
}

/** 
 * The servo turns to the left for the ultrasonic sensor to determine the distance between the robot and the 
 * nearest obstacle from the left direction 
 */
float lookLeft()
{
  servo.write(90);
  delay(500);
  int dist = ultraSonicMeasure();
  delay(100);
  servo.write(45);
  delay(100);
  return dist;
}

/** Turns all the motors off to stop */
void moveStop()
{
  digitalWrite(rightMotorFor, LOW);
  digitalWrite(leftMotorFor, LOW);
  
  digitalWrite(rightMotorBack, LOW);
  digitalWrite(leftMotorBack, LOW);
}

/** Moves backward at maximum speed */
void moveBackward()
{
  digitalWrite(leftMotorFor, HIGH);
  digitalWrite(rightMotorFor, HIGH);

  digitalWrite(leftMotorBack, LOW);
  digitalWrite(rightMotorBack, LOW);

  analogWrite(leftMotorFor, 255);
  analogWrite(rightMotorFor, 255);
}

/** Moves forward at maximum speed */
void moveForward()
{
  digitalWrite(leftMotorFor, LOW);
  digitalWrite(rightMotorFor, LOW);

  digitalWrite(leftMotorBack, HIGH);
  digitalWrite(rightMotorBack, HIGH);

  analogWrite(leftMotorBack, 255);
  analogWrite(rightMotorBack, 255);
}

/** Turns left by turning alternate motors on */
void turnLeft()
{
  digitalWrite(leftMotorFor, HIGH);
  digitalWrite(rightMotorBack, HIGH);
  
  digitalWrite(leftMotorBack, LOW);
  digitalWrite(rightMotorFor, LOW);
}

/** Turns right by turning alternate motors on */
void turnRight(){
  digitalWrite(leftMotorBack, HIGH);
  digitalWrite(rightMotorFor, HIGH);
  
  digitalWrite(leftMotorFor, LOW);
  digitalWrite(rightMotorBack, LOW);
}
```
