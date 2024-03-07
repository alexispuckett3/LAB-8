# Lab 8 Report: App Development
* Alexis Puckett
* Hannah Markwell

March 7, 2024


## Project Summary:


## Design/Methods:
For this lab we needed the following:
* A computer running Arduino IDE and Google Chrome: MacBook Air
* A smart phone running MIT AI2 Companion App on Android OS: Android
* RedBoard, ultrasonic sensor, two motors, motor driver, and a battery pack: SparkFun Inventor's Kit
* A cable/ adaptor to connect the smart phone to the RedBoard

**Part One: Assemble and test the robot**

We started by creating a robot using two motors, a motor driver, an ultrasonic sensor, a RedBoard, a common breadboard, a battery pack, and two wheels. Create the following circuit to drive the two motors with the motor driver and add the ultrasonic sensor to measure the distance between the robot and outside objects. Do not include the switch. 

<p align="center">
  <img src="https://github.com/hrma240/Lab-8/blob/main/Screenshot%202024-03-06%20at%201.11.25%20PM.png">
</p>
(SparkFun, Creative Commons Attribution ShareALike 3.0, https://learn.sparkfun.com/tutorials/sparkfun-inventors-kit-experiment-guide---v41/circuit-5c-autonomous-robot)

Next, use the adhesive in the SparkFun Kit to attach the motors to the bottom edges of the breadboard and RedBoard baseplate. Add the battery pack to the bottom of the baseplate as well and connect the wheels to the motors for the robot to act as a car. Make sure that the battery pack is installed so that the binder clip will not touch the surface the robot is on when we attach the binder clip to it. Also, make sure the ultrasonic sensor is on the front of the car so that it can measure the distance between the car and other objects accurately. The final set up should look like the following.

<p align="center">
  <img src="https://github.com/hrma240/Lab-8/blob/main/Screenshot%202024-03-06%20at%201.17.25%20PM.png">
</p>
(SparkFun, Creative Commons Attribution ShareALike 3.0, https://learn.sparkfun.com/tutorials/sparkfun-inventors-kit-experiment-guide---v41/circuit-5c-autonomous-robot)

Do not connect the battery pack to the RedBoard, but connect the RedBoard to the computer running Arduino IDE. Use the following code to make the robot move front, back, right, and left at various input speeds.




**Part Two: Develop the app**

First, we needed to create an account with the MIT App Inventor 2 Web-based tool. Create a new project and add a button in the Designer side that will be used to initialize the serial connections. Then add four buttons that will control the direction and speed that the robot will move in, along with a stop button to stop the movement of the robot. We named each button according to its function and our home screen looked like the following image.  

<p align="center">
  <img src="https://github.com/hrma240/Lab-8/blob/main/Screenshot%202024-02-28%20at%203.14.30%20PM.png">
</p>

Still in the Designer side, add a serial component from the connectivity section. Then switch to the Blocks side and create the following blocks. 

<p align="center">
  <img src="https://github.com/hrma240/Lab-8/blob/main/Screenshot%202024-02-28%20at%203.18.50%20PM.png">
</p>

In our app, Button1 sets up the connection between the robot and Arduino IDE, and Buttons 2-6 change the direction and speed of the robot. When clicking Buttons 2-6, the command seen in data " " is sent to the RedBoard and the motors move based on this command following what the code says on Arduino IDE. The code used in this part of the lab is the same code as used in Part One. 

Finally, connect the battery pack to power the RedBoard and connect the smart phone to the RedBoard using an adaptor and the regular cable for the RedBoard. Click the build app button on the Web-based tool and click Android App (.apk). A QR code will pop up for the Android to scan and run the MIT AI2 Companion app. Run our app by clicking the StartUp button to begin the serial connection and test out the other buttons to change the speed and direction of the robot. 

We were unable to complete the third part of the lab due to recent updates to Android OS that change how we connect Bluetooth to the robot using an HC-05 Bluetooth UART Module. 

## Results:

**Part One:**



**Part Two:**

## Conclusions:
