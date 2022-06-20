# Obstacle Avoiding Robot
I designed an Arduino robot that can detect and maneuver around obstacles using an ultrasonic sensor. 

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Anvitha Mattapalli | Monta Vista High School | Computer Science | Incoming Junior

![Headstone Image](https://bluestampengineering.com/wp-content/uploads/2016/05/improve.jpg)
  
# Final Milestone
My final milestone is completing the robot! I programmed the robot to turn either right or left when confronted with an obstacle depending on which direction has the farther obstacle. To do this, I connected a Servo to the ultrasonic sensor. When the robot stops, the Servo will cause the ultrasonic sensor to turn left and right and determine the distance between itself and the nearest obstacle for each direction. The robot will turn in that direction and then continue to move forward until the next obstacle is faced.

[![Final Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1612573869/video_to_markdown/images/youtube--F7M7imOVGug-c05b58ac6eb4c4700831b2b3070cd403.jpg )](https://www.youtube.com/watch?v=F7M7imOVGug&feature=emb_logo "Final Milestone"){:target="_blank" rel="noopener"}

# Second Milestone
My second milestone is assembling the robot as well as programming it to move forward/backward and stop if an obstacle is within 10 inches. I connected the back motors to the front motors using wires and connected the front motors to the motor controller. The motor controller is attached to a battery pack and the Arduino is attached to a 9V battery so the robot does not depend on a laptop as a power source. I realized that one of the wheels was not functioning because the motor attached to the wheel had a broken copper wire. To solve this issue, I will attach a new motor.

[![Third Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1612574014/video_to_markdown/images/youtube--y3VAmNlER5Y-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=y3VAmNlER5Y&feature=emb_logo "Second Milestone"){:target="_blank" rel="noopener"}
# First Milestone
  

My first milestone is plugging in and connecting the ultrasonic sensor. The ultrasonic sensor reports the distance between itself and the nearest obstacle by utilizing how quickly sound waves are inputted and outputted. I inserted the ultrasonic sensor into the breadboard and connected the male-to-male jumper wires into the Arduino. At first, the ultrasonic sensor was unresponsive, which I fixed through calling the delay() method between inputting and outputting the audio.

[![First Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1655485582/video_to_markdown/images/youtube--KcmZJILX97M-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=KcmZJILX97M "First Milestone")
{:target="_blank" rel = "noopener"}
