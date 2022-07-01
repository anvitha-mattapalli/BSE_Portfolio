---
permalink: /
title: Obstacle Avoiding Robot!
---
# Introduction
I designed a 4 wheel autonomous Arduino robot that can detect and maneuver around obstacles using an ultrasonic sensor. From this base project, I implemented voice control via HC-05 Bluetooth module and edge-avoiding characteristicsc with an infrared sensor.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Anvitha M. | Monta Vista High School | Computer Science | Incoming Junior

![Headstone Image](/robot.png)
-------------------------------------------------------------------------------------------  
# Third Milestone
My third milestone is completing the robot! I attached the new motor to the wheel and soldered the wires in place. I programmed the robot to turn either right or left when confronted with an obstacle depending on which direction has the farther obstacle. To do this, I connected a servo to the ultrasonic sensor. When the robot stops, the servo will cause the ultrasonic sensor to turn left and right and determine the distance between itself and the nearest obstacle for each direction. The robot will turn in that direction and then continue to move forward until the next obstacle is faced.

[![Third Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1612573869/video_to_markdown/images/youtube--F7M7imOVGug-c05b58ac6eb4c4700831b2b3070cd403.jpg )](https://www.youtube.com/watch?v=F7M7imOVGug&feature=emb_logo "Final Milestone"){:target="_blank" rel="noopener"}
-------------------------------------------------------------------------------------------
# Second Milestone
My second milestone is assembling the robot as well as programming it to move forward/backward and stop if an obstacle is within 10 inches. I connected the back motors to the front motors using wires and connected the front motors to the motor controller. The motor controller is attached to a battery pack and the Arduino is attached to a 9V battery so the robot does not depend on a laptop as a power source. I realized that one of the wheels was not functioning because the motor attached to the wheel had a broken copper wire. To solve this issue, I will attach a new motor.

[![Second Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1655936928/video_to_markdown/images/youtube--0JBKb5Npano-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/0JBKb5Npano "Second Milestone"){:target="_blank" rel="noopener"}
-------------------------------------------------------------------------------------------
# First Milestone
  

My first milestone is plugging in and connecting the ultrasonic sensor. The ultrasonic sensor reports the distance between itself and the nearest obstacle by utilizing how quickly sound waves are inputted and outputted. I inserted the ultrasonic sensor into the breadboard and connected the male-to-male jumper wires into the Arduino. At first, the ultrasonic sensor was unresponsive, which I fixed through calling the delay() method between inputting and outputting the audio.

[![First Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1655485582/video_to_markdown/images/youtube--KcmZJILX97M-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=KcmZJILX97M "First Milestone")
{:target="_blank" rel = "noopener"}
-------------------------------------------------------------------------------------------
# Reflection
Through BlueStamp Engineering, I was able to learn more about robotics and create the obstacle avoiding robot. Sometimes, I had to troubleshoot, but I was able to persevere, understand the root cause of the issues, and solve them. I thought like an engineer -- brainstorming new solutions and going one step at a time -- in order to finish and add modifications to this project. In the future, I will apply both my passion and my acquired knowledge to add more modifications and create new projects!

-------------------------------------------------------------------------------------------
# Materials
* Arduino UNO
* L298N Motor Controller
* Smart Car Chassis Kit
  * 4 DC motors and wheels
* HC-SR04 Ultrasonic Sensor
* SG90 Micro Servo
* HC-05 Bluetooth Module
* HW201 Infrared Sensor
* Piezo Buzzer
* Jumper Wires
  * Male-to-Male
  * Male-to-Female
* Battery Pack
* 9V Battery

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
    move = false; /** Prevents the autonomous characteristics of the robot to override the user's "stop" command */
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
