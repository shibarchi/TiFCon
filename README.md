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

## Understanding X-Plane data

X-plane data output can be turned on by opening settings window -> Data Output -> selecting data from the list (Show in Cockpit/ Network via UDP).

For e.g. we have selected the following -

```
Index    Data
  3     Speeds
 17     Pitch...
 26   Throttle(a)
```

The same can be seen as follows in the cockpit.

![alt text](https://github.com/shibarchi/TiFCon/blob/master/cockpit_data.PNG)

The output data format is very interesting and needs understanging of use the data.
We received a string as displayed below - 

![alt text](https://github.com/shibarchi/TiFCon/blob/master/dataread.PNG)

The first 5 elements are ASCII codes (68,65,84,65,42) that represents 'DATA*'.
The next digit is the intex number of the data followed by 3 zeros. i.e. 3,0,0,0 for Speeds.

Next 28 elements represents readings, each feature/ reading is represented by 4 bytes. 
for e.g. the next four elements (106,34,84,67) represents Indicated air speed in knots.

The next 24 elements (,89,12,84,67,67,149,107,67,131,120,106,67,0,192,121,196,181,30,116,67,85,141,135,67,85,141,135,67,) belongs to Vtrue,Vtrue, blank,Vind,Vtru,Vtrue as shown in the cockpit data out. 

The next element is 17 i.e. the index for Pitch,roll, heading followed by 3 zeros. (60,118,187,64,28,193,81,61,1,248,165,66,184,152,135,66,0,192,121,196,0,192,121,196,0,192,121,196,0,192,121,196,) represents pitch, roll, true heading and magnetic heading followed by 4 blank spaces (0,192,121,196 is one blank space).

***BitConverter.ToSingle*** (or Int/Int16/Double) can be used in C# to get numeric data from the 4 bytes; 
i.e. **BitConverter.ToSingle(xdata, 45)** will give pitch value if data array is stored as **xdata**

