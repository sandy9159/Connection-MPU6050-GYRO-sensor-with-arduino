# Connection-MPU6050-GYRO-sensor-with-arduino

# Using-MPU6050-IMU-with-arduino


![image](https://user-images.githubusercontent.com/19898602/145222775-7fe80f98-87ff-430d-a100-4e419fb48267.png)


Hello all, welcome to another Arduino Sensor Tutorial, in this Blog, we will learn how to wire and code MPU 6050 which is a 6 axis Accelerometer, with our Arduino Board, in detail, so follow till end!


## Hardware

> Arduino Nano
> MPU 6050
> Jumper Wire


## Software

> Arduino IDE

![image](https://user-images.githubusercontent.com/19898602/134221975-63920f76-263c-4c83-9e5e-da3fc90d00ee.png)



MPU6050 is the world’s first integrated 6-axis Motion Tracking device that combines a 3-axis gyroscope, 3-axis accelerometer, 

and a Digital Motion Processor™ (DMP) all in a small 4x4x0.9mm package which is theIntegrated Circuit in Middle, it is based on I2C communication protocol, 

rather than discussing the specifics, refer the Datasheet of MPU 6050.


![image](https://user-images.githubusercontent.com/19898602/134222029-6e6eff19-e465-41fa-a824-7506ec2edc11.png)



MPU 6050 comes in a Module form, with 8 pins, but don’t worry, we will use only 4 important pins and it will be sufficient to integrate with our Arduino Board.

So we have VCC, ground, which takes any input from 2v to 5v, since this board has a voltage regulator on board and thus supports 3.3v logic high and 5v logic high.

Next we have few complimentary resistors and capacitors in SMD package and the most important PART the MPU6050 IC, which is a MEMS or say micro electro mechanical system, which changes voltage depending on change in axis position.

![image](https://user-images.githubusercontent.com/19898602/134222121-a1c36a0d-4a55-441e-af9b-4f50127fb4d3.png)


This IC also has SCL SDA, which are I2C pins and XDA and XCL which are auxiliary Serial pins, we won’t use them with Arduino for this tutorial, we have AD0 which is address select between Auxiliary and Primary ports, lastly we have INT interrupt pin,

connections for our Arduino UNO and NANO are as following:

VCC - 5v

GND - GND

SCL - A5

SDA - A4

(only SDA and SCL pins change for other Arduino boards.)

![image](https://user-images.githubusercontent.com/19898602/134222183-e454ab3d-4e67-42f5-88e1-aee6e3ef55a7.png)


And that’s all for connection

( find all the components at UTSOURCE )


Before moving fuurther I would like to tell you something about PCB

Yes PCB are the heart of the electronics based project usually we hesitate to try custom PCB and opt to homemade solutions

like breadboard or Zero PCB earlier I also was in the same boat, I hesitate to try custom PCB my belief was they are much expensive.

but then I came to know about [JLCPCB.com](https://jlcpcb.com/IAT) and I was totally surprised how low price PCB's are they offering 

there PCB quality is best in market, now I always go with PCB for my project and [JLCPCB.com](https://jlcpcb.com/IAT) is my trusted 

PCB manufacturer, you can also try there PCB service for more details you can visit their website [JLCPCB.com](https://jlcpcb.com/IAT)
![image](https://user-images.githubusercontent.com/19898602/134224512-bea8d1c8-9ebe-448d-bbba-0cbecb42d528.png)


![image](https://user-images.githubusercontent.com/19898602/130722577-c30b7b43-ea89-4847-9c6b-058f9fabeda3.png)![image](https://user-images.githubusercontent.com/19898602/130722585-b5268db1-5f17-428f-ba60-b823140f2a70.png)





Add TipAsk QuestionCommentDownload
Step 4: Install Libraries
Install Libraries
Install Libraries


Before we start Coding, we will need a library called as Arduino MPU-6050 by jarzebski,

also we will need Wire Library, which is inbuilt, so we will just install MPU - 6050 Library.

here is the link to MPU6050 Library.

To install a new library into your Arduino IDE you can use the Library Manager.

Open the IDE and click to the "Sketch" menu and then Include Library > select the option to "Add .ZIP Library''.
Navigate to the .zip file's location and open it.
for more information on importing, refer https://www.arduino.cc/en/guide/libraries

![image](https://user-images.githubusercontent.com/19898602/134222278-1bcd8ff3-3f02-4d65-a08e-729872e66bcf.png)


once the MPU-6050 library is added to Arduino IDE, we have quite a list of examples to choose from, like

> MPU6050_accel_pitch_roll


> MPU6050_accel_simple


> MPU6050_free_fall


> MPU6050_gyro_pitch_roll_yaw


> MPU6050_gyro_simple


> MPU6050_motion


> MPU6050_temperature

we need to start slow to understand the Library and Basics, so lets start with MPU6050_gyro_simpleexample.

(did you know MPU6050 has a Tempreature Sensor as well, but not very accurate one so we didn't discuss it here!)

![image](https://user-images.githubusercontent.com/19898602/134222431-9f94158b-fccf-434d-be04-31b930b2f01c.png)

asically in this example, we will see if our sensor is working so we will display the sensor data on serial monitor.



So we begin the serial monitor in setup part.
Serial.begin(115200);
In this While Loop, the sensor test sequence is executed.
while(!mpu.begin(MPU6050_SCALE_2000DPS, MPU6050_RANGE_2G)) 
{    Serial.println("Could not find a valid MPU6050 sensor, check wiring!");
     delay(500); 
}


Sometimes we are making a project, and have to set our sensor in a specific orientation, we need offsets, we don’t need it for this tutorial,
but to change offset, simply un-comment these lines.
// mpu.setGyroOffsetX(155);
// mpu.setGyroOffsetY(15);  
// mpu.setGyroOffsetZ(15);



( uncomment by removing "//" )
there is a calibration line, which will virtually set our sensor flat.
// Calibrate gyroscope. The calibration must be at rest.
// If you don't want calibrate, comment this line.
mpu.calibrateGyro();
Remember offset and calibration are two different things, offset will give you defined calibration, for example, you can mount this sensor at weird angle and yet it will act as reference point for zero.
Next we have sensitivity, which at default is 3.
// Set threshold sensivty. Default 3.
// If you don't want use threshold, comment this line or set 0. 
mpu.setThreshold(3);
In check loop section, basic hardware checking is done.
void checkSettings()

```javascript
{
  Serial.println();
  
  Serial.print(" * Sleep Mode:        ");
  Serial.println(mpu.getSleepEnabled() ? "Enabled" : "Disabled");
  
  Serial.print(" * Clock Source:      ");
  switch(mpu.getClockSource())
  {
    case MPU6050_CLOCK_KEEP_RESET:     Serial.println("Stops the clock and keeps the timing generator in reset"); break;
    case MPU6050_CLOCK_EXTERNAL_19MHZ: Serial.println("PLL with external 19.2MHz reference"); break;
    case MPU6050_CLOCK_EXTERNAL_32KHZ: Serial.println("PLL with external 32.768kHz reference"); break;
    case MPU6050_CLOCK_PLL_ZGYRO:      Serial.println("PLL with Z axis gyroscope reference"); break;
    case MPU6050_CLOCK_PLL_YGYRO:      Serial.println("PLL with Y axis gyroscope reference"); break;
    case MPU6050_CLOCK_PLL_XGYRO:      Serial.println("PLL with X axis gyroscope reference"); break;
    case MPU6050_CLOCK_INTERNAL_8MHZ:  Serial.println("Internal 8MHz oscillator"); break;
  }
  
  
  
  
  Serial.print(" * Gyroscope:         ");
  switch(mpu.getScale())
  {
    case MPU6050_SCALE_2000DPS:        Serial.println("2000 dps"); break;
    case MPU6050_SCALE_1000DPS:        Serial.println("1000 dps"); break;
    case MPU6050_SCALE_500DPS:         Serial.println("500 dps"); break;
    case MPU6050_SCALE_250DPS:         Serial.println("250 dps"); break;
  } 
  
  
  
  Serial.print(" * Gyroscope offsets: ");
  Serial.print(mpu.getGyroOffsetX());
  Serial.print(" / ");
  Serial.print(mpu.getGyroOffsetY());
  Serial.print(" / ");
  Serial.println(mpu.getGyroOffsetZ());
  
  Serial.println();
}


I highly suggest to leave this loop as it is.
in loop section, which is most important part of this entire code, that is getting the values from our sensor. First we need to call the values, using mpu.readRawGyro or mpu.readNormalizeGyro, now the concept of raw and normalized is such that raw are basically numbers and normalized values are values which go through filters and calculations or you can say, processed data.
void loop()


{
  Vector rawGyro = mpu.readRawGyro();
  Vector normGyro = mpu.readNormalizeGyro();

We have 3 axis, called as x y and z, which can be called using variable name which we set as rawGyro, followed by axis name,to make a project, we will need these 3 values of x,y and z using this variablename.axis command.
  Serial.print(" Xraw = ");  
  Serial.print(rawGyro.XAxis);
  Serial.print(" Yraw = ");
  Serial.print(rawGyro.YAxis);
  Serial.print(" Zraw = ");
  Serial.println(rawGyro.ZAxis);
  Serial.print(" Xnorm = ");
  Serial.print(normGyro.XAxis);
  Serial.print(" Ynorm = ");
  Serial.print(normGyro.YAxis);
  Serial.print(" Znorm = ");
  Serial.println(normGyro.ZAxis);
atlast, to view the values at comfortable speed, lets increase the delay from 10 to 1000
delay(10);
}

```

since our example code is ready and we understand what we did in code, its time to upload the code and view results.


![How to configure 6-Axis MP-6050 Gyroscope Sensor for first run beginner tutorial](https://user-images.githubusercontent.com/19898602/134222951-ae650eef-587f-4397-8b21-5d77d7825995.gif)

