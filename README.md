# LED-wind-chime
touchdesigner -- Teensy -- LED strip 2812B


1.Computer tuning touch 
	It has to be a PC, Mac does not support serial unless you have a USB to serial adaptor.

2.Download Teensy Loader
	Download Teensy Loader on your PC and follow guides to get started. https://www.pjrc.com/teensy/first_use.html

3.Download Arduino 
	Download Arduino from https://www.arduino.cc/

4.Download library in Arduino
	If you are using Adafruit Neopix library , please download it from Arduino library. I am using FastLED for testing, which can be found: 
	http://fastled.io/
	There are other LED library in Arduino, it really does not matter which one are using as long as it has been downloaded in Arduino

5.LED Strip lights
	I am using strip lights named 2812B, it could be brought from Amazon , LED strip lights are varies, knowing strip product name will help coding and trouble shooting as we moving on

6.Teensy 3.2 OctoWS2811 Adaptor
	It is highly recommended to have one, it can be found both on PJRC store or Amazon. 

7.Solder Teensy 3.2 OctoWS2811 Adaptor to Teesny 3.2 board
	If you are using Teesny 3.6 board, you have to use  double insulator pins and sockets. 

8.Cat#5 or Cat#6 ethernet cable. 
	Cut RJ45 jack , following instruction here

9.Power for LED strips
	If you are using less LED, it could be directly powered by Teensy board 5V output. If you are working on relatively big scale installation, 5V power supply is recommend. NO USE power over 5V, It would damage whole strip. 

10.Write Code in Arduino for Receiving Messages
```
 #include <Adafruit_NeoPixel.h>
 #define NUM_PIXELS 824 // The number of Led Lights
 #define DIN_PIN 6 // The Pin Number of your Led (Din)wire

 uint32_t pix[NUM_PIXELS]; //unsigned integer range(0-4294967295)
 char input[NUM_PIXELS * 3 ]; // total input value, led num * 3 (each led has r, g, b three value)

 Adafruit_NeoPixel strip = Adafruit_NeoPixel(NUM_PIXELS, DIN_PIN, NEO_GRB + NEO_KHZ800);

 void setup()
  {
    strip.begin();
   Serial.begin(1382400); //Baud Rate
  }

 void loop()
  {
    int r, g, b;
    char charsRead;
    if (Serial.available() > 0)
  {
    charsRead = Serial.readBytes(input, NUM_PIXELS * 3); // read the bytes

    // calculate the rgb value in the led strip
    for (int i = 0; i < NUM_PIXELS; i++)
    {
      r = input[i * 3]; 
      g = input[i * 3 + 1];
      b = input[i * 3 + 2];

      pix[i] = strip.Color(r, g, b);
      strip.setPixelColor(i, pix[i]);
/*-------------------Important: the serial print will slow down the message sending speed----------------------*/
      //Serial.print("input was.. ");  
      //Serial.print(r);
      //Serial.println(g);
      //Serial.println(b);
    }
    strip.show();
  }
}
```
11. Write Code to Send message from TouchDesigner to Arduino
	Using Execute DAT in touch , write down following code to send information from TouchDesigner to Arduino by serial. 
```
def onFrameEnd(frame):

	num = op(‘THE NAME OF CHOP')
	op(‘SERIAL DAT NAME ').sendBytes(*num[0].vals)
	#print(num.chan('r','g','b'))
	return
	
def onPlayStateChange(state):
	num = op('THE NAME OF CHOP')
	#op('SERIAL DAT  NAME').sendBytes(*num[0].vals)
	print(*num[0].vals)
	return
```
                                                              



           
