# TiFCon (Time-predictable Flight Control)

TiFCon, Time-predictable Flight Control, is a fly-by-wire flight control system based on T-CREST architecture.





## X-Interface 

X-Interface is a tool written in C# to interface between X-Plane, control device and embedded flight control computer i.e. T-CREST in this case. However, this can be used to communicate with any embedded hardware (like Arduino) with serial interface. 

```
X-Plane <________UDP ______
                            \
                              X-Interface  <-------- Serial --------->  Embedded Flight Computer (T-CREST)  
Joystick <______ USB _______/
```
The default configuration assumes that X-Plane server is at localhost (IP 127.0.0.1) port: 49000. The same can be configured as shown below -

![alt text](https://github.com/shibarchi/TiFCon/blob/master/X_plane_settings.PNG)

The default serial port is COM5 (115200 bits/sec, 8-bit word, 1-stop bit, no parity) 
The same can be configured as shown below. Device Manager -> Ports -> USB Serial Port(COM X) -> (right click) -> Properties -> Port settings -> Advanced...

![alt text](https://github.com/shibarchi/TiFCon/blob/master/COM_port.PNG)

The default controls are as follows - 

![alt text](https://github.com/shibarchi/TiFCon/blob/master/joystick.png)
