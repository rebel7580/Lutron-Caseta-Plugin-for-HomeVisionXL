<b>You must have a HomeVision or HomeVision Pro Controller running HomeVisionXL software for this to work.</b>

**This is Beta Software. Please report issues via the homevision users group**

# Overview

The Caseta Client plug-in provides a way to interface Lutron Caseta switches to HomeVision. Control of the switches can be via a serial command (from your HomeVision controller schedule or from other plug-ins), via Netio, or via MQTT, in conjunction with [MQTT Plug-in For HomeVisionXL Version 1.82 or later](https://github.com/rebel7580/MQTT-Plug-in-For-HomeVisionXL). 

**This plug-in only works with the Lutron Smart Bridge Pro, NOT the non-Pro version!**

# Getting Started
Caseta switches are automatically configured in the plug-in using a file obtained from the Lutron application. 

Before you can set up the plug-in, you must configure the lights and scenes within the Lutron application and generate a configuration file. The names you assign to your switches are the names you will use in the plug-in. 
If you add devices later, you simply generate a new configuration file from the Lutron application.
# Installing

The HomeVisionXL Caseta Plug-in package consists of the following files: 
* caseta.hap - the plug-in itself
* caseta.hlp - the help file

Click the "Code" button then select "Download ZIP".
Extract the above files into your plugin folder.
You can delete README.md and LICENSE.

The plug-in should be enabled via the Plug-in manager.

# Change Log

The Change Log can be found in the Wiki section [Here](https://github.com/rebel7580/Lutron-Caseta-Plugin-for-HomeVisionXL/wiki/Change-Log).

# Help

Help is available through the plug-in. Also, a version of the help file can be found in this Project's [Wiki page](https://github.com/rebel7580/Lutron-Caseta-Plugin-for-HomeVisionXL/wiki/Help).
The help file is very detailed. Please read it throughly to properly set up your devices.
