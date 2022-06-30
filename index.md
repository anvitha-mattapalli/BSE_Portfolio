I designed a 4 wheel Arduino robot that can detect and maneuver around obstacles using an ultrasonic sensor. From this base project, I implemented voice control via HC-05 Bluetooth module and edge-avoiding characteristicsc with an infrared sensor.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Anvitha M. | Monta Vista High School | Computer Science | Incoming Junior

![Headstone Image](/robot.png)
  
# Final Milestone
My final milestone is completing the robot! I attached the new motor to the wheel and soldered the wires in place. I programmed the robot to turn either right or left when confronted with an obstacle depending on which direction has the farther obstacle. To do this, I connected a Servo to the ultrasonic sensor. When the robot stops, the Servo will cause the ultrasonic sensor to turn left and right and determine the distance between itself and the nearest obstacle for each direction. The robot will turn in that direction and then continue to move forward until the next obstacle is faced.

[![Final Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1612573869/video_to_markdown/images/youtube--F7M7imOVGug-c05b58ac6eb4c4700831b2b3070cd403.jpg )](https://www.youtube.com/watch?v=F7M7imOVGug&feature=emb_logo "Final Milestone"){:target="_blank" rel="noopener"}

# Second Milestone
My second milestone is assembling the robot as well as programming it to move forward/backward and stop if an obstacle is within 10 inches. I connected the back motors to the front motors using wires and connected the front motors to the motor controller. The motor controller is attached to a battery pack and the Arduino is attached to a 9V battery so the robot does not depend on a laptop as a power source. I realized that one of the wheels was not functioning because the motor attached to the wheel had a broken copper wire. To solve this issue, I will attach a new motor.

[![Second Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1655936928/video_to_markdown/images/youtube--0JBKb5Npano-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/0JBKb5Npano "Second Milestone"){:target="_blank" rel="noopener"}
# First Milestone
  

My first milestone is plugging in and connecting the ultrasonic sensor. The ultrasonic sensor reports the distance between itself and the nearest obstacle by utilizing how quickly sound waves are inputted and outputted. I inserted the ultrasonic sensor into the breadboard and connected the male-to-male jumper wires into the Arduino. At first, the ultrasonic sensor was unresponsive, which I fixed through calling the delay() method between inputting and outputting the audio.

[![First Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1655485582/video_to_markdown/images/youtube--KcmZJILX97M-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=KcmZJILX97M "First Milestone")
{:target="_blank" rel = "noopener"}

# Reflection
Through BlueStamp Engineering, I was able to learn more about robotics and create the obstacle avoiding robot. Sometimes, I had to troubleshoot, but I was able to persevere, understand the root cause of the issues, and solve them. I thought like an engineer -- brainstorming new solutions and going one step at a time -- in order to finish and add modifications to this project. In the future, I will apply both my passion and my acquired knowledge to add more modifications and create new projects!

# Schematic
![Schematic](/schematic.png)

# Source Code
'''
#include <Servo.h>
#include <SoftwareSerial.h>

const int leftMotorFor = 7;
const int leftMotorBack = 6;
const int rightMotorFor = 4;
const int rightMotorBack = 5;

const int trigPin = 9;
const int echoPin = 8;

const int ir = 13;
const int piez = 12;

Servo servo;
String command;
boolean move = true;

void setup()
{
  Serial.begin(9600);
  
  servo.attach(10);
  servo.write(45);
  
  pinMode(rightMotorFor, OUTPUT);
  pinMode(leftMotorFor, OUTPUT);
  pinMode(rightMotorBack, OUTPUT);
  pinMode(leftMotorBack, OUTPUT);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  pinMode(piez, OUTPUT);
  checkStart();
}

void checkStart()
{
  do
  {
    command = "";
    while(Serial.available())
    {
      delay(10);
      char c = Serial.read();
      command += c;
      if(c == '#')
        break;
    }
  } while(command != "*start#");
}

void loop()
{ 
  while(Serial.available())
  {
    delay(10);
    char c = Serial.read();
    command += c;
    if(c == '#')
      break;
  }
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
    move = false;
  }
  delay(1000);
  if(move)
  {
    float val = ultraSonicMeasure();
    Serial.println(val);
    if(val <= 10 && val > 0 || digitalRead(ir) == HIGH)
    {
      tone(piez,200);
      moveStop();
      delay(200);
      noTone(piez);
      moveBackward();
      moveBackward();
      delay(200);
      moveStop();
      delay(200);
      if(lookRight() >= lookLeft()) 
      {
          turnRight();
      }
      else
      { 
          turnLeft();
      }
    }
    moveForward();
  }
  command = "";
}

float ultraSonicMeasure()
{
   float duration, inches, cm;
   pinMode(trigPin, OUTPUT);
   digitalWrite(trigPin, LOW);
   delayMicroseconds(2);
   digitalWrite(trigPin, HIGH);
   delayMicroseconds(10);
   digitalWrite(trigPin, LOW);
   pinMode(echoPin, INPUT);
   duration = pulseIn(echoPin, HIGH);
   inches = duration / 74.0 / 2;
   return inches;
}

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

void moveStop()
{
  digitalWrite(rightMotorFor, LOW);
  digitalWrite(leftMotorFor, LOW);
  
  digitalWrite(rightMotorBack, LOW);
  digitalWrite(leftMotorBack, LOW);
}

void moveBackward()
{
  digitalWrite(leftMotorFor, HIGH);
  digitalWrite(rightMotorFor, HIGH);

  digitalWrite(leftMotorBack, LOW);
  digitalWrite(rightMotorBack, LOW);

  analogWrite(leftMotorFor, 255);
  analogWrite(rightMotorFor, 255);
}

void moveForward()
{
  digitalWrite(leftMotorFor, LOW);
  digitalWrite(rightMotorFor, LOW);

  digitalWrite(leftMotorBack, HIGH);
  digitalWrite(rightMotorBack, HIGH);

  analogWrite(leftMotorBack, 255);
  analogWrite(rightMotorBack, 255);
}

void turnLeft()
{
  digitalWrite(leftMotorFor, HIGH);
  digitalWrite(rightMotorBack, HIGH);
  
  digitalWrite(leftMotorBack, LOW);
  digitalWrite(rightMotorFor, LOW);
}

void turnRight(){
  digitalWrite(leftMotorBack, HIGH);
  digitalWrite(rightMotorFor, HIGH);
  
  digitalWrite(leftMotorFor, LOW);
  digitalWrite(rightMotorBack, LOW);
}
'''
