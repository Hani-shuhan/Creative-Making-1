# mid-term

Mid-term Project
//Brief description of the project
This project aims to realise the effect of LED light strips lighting up with the rhythm of music through Arduino control. The design transforms the traditional static pumpkin lights into dynamic responsive pumpkin lights, creating a more interactive and festive experience.

//materials required
 1. Arduino control board: for controlling the LED strip to respond to the music.
 2. LED strip WS2812B: for creating colour and brightness effects.
 3. microphone sound sensor: for detecting music or sound intensity.
 4. 5V power supply or USB power supply: to power the Arduino and the LED strip.
 5. wires and connectors: for connecting the components.
 6. Breadboard: to test and connect the circuits.
 7. pumpkin: inside to place the LED strip and electronic components.
 8. Tape or hot melt glue: to fix the LED strip, sensors and Arduino.

![WechatIMG627](https://github.com/user-attachments/assets/ad073ce2-47fc-4a77-808e-ac17044b296c)
![WechatIMG626](https://github.com/user-attachments/assets/36fb3eeb-3978-468a-a608-6ce886a414d0)
![WechatIMG625](https://github.com/user-attachments/assets/bdbf3714-6fdd-4ead-a31d-72833c22e58e)


//Video Display

https://github.com/user-attachments/assets/aac67be7-9034-44a3-972a-2935f69c5f6d


//Detailed photos

![WechatIMG631](https://github.com/user-attachments/assets/7c582a16-9b54-44dc-b37c-b3ba73bff21a)
![WechatIMG630](https://github.com/user-attachments/assets/eb70965c-d752-4608-b290-865474e3a40e)
![WechatIMG633](https://github.com/user-attachments/assets/a3c1bf8e-79e3-4c81-983f-0e91a68f0e30)
![WechatIMG629](https://github.com/user-attachments/assets/a5a4ab85-354f-4e3f-a3ce-42d40fd957f3)


//Design Rationale

 - Interactive: add sound response so that the brightness and colour of the LED strip changes with the rhythm of the music.
 - Aesthetics and Practicality: Traditional pumpkin lamps have monotonous lighting, adding dynamic light strips enhances the visual effect and fun.
 - Resource Accessibility: The use of widely available materials and simple circuit design makes the project easy to reproduce.



//design justification 

The original LED Umbrella Light project used LED strips on the outside of umbrellas to create an outdoor decorative effect. However, it lacked interactivity and provided primarily static lighting. This project re-imagines this design as a sound-responsive pumpkin light for a more dynamic experience. By embedding a WS2812B LED strip and an Arduino microcontroller inside the pumpkin, we transformed the aesthetics of the pumpkin light into a Halloween-themed decoration, while retaining control over the colour and brightness through programming.

For added functionality, a microphone sensor was added to allow the LED strip to react to surrounding sounds or music. This real-time interactivity allows the lights to change in response to audio input, adding a new level of engagement to the festivities. The combination of sound and light not only enhanced the visual and experiential appeal, but also extended the use case to include music-synchronised décor, demonstrating how thoughtful design changes can transform static lighting into an interactive, immersive experience.



//Code Implementation

Using the FastLED library to control the colour change and response time of the LED strip, a microphone sensor is connected to an analogue input pin on the Arduino to change the brightness and colour of the LEDs based on the sound intensity signal. The program can be used to respond to an external music beat to make the strip light up in sync with the music.

//Code
#include <Adafruit_NeoPixel.h>
#ifdef __AVR__
 #include <avr/power.h> // Required for 16 MHz Adafruit Trinket
#endif

#define LED_PIN    6         // NeoPixel data pin
#define LED_COUNT 100        // Number of pixels in the LED strip
#define SOUND_PIN  A0        // Sound sensor connected to A0
#define SOUND_THRESHOLD 90   // Sound threshold to trigger color change

Adafruit_NeoPixel strip(LED_COUNT, LED_PIN, NEO_GRB + NEO_KHZ800);

void setup() {
  #if defined(__AVR_ATtiny85__) && (F_CPU == 16000000)
    clock_prescale_set(clock_div_1);
  #endif
  
  strip.begin();                // Initialize the NeoPixel strip
  strip.show();                 // Turn off all pixels
  strip.setBrightness(255);     // Set maximum brightness
  Serial.begin(9600);           // Initialize serial monitor for debugging
}

void loop() {
  int soundValue = analogRead(SOUND_PIN); // Read value from sound sensor
  Serial.println(soundValue);             // Print sound value for debugging
  
  // Adjust LED strip effect based on sound level


    if (soundValue > SOUND_THRESHOLD) {
    int brightness = map(soundValue, SOUND_THRESHOLD, 1023, 0, 255); // Map sound value to brightness range
    brightness = constrain(brightness, 0, 255); // Ensure brightness is between 0 and 255
    strip.setBrightness(brightness); // Set strip brightness
    // Change color based on sound level
    uint32_t color = strip.Color(225, 0, 0); // Red
    if (soundValue > 500) {
      color = strip.Color(0, 150, 0); // Green
    }
    if (soundValue > 800) {
      color = strip.Color(0, 0, 200); // Blue
    }

    // Update LED strip with new color
    for (int i = 0; i < strip.numPixels(); i++) {
      strip.setPixelColor(i, color); // Set color of each pixel
    }
    strip.show(); // Display updated colors
  } else {
    // If no sound is detected, turn off the LED strip
    strip.clear();
    strip.show();
  }

  delay(100); // Adjust delay as needed
}
