# Arduino Joystick Controller

## Overview
This resource should help most to wire and program every Cadet module.  Please view the references below for other helpful links. It uses the Arduino Joystick Library to interface with computers as a standard HID device.

## Requirements
- Arduino board with ATmega32U4 or compatible microcontroller (Leonardo, Micro, Pro Micro)
- [Arduino Joystick Library](https://github.com/MHeironimus/ArduinoJoystickLibrary/tree/master)
- Arduino IDE (1.6.6 or higher)
- Input components (depending on your setup):
  - Analog joysticks
  - Push buttons
  - Toggle switches
  - Potentiometers

## Installation

1. **Install the Arduino Joystick Library**
   - Download the library from https://github.com/MHeironimus/ArduinoJoystickLibrary/tree/master
   - In Arduino IDE: Sketch > Include Library > Add .ZIP Library
   - Select the downloaded zip file

2. **Connect Your Hardware**
   - Wire your input components to the appropriate pins on your Arduino board
   - Make sure to include pull-up/pull-down resistors as needed

## Basic Usage

```cpp
#include <Joystick.h>

// Create the Joystick object
Joystick_ Joystick;

void setup() {
  // Initialize Button Pins as inputs with pull-up resistors
  pinMode(2, INPUT_PULLUP);
  pinMode(3, INPUT_PULLUP);
  
  // Initialize Joystick Library
  Joystick.begin();
}

void loop() {
  // Read pin values and update joystick state
  Joystick.setButton(0, !digitalRead(2));
  Joystick.setButton(1, !digitalRead(3));
  
  // Read analog inputs (if used)
  int xAxis = analogRead(A0);
  Joystick.setXAxis(map(xAxis, 0, 1023, 0, 255));
  
  delay(10);
}
```

## Advanced Features

### Multiple Axes Configuration
The library supports configuring multiple axes:
```cpp
Joystick_ Joystick(JOYSTICK_DEFAULT_REPORT_ID, 
  JOYSTICK_TYPE_JOYSTICK, 32, 0,
  true, true, true,     // X, Y, Z axes
  true, true, true,     // Rx, Ry, Rz axes
  false, true,          // Rudder, Throttle
  false, false, false); // Accelerator, Brake, Steering
```

### Setting Axis Ranges
You can set custom ranges for your axes:
```cpp
Joystick.setXAxisRange(-127, 127);
Joystick.setYAxisRange(-127, 127);
```

### Using the Hat Switch
```cpp
// Set hat switch position (0-360, -1 = center)
Joystick.setHatSwitch(0, hatPosition);
```

### Sample code I was able to dig up
```cpp
#include <Keypad.h>
#include <Joystick.h>

//Potentiometer Setup
Joystick_ Joystick(JOYSTICK_DEFAULT_REPORT_ID,JOYSTICK_TYPE_JOYSTICK,
  0, 0,                  // Button Count, Hat Switch Count
  true, true, true,     // X, Y, and Z Axis
  true, false, false,     // Rx, but no Ry or Rz (Rx represents throttle as fs2020 does not recognize the throttle axis!)
  true, true,          // No rudder or throttle (represented as Rx!)
  true, true, false);  // No accelerator, brake, or steering; 
int throttle = 0;
int fuel = 0;
int pitch = 0; 
int roll = 0;
int rudder = 0;
int trim = 0; 
int L_brake = 0;
int R_Brake = 0;               
int flaps = 0;


void setup(){
  Joystick.begin(); //Starts joystick
}
  
void loop(){
  
  throttle = analogRead(A0);
  throttle = map(throttle,0,1023,0,255);
  Joystick.setRxAxis(throttle);
  
  fuel = analogRead(A1);
  fuel = map(fuel,0,1023,0,255);
  Joystick.setXAxis(fuel);

  pitch = analogRead(A2);
  pitch = map(pitch,0,1023,0,255);
  Joystick.setYAxis(pitch);

  roll = analogRead(A3);
  roll = map(roll,0,1023,0,255);
  Joystick.setYAxis(roll);

  rudder = analogRead(A4);
  rudder = map(rudder,0,1023,0,255);
  Joystick.setYAxis(pitch);

  trim = analogRead(A5);
  trim = map(trim,0,1023,0,255);
  Joystick.setYAxis(pitch);

  L_brake = analogRead(A6);
  L_brake = map(L_brake,0,1023,0,255);
  Joystick.setYAxis(L_brake);

  R_Brake = analogRead(A7);
  R_Brake = map(R_Brake,0,1023,0,255);
  Joystick.setYAxis(R_Brake);
  
  flaps = analogRead(A8);
  flaps = map(flaps,0,1023,0,255);
  Joystick.setZAxis(flaps);
  delay(5);
}
```

## Troubleshooting

### Device Not Recognized
- Ensure you're using a board with native USB support
- Try pressing the reset button on your Arduino
- Check that you've installed the correct drivers

### Erratic Input Behavior
- Add debouncing for buttons
- Verify your wiring and check for loose connections
- Use capacitors to filter noise on analog inputs

### Calibration Issues
- Add deadzone handling for analog inputs

## Examples

The Arduino Joystick Library includes several example sketches:
- JoystickTest - Basic functionality test
- FlightControllerTest - Complex setup with multiple axes
- GamepadExample - Simple gamepad implementation
- HatSwitchTest - Demonstrates hat switch functionality

[Great video on creating button boxes and matrices](https://youtu.be/Z7Sc4MJ8RPM?si=0KqFiOvn3FeP2ZkJ)

## Resources
- [Arduino Joystick Library Documentation](https://github.com/MHeironimus/ArduinoJoystickLibrary/tree/master)
- [Arduino IDE Download](https://www.arduino.cc/en/software)
- [USB HID Usage Tables](https://www.usb.org/sites/default/files/documents/hut1_12v2.pdf)
