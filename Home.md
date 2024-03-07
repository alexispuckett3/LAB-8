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

```c++
//the right motor will be controlled by the motor A pins on the motor driver
const int AIN1 = 13;           //control pin 1 on the motor driver for the right motor
const int AIN2 = 12;            //control pin 2 on the motor driver for the right motor
const int PWMA = 11;            //speed control pin on the motor driver for the right motor

//the left motor will be controlled by the motor B pins on the motor driver
const int PWMB = 10;           //speed control pin on the motor driver for the left motor
const int BIN2 = 9;           //control pin 2 on the motor driver for the left motor
const int BIN1 = 8;           //control pin 1 on the motor driver for the left motor

const int trigPin = 6;        //trigger pin for distance snesor
const int echoPin = 7;        //echo pin for distance sensor


String botDirection;           //the direction that the robot will drive in (this change which direction the two motors spin in)
String motorSpeedStr;

int motorSpeed;               //speed integer for the motors
float duration, distance;     //duration and distance for the distance sensor

/********************************************************************************/
void setup()
{
  //set the motor control pins as outputs
  pinMode(AIN1, OUTPUT);
  pinMode(AIN2, OUTPUT);
  pinMode(PWMA, OUTPUT);

  pinMode(BIN1, OUTPUT);
  pinMode(BIN2, OUTPUT);
  pinMode(PWMB, OUTPUT);
  
  //set the distance sensor trigger pin as output and the echo pin as input
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);



  Serial.begin(9600);           //begin serial communication with the computer

  //prompt the user to enter a command
  Serial.println("Enter a direction followed by speed.");
  Serial.println("f = forward, b = backward, r = turn right, l = turn left, s = stop");
  Serial.println("Example command: f 50 or s 0");
}
```
This section of code initializes pin connects from the RedBoard to Arduino IDE for the motors and the ultrasonics sensor. It then sets up the motor control pins as outputs, the ultrasonic sensor trigger pin as output, and the echo pin as input. It then prompts the user to type in values that will then drive the robot in a certain direction at a specified speed. 

```c++
void loop()
{
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = (duration*340/10000)/2; // Units are cm
//  Serial.print("Distance: ");
//  Serial.println(distance);
    delayMicroseconds(50);

  if (Serial.available() > 0)                         //if the user has sent a command to the RedBoard
  {
    botDirection = Serial.readStringUntil(' ');       //read the characters in the command until you reach the first space
    motorSpeedStr = Serial.readStringUntil(' ');           //read the characters in the command until you reach the second space
    motorSpeed = motorSpeedStr.toInt();
  }
  if (distance > 20)
  {                                                     //if the switch is in the ON position
    if (botDirection == "f")                         //if the entered direction is forward
    {
      rightMotor(-motorSpeed);                                //drive the right wheel forward
      leftMotor(motorSpeed);                                 //drive the left wheel forward
    }
    else if (botDirection == "b")                    //if the entered direction is backward
    {
      rightMotor(motorSpeed);                               //drive the right wheel forward
      leftMotor(-motorSpeed);                                //drive the left wheel forward
    }
    else if (botDirection == "r")                     //if the entered direction is right
    {
      rightMotor(motorSpeed);                               //drive the right wheel forward
      leftMotor(motorSpeed);                                 //drive the left wheel forward
    }
    else if (botDirection == "l")                   //if the entered direction is left
    {
      rightMotor(-motorSpeed);                                //drive the right wheel forward
      leftMotor(-motorSpeed);                                //drive the left wheel forward
    }
    else if (botDirection == "s")
    {
      rightMotor(0);
      leftMotor(0);
    }
  }
  else if (distance < 20)
  {
    if (botDirection == "b")
    {
      rightMotor(motorSpeed);                               //drive the right wheel forward
      leftMotor(-motorSpeed);                                //drive the left wheel forward
    }
    else
    {
      Serial.print("Object Detected at ");
      Serial.print(distance);
      Serial.println(" cm");
      rightMotor(0);                                  //turn the right motor off
      leftMotor(0);                                   //turn the left motor off
    }
  }
}
/********************************************************************************/
void rightMotor(int motorSpeed)                       //function for driving the right motor
{
  if (motorSpeed > 0)                                 //if the motor should drive forward (positive speed)
  {
    digitalWrite(AIN1, HIGH);                         //set pin 1 to high
    digitalWrite(AIN2, LOW);                          //set pin 2 to low
  }
  else if (motorSpeed < 0)                            //if the motor should drive backward (negative speed)
  {
    digitalWrite(AIN1, LOW);                          //set pin 1 to low
    digitalWrite(AIN2, HIGH);                         //set pin 2 to high
  }
  else                                                //if the motor should stop
  {
    digitalWrite(AIN1, LOW);                          //set pin 1 to low
    digitalWrite(AIN2, LOW);                          //set pin 2 to low
  }
  analogWrite(PWMA, abs(motorSpeed));                 //now that the motor direction is set, drive it at the entered speed
}

/********************************************************************************/
void leftMotor(int motorSpeed)                        //function for driving the left motor
{
  if (motorSpeed > 0)                                 //if the motor should drive forward (positive speed)
  {
    digitalWrite(BIN1, HIGH);                         //set pin 1 to high
    digitalWrite(BIN2, LOW);                          //set pin 2 to low
  }
  else if (motorSpeed < 0)                            //if the motor should drive backward (negative speed)
  {
    digitalWrite(BIN1, LOW);                          //set pin 1 to low
    digitalWrite(BIN2, HIGH);                         //set pin 2 to high
  }
  else                                                //if the motor should stop
  {
    digitalWrite(BIN1, LOW);                          //set pin 1 to low
    digitalWrite(BIN2, LOW);                          //set pin 2 to low
  }
  analogWrite(PWMB, abs(motorSpeed));                 //now that the motor direction is set, drive it at the entered speed
}
```
This loop sends out a signal and reads distance measured by the ultrasonic sensor. If this distance is greater than 20cm, then the user can input a direction and a speed and the robot will go in the prompted direction at that speed. If an object is less than 20cm away from the ultrasonic sensor, then the user will once again input a direction and a speed, but the robot will only go backwards. If another value is typed in, then the robot will not move. We tested our robot by inputting each direction followed by a different speed to see what the movement of the robot would be like. 

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
