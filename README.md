# Arduino-smart-lamp
Overview
This project controls a lamp through a relay using a PIR (passive infrared) motion sensor. It supports two operating modes:
Manual mode: Turn the lamp on or off by sending commands through the serial port.
Automatic mode: Turn the lamp on when motion is detected and turn it off after a configurable period without motion.
Serial communication is configured for 9600 baud.
Hardware
Arduino-compatible board.
PIR motion sensor.
Relay module for switching the lamp.
Lamp and an appropriate power supply.
Connecting wires.
Pin Connections
Component
Controller pin
Relay control input
8 (LED)
PIR sensor output
2 (PIR)
Power the sensor and connect the relay according to the documentation for your specific modules. If the relay switches mains voltage, follow appropriate electrical safety procedures. Never connect mains wiring directly to the controller board.
Configuration
The main settings are defined at the beginning of the program:
unsigned long count = 180000; // Delay in milliseconds
#define LED 8                 // Relay control pin
#define PIR 2                 // PIR sensor pin
count = 180000 equals 180 seconds (3 minutes). When the PIR sensor output changes to LOW, the countdown starts. If motion is detected again before the delay expires, the lamp stays on and the timer starts again after motion stops.
Serial Commands
Send these single-character commands through the Serial Monitor or another serial terminal. Commands are case-sensitive.
Command
Action
H Turn the lamp on in manual mode
L Turn the lamp off in manual mode
A Enter automatic mode
M Exit automatic mode
Set the serial port speed to 9600 baud. If necessary, configure the Serial Monitor to send commands without a line ending, so no extra characters are transmitted.
How It Works
Manual Mode
The main loop checks for incoming serial data:
H sets the LED output to HIGH and activates the relay.
L sets the LED output to LOW and deactivates the relay.
A starts automatic mode.
Automatic Mode
After receiving A, the program monitors the PIR sensor:
1. When the sensor output is HIGH, the lamp turns on.
2. When the sensor output becomes LOW, the current millis() value is saved and the delay countdown begins.
3. If no motion is detected before the delay expires, the lamp turns off.
4. If motion is detected before the delay expires, the lamp stays on and the program returns to waiting for motion to stop.
5. The M command exits automatic mode.
Notes About the Source Code
The provided code uses an object named mySerial, but its declaration is not included in the supplied snippet. Before compiling, make sure this object is defined. For example, on an Arduino Uno or Nano using the built-in serial interface, you can usually replace every occurrence of mySerial with Serial.
The automatic-mode logic uses nested while loops. While the program is inside these loops, execution of the main loop is suspended. The M command is checked inside automatic mode, but H and L are not handled as manual-mode commands while automatic mode is active.
Check the relay module's trigger polarity. Some relay modules are active-LOW, meaning they turn on when the control pin is LOW rather than HIGH. If yours is active-LOW, invert the output logic accordingly.
Build and Upload
2. Open the source code in the Arduino IDE or another compatible development environment.
3. Make sure mySerial is declared, or replace it with Serial if you are using the board's built-in serial interface.
4. Select the correct board and port.
5. Compile and upload the program.
6. Open the Serial Monitor at 9600 baud. Send A for automatic mode or H / L for manual control.
License
No license has been specified. If you publish or distribute this project, add an appropriate license if needed