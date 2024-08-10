# Robot-mouth
Making a Robot Mouth by LED Dot Matrix 

## Definitions :
<img src="https://www.crowdsupply.com/img/76e8/wokwi-logo_png_organization-profile.png" width="200" />

- Wokwi -> is an online platform that provides a simulation environment for developing and testing Arduino and ESP32 projects. It allows users to create virtual circuits and simulate their behavior without the need for physical hardware.

<img src="https://content.instructables.com/FWL/9DXP/ITW2CVXM/FWL9DXPITW2CVXM.png?auto=webp&fit=bounds&frame=1auto=webp&frame=1&height=300" width="200" />

- Arduino UNO -> is a popular microcontroller board based on the ATmega328P chip. It's widely used in electronics projects and prototyping due to its ease of use and versatility. The Uno board includes digital and analog input/output pins that can be programmed to interact with various sensors, actuators, and other electronic components. It's known for being beginner-friendly while offering enough capabilities for more advanced projects, making it a staple in the maker and DIY electronics communities.
<img src="https://th.bing.com/th/id/R.9ef5d0f4c6fc5e2d4ba21f9ff9872dd0?rik=ftCp7ns%2fCXJfDg&pid=ImgRaw&r=0" width="200"/>

- LED Dot Matrix -> is a display technology that uses a grid of light-emitting diodes (LEDs) arranged in rows and columns to create images, text, or animations. Each individual LED acts as a pixel, and by turning specific LEDs on or off, or by adjusting their brightness, the matrix can display various patterns or information.
----------
  
## Simulation :
Hardware Required:
1. Arduino UNO
2. LED Dot Matrix
   
![Screenshot 2024-08-09 170434](https://github.com/user-attachments/assets/2a64ddd7-cc5c-4c14-adbb-67ab86b1bf04)

**LED dot matrix pins :**

- VCC -> is the power pin, and it's connected to 5V on Ardunio (the red wire).
- GND -> is the ground pin, and it's connected to GND on the ardunio (the black wire).
- DIN -> is the data input pin, and it's connected to digital pin 11 (the yellow wire).
- CS -> is chip select, and it's connected to digital pin 10 (the green wire).
- CLK -> is clock pin, and it's connected to digital pin 13 (the pink wire).
   

------------
## Coding :
``` C++
// Include the llibraties
#include <MD_MAX72xx.h>
#include <SPI.h>

// Define hardware type, size, and pins
#define HARDWARE_TYPE MD_MAX72XX::FC16_HW
#define MAX_DEVICES 1 // The number of led dot matrix 
#define CLK_PIN   13
#define DATA_PIN  11
#define CS_PIN    10

// Create a MAX72xx object
MD_MAX72XX display = MD_MAX72XX(HARDWARE_TYPE, CS_PIN, MAX_DEVICES);

// Bitmaps for animations
byte smiley[2][8] = {
  {0b00000000,0b00000000,0b00100100,0b00000000,0b01000010,0b00111100,0b00000000,0b00000000},
  {0b0000000, 0b00000000, 0b00100100, 0b00000000, 0b00111100, 0b00000000, 0b00000000, 0b00000000}
};


// Scrolling text message
const char *scrollingText = " Components101 ";
uint16_t scrollDelay = 100; // Delay between shifts in milliseconds
uint16_t displayTime = 500; // Time each frame is shown in milliseconds
int brightnessLevel = 8; // Set brightness level (0 is minimum, 15 is maximum)

void setup() {
  Serial.begin(115200);
  display.begin(); // Initialize the display
  display.control(MD_MAX72XX::INTENSITY, brightnessLevel); // Set the brightness level
  display.clear(); // Clear the display
}

void loop() {
  playAnimation(smiley, 2, 2);

}

void playAnimation(byte animation[][8], int frameCount, int repeat) {
  for (int r = 0; r < repeat; r++) {
    for (int i = 0; i < frameCount; i++) {
      displayFrame(animation[i]);
      delay(displayTime); // Wait before showing next frame
    }
  }
}

void playRandomFlash(int count, int repeat) {
  for (int r = 0; r < repeat; r++) {
    for (int i = 0; i < count; i++) {
      byte row = rand() % 8;
      byte col = rand() % 8;
      display.setPoint(row, col, true);  // Turn on a random LED
      display.update();
      delay(50);  // Short flash
      display.setPoint(row, col, false); // Turn off the LED
      display.update();
    }
  }
}

void displayFrame(byte frame[8]) {
  for (int i = 0; i < 8; i++) {
    display.setRow(0, i, frame[i]); // Display each row of the bitmap
  }
  display.update(); // Ensure the display is updated
}

```

---------------------------------
## The Resulte :


https://github.com/user-attachments/assets/7064abbb-4d7d-4e83-9f18-cdab637c35b6

From the video, I attempt to make the robot move its mouth while talking. Here the project link on [Wokwi](https://wokwi.com/projects/405675697097801729).

------------------

## Helpful Resources :
1. [Document about LED dot matrix](https://lastminuteengineers.com/max7219-dot-matrix-arduino-tutorial/)
2. [YouTube video explains how led dot matrix works](https://youtu.be/pHw3AokxRXM?si=cn4a4Ch2YJvymox-)

