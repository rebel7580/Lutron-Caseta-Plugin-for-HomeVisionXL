<!-- $Revision: 1.1 $ -->
<!-- $Date: 2018/07/11 18:30:16 $ -->
<html>

<body>
<h1>Lutron Caseta Plugin for HomeVisionXL</h1>
<h2>Overview</h2>
The Caseta Client plug-in provides a way to interface Lutron Caseta switches to HomeVision.
<h3>Getting Started</h3>
This plug-in only works with the Lutron Smart Bridge Pro, NOT the non-Pro version!
<br><br>
Caseta switches are automatically configured in the plug-in using a file obtained from the Lutron application.
    
<ol>
<li>Before you can set up the plug-in, you must configure the lights and scenes within the Lutron application.
The names you assign to your switches are the names you will use in the plug-in.
<br><br>
<li>
If you are using the iOS version of the Lutron application, you will need to access the OS settings and find the Lutron application. After you select the Lutron application, enable Advanced settings. Once you have enabled the Advanced settings go back to the Lutron application.
<br>
<br>
If you are using the Android version of the Lutron application, go to settings within the Lutron application and click the check box to show Advanced settings.
<br><br>
<li>
Choose Integration and then
"enable telnet support"
and then
"Send integration report".
<br><br>
<li>
The integration report will be sent to your email address that you have registered via the application. 
Copy the configuration and save it to a file on the computer running HomeVisionXL.
The default is to have it in your plugin directory with the name CasetaConfigFile.json.
<br><br>
The plug-in will read this file and pick up the switch names.
<br><br>
<li>
Find the IP of the SmartHub. It might make sense to lock this IP in (make it static) on your router so it doesn't change.
<br><br>
<li>
Proceed to Caseta plug-in configuration by enabling the Caseta plug-in
and going to the Caseta entry in the HomeVisionXL <i>Plugins</i> menu.
</ol>
<h3>Settings Tab</h3>
<ul>
<li>
"Caseta SmartHub Pro IP Address" should be set to the SmartHub's IP address from Step 5 above.
<li>
"Caseta SmartHub Pro Port" defaults to "23" (for a telnet connection) and probably won't need to change.
<li>
Fill in the "Username" and "Password".
Default values are "lutron" and "integration", respectively and probably won't need to change.
<li>
Make sure "Caseta Config File" points to the file in Step 4 above.
<li>
If logging is desired, check "Create log file".
Doing so will display an entry box for the folder for the logs.
Log files will be created in this folder.
You can enter the folder name directly, or click the "..." button to browse to or create it.
To keep file sizes to a reasonable length, a new file is created each day there is logging information.
The file name is "CasetaLogYYMMDD", with a ".txt" extension if HomeVisionXL is running in Windows.
<li>
If you want the Caseta switches to use MQTT, check "Enable MQTT".
You must have the MQTT plug-in enabled.
Switches will be subscribed using the MQTT plug-in's standard command/power topic using the switch's name as the sub-topic. E.g., 
<pre>
    cmnd/Dimmer1/POWER
</pre>
<li>
"Netio string", "Serial string prefix string", and "Serial string terminator character(s)" are set to reasonable defaults and probably don't need to be changed, except in the rare case that they conflict with other plug-ins.
</ul>
<h3>Device Tab</h3>
This tab displays a list of devices from the Caseta Config file.
<h2>Controlling Devices</h2>
<h3>Device Display Area</h3>
Right-clicking on a device line in the Device tab
will bring up a menu from which you can select "Off", "On", or "Set to".
When clicked, the appropriate command is sent to the selected switch.
<br><br>
Note: The Caseta SmartBrige Pro returns levels to two decimal places.
The plug-in will accept levels with decimals; however, they will be truncated to a whole (integer) number when sent to the switch.
<h3>MQTT Control</h3>
When MQTT is enabled, switches can be controlled by issuing the appropriate MQTT topic. E.g.,
<pre>
    cmnd/Dimmer1/POWER on       Turns Dimmer1 on to 100%
    cmnd/Dimmer1/POWER 50       Turns Dimmer1 on to 50%
    cmnd/Dimmer1/POWER off      Turns Dimmer1 off
    cmnd/Dimmer1/POWER 0        Turns Dimmer1 off
</pre>
The payload is case-insensitive.
<br><br>
Switch state can be retrieved with a payload of "?" or empty. E.g.,
<pre>
    cmnd/Dimmer1/POWER          Returns Dimmer1 state
    cmnd/Dimmer1/POWER ?        Returns Dimmer1 state
</pre>
When a switch changes state, it will automatically report its state. E.g.,
<pre>
    stat/Dimmer1/POWER ON 50.00  Dimmer1 is on at 50%
    stat/Dimmer1/POWER OFF       Dimmer1 is off
</pre>
<h3>Serial Control</h3>
Caseta switches can be controlled within a schedule via serial commands which take the form:
<pre>
    caseta: {device name} {command};
</pre>
"caseta:" is whatever the "Serial string prefix" is defined as.
":' is whatever the "Serial string terminator character(s)" is defined as.
<i>device name</i> can contain spaces and is <i>case-sensitive</i> when matching a <i>name</i> in the Caseta Configuration file.
<i>command</i> can be "off", "on", or 0-100 and is <i>case-insensitive</i>.
A value of 0 is equivalent to "off"; 100 is equivalent to "on".
<br>
<br>
For example, to turn on a switch "Dimmer 1",
the serial command would be:
<pre>
    caseta: Dimmer 1 on;
</pre>
or equivalently,
<pre>
    caseta: Dimmer 1 100;
</pre>
<h3>NetIO</h3>
Caseta switches can be controlled via NetIO using a "netioaction" command in the NetIO application and take the form:
<pre>
    netioaction caseta {device name} {command}
</pre>

"caseta" is whatever the "Netio string" is defined as.
<i>device name</i> can contain spaces and is <i>case-sensitive</i> when matching a <i>name</i> in the Caseta Configuration file.
<i>command</i> can be "off", "on", or 0-100 and is <i>case-insensitive.</i> 
A value of 0 is equivalent to "off"; 100 is equivalent to "on".
<br>
<br>
For example, to turn on a switch named "Dimmer 1",
the button's <i>sends</i> attribute would be set to:
<pre>
    sends:  netioaction caseta Dimmer 1 on
</pre>
The state of the device can be retrieved by:
<pre>
    reads:  get caseta {device name}
</pre>
The <i>get</i> command will return the switch's state,
which will be "OFF", "ON {level in %}", or an empty string if the current state is unknown.
<br>
<h2>Responding to Switch State Changes</h2>
The plug-in will update switch states in the Device Tab when switches change state.

If MQTT is enabled, a "stat" message will be sent with the new state info.
</html>

