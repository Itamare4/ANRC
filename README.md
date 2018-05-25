ANRC (Autonomous Nitro RC)
------------------------
<p align="center">
<img src="https://github.com/Itamare4/ANRC/blob/master/MD_Images/Car_Zoom.jpg?raw=true" height="300" width=auto>
<img src="https://github.com/Itamare4/ANRC/blob/master/MD_Images/IMG_4762.JPG?raw=true" height="300" width=auto>
<img src="https://cdn.hackaday.io/images/5953261527262523526.png" height="300" width=auto>
</p>

### Contacts ###
Itamar Eliakim <br>

### Goal ###
Got an old RC car platform that wasnt in use for around ~10 years. Project goal is to implement algorithms for localization, lane tracking, steering etc. and creating a tiny nitro self-driving car.

### Hardware ###
* Nitro RC Car Platform
* Neato XV-11 Lidar
* Logitech C170
* Orange Pi Lite (AllWinner H3), might change later to BeagleBone
* STM32F103C8
* FlySky i6
* Lipo 7.4v 2200mah
* LM2596 - 5V 3A Buck Regulator
* AMS1117 - 5V to 3.3V (Powering XV11 DC Motor)
* For DIY Electric starter - High torque DC motor (took mine from an old neato)


### Tools/Connectors Needed ###
* 3D printer
* crimp tool
* XT60 Connectors
* PCB Breadboard
* Soldering Station

### Software ###
* Ubuntu 16.04 LTS
* ROS Kinetic

### Experiments ###


### Videos ###


### About ###
Itamar Eliakim<br>
Tel Aviv, Israel<br>
Email - Itamare@gmail.com



Detailed Changelog
------------------------
<p align="center">
<img src="https://github.com/Itamare4/ANRC/blob/master/MD_Images/Car_Zoom.jpg?raw=true" height="400" width=auto>
</p>

### #3 ###
Wrote a ROS package in python for processing the image from the Logitech C170 (Code available on GitHub). In most example codes available online it's easier to detect the lane by filtering out the lane colors(white, yellow), here i did it in a narrow street so i didn't have many colors to filter out.
<p align="center">
<img src="https://cdn.hackaday.io/images/9654471527261893932.jpg" height="400" width=auto>
</p>

Based on the great tutorial of Galen Ballew:
https://medium.com/@galen.ballew/opencv-lanedetection-419361364fc0

I had to do many modification so it will work without filtering the yellow and white colors, I've used the following steps:

1. Convert to HSV
2. Filter out background colors.
3. Calculating Sobel magnitude.
4. Flood Fill.
5. Split to left and right image.
5. Manually find contours.
6. Draw a line for the left and right edge.

I got the following result:
<p align="center">
<img src="https://cdn.hackaday.io/images/3545391527262176709.png" height="400" width=auto>
</p>

Running Yolo object detection, with VOC weights on the image(offline), we got the following result - 
<p align="center">
<img src="https://cdn.hackaday.io/images/5953261527262523526.png" height="400" width=auto>
</p>

### #2 ###
I had to record some bagfiles to start working on the lane tracking, auto steering of the car. To make things easier i thought of connecting the Flysky RC to a simple python code and by toggle the switch on the remote start and pause the rosbag record process.
Connecting the Flysky RC to OrangePi sounds like an easy task, had some issues with sampling the PWM signal from the remote at frequency high enough to determine the switch state(This can be done easly on Raspberry Pi using pigpio), if you have some ADC to I2C you can do it easly, something like ADS1115, or just playing with RC circuit.<br>
I didn't have enough time to spend on it, so a simple Arduino Nano was perfect for this task. Routing the SWD on Flysky to CH6 on the reciever and connecting it to PIN 6 on the Arduino.<br>
The python code for reading values for Arduino and starting the record sometimes doesnt end cleanly(will be fixed later), need to run this on the bags - 
```
rosbag reindex *.bag.active
rosbag fix *.bag.active repaired.bag
```

### #1 ###
### Mechanical ###
Getting the RC Car ready to go after ~10 years, engine compression seems to be good, carburetor is completely cleaned. Found out the car didn’t start because of a small pin between the crankshaft and the backplate, when pulling the cord, it rotated only the shaft at the backplate. Instead of ordering a new spring/pull mechanism (didn’t have the patience) just put some Teflon tape between the crankshaft and the small pin, it worked perfectly.    
<p align="center">
<img src="https://github.com/Itamare4/ANRC/blob/master/MD_Images/CrankShaft.png?raw=true" height="400" width=auto>
</p>
Next, designing the parts, I’ve separated this to three major units:<br>
Rear Unit: Webcam, Lights(Later).<br>
Center Unit: Power Regulators (5V and 3.3V), Power Distribution Board, RC Receiver.<br>
Front Unit: Battery, XV11 Lidar, Orange Pi.<br><br>

### Hardware ###
**Rear Unit:**  
1x Logitech C170  
<p align="center"> <img src="https://github.com/Itamare4/ANRC/blob/master/MD_Images/Rear_Unit.jpg?raw=true" height="400" width=auto></p>

**Center Unit:**  
Main power supply – LIPO 7.4 2200mah, several components at this design works at 5V/3.3, will use two different regulators:  
1xLM2596 – 5V 3A Buck converter  
1xAMS1117 – 3.3V 800mA Buck converter  
<p align="center">
<img src="https://cdn.hackaday.io/images/9512821526818357513.png" height="400" width=auto>
</p>

<p align="center">
<img src="https://cdn.hackaday.io/images/6598211526818363078.png" height="400" width=auto>
</p>


| PIN  | Details  |
|---|---|
| 1 | 5V from power regulator, pin 10  |
| 2 | GND, pin 11 |
| 3 | 5V - Orange PI|
| 4 | GND - Orange PI |
| 5 | 5V RC Receiver |
| 6 | GND RC Receiver |
| 7 | 3.3V output |
| 8 | 7.4V 2200mah LIPO battery |
| 9 |  GND |

**Front Unit:**    
1x Orange Pi Lite(Allwinner H3)  
1x XV11 Neato Lidar  
1x STM32F103C8  
<p align="center">
<img src="https://github.com/Itamare4/ANRC/blob/master/MD_Images/IMG_4762.JPG?raw=true" height="400" width=auto>
<img src="https://github.com/Itamare4/ANRC/blob/master/MD_Images/Front_Unit.png?raw=true" height="400" width=auto>
</p>

### Software ###
1.	Install Armbian for Orange Pi from here:
[https://www.armbian.com/orange-pi-lite/]( https://www.armbian.com/orange-pi-lite/)
2.	Install ROS kinetic
```
echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" >ros-latest.list
sudo cp ros-latest.list /etc/apt/sources.list.d/
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
sudo apt-get update
sudo apt-get install ros-kinetic-ros-base
sudo rosdep init
rosdep update
```

Congratulations, you have ROS kinetic running on your Orange PI. Add some of the basic ROS packages, such as Hector SLAM, and the drivers to XV11, based on this tutorial:
 [http://meetjanez.splet.arnes.si/2015/08/22/neato-xv-11-to-ros-slam/]( http://meetjanez.splet.arnes.si/2015/08/22/neato-xv-11-to-ros-slam/)

Got the XV11 lidar work on the Nitro RC, example of the Laserscan:
<img src="https://github.com/Itamare4/ANRC/blob/master/MD_Images/Workspace%201_001.png?raw=true" height="400" width=auto>


Next weeks:
1.	Check how slow can the car move, XV11 lidar works at really really low speed compare to the speed of the car, will need to see if we can use this lidar or just replace it with a better RGB camera.
2.	Stress test the poor Orange Pi to see if we can use this or will need to change to a better one.
3.	DIY Electric starter.
4.	Record some videos form the webcam so we can start working on the lane tracking, and autonomous steering.



