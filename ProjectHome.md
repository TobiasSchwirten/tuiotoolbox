# TUIO ToolBox by Tobias Schwirten v1.0 #
The TUIO ToolBox is made to receive TUIO/2dCur data and process the data.
The data can be forwarded as TUIO/2dCur data or in a new protocol using OSC.
For more information please visit www.tuio.org and www.opensoundcontrol.org.

There are two options how the Software can modify the data:
> - Filtering

> - Transforming

Those will later be explained in detail.

The software uses two different file types for storing settings etc.
There is one file called TUIO\_ToolBox\_config.xml. If this file is not available in the program path, it will be created with default values.
One TUIO\_Filter software has just one TUIO\_Filter\_config file. It stores basic settings like Network Ports and IPs.
The other file can be created by users on their own (File --> Save) are GuiData files. These files basically store all values users will find in the GUI.
The software automatically stores the last saved GuiData file in the TUIO\_Filter\_config.xml and will automatically reload this file during start.

Besides Filtering and Transforming, there is a Function that denoises input data. Although it is kind of a filter, it works seperately to the other filters.
It processes the incoming data at a very early stage. It smoothes the values with a simple but robust algorithm interpolating between a few values.
The user can define the strength of the denoising with the smoothingWidth parameter.


When the software runs, a small window appears. By closing this you will exit the application.
Flashing green Label shows you that the app recieves data.
IF the label stays red, no data is received.


---


## TUIO Filtering ##
The TUIO option can completely be turned off or it could just forward the original TUIO data or it could forward filtered data.
If filtering option is turned on, you can filter the data by using the following parameter:
- Minimum Lifetime
Minimum lifetime in miliseconds after which a detected obstacle is valid
- Max Distance
The Maximum Distance in TUIO Coordinates (0-1) describing a radius which clusters all objects inside
- Scaling X
Scaling in X Direction
- Scaling Y
Scaling in Y Direction


## Transformator Option ##
The 2dCur Transformator receives 2dCur messages and sends them to different OSC Addresses:

/alive		- The messages that are usually send after the String “alive” was transmitted

/fseq		- The messages that are usually send after the String “fseq” was transmitted

/setn		- The messages that are usually send after the String “set” was transmitted.
> Where 'n' is a counter for all detected obstacle. If there are 6 obstacles detected, you will recieve data on /set0, /set1, /set2, /set3, /set4 /set5 ...

The message format that is transmitted looks like this:

/setn mappedID[int](int.md), x[float](float.md), y [float](float.md), X[float](float.md), Y[float](float.md), m[float](float.md)
with: 	mappedID 	--> the new unique ID for this point
> x	 	--> TUIO x coordinate
> y 		--> TUIO y coordinate
> X 		--> TUIO Movement Vector in X-Direction
> Y 		--> TUIO Movement Vector in Y-Direction
> m 		--> TUIO Motion acceleration


The transformator supports up to 20 obstacles.
All /setn that were never used, will send for all parameter the value -9.
If an obstacle n is removed, than the system will send on the x and y coordinates still the last values but on the other parameters (X, Y, acc) it
will send the value -9.


## Acknowledgements ##
The software TUIO ToolBox is using the Java TUIO library by Martin Kaltenbrunner.
The included Java TUIO implementation as well as TUIO ToolBox directly is using the JavaOSC library by Chandrasekhar Ramakrishnan. Please note that the GPL demands the publication of the full source code of any derived work. If you are planning to develop a proprietary application based on this code, we may be able to provide an alternative commercial license option.