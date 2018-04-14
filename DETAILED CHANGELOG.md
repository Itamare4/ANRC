ANRC (Autonomous Nitro RC)    
Detailed Changelog
------------------------
<p align="center">
<img src="https://github.com/Itamare4/ANRC/blob/master/MD_Images/Car_Zoom.jpg?raw=true" height="400" width=auto>
</p>

### Week 1 - (4.7.2018) ###
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
<p align="center"> <img src=" https://github.com/Itamare4/ANRC/blob/master/MD_Images/Power_Distribution_Board.png?raw=true" height="400" width=auto><img src=" https://github.com/Itamare4/ANRC/blob/master/MD_Images/Regulator.png?raw=true" height="400" width=auto></p>

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


