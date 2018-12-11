# LED-wind-chime
touchdesigner -- Teensy -- LED strip 2812B


1.Computer tuning touch<br />
	It has to be a PC, Mac does not support serial unless you have a USB to serial adaptor.

2.Download Teensy Loader<br />
	Download Teensy Loader on your PC and follow guides to get started. https://www.pjrc.com/teensy/first_use.html

3.Download Arduino<br /> 
	Download Arduino from https://www.arduino.cc/

4.Download library in Arduino<br />
	If you are using Adafruit Neopix library , please download it from Arduino library. I am using FastLED for testing, which can be found [here](http://fastled.io/).
	There are other LED library in Arduino, it really does not matter which one are using as long as it has been downloaded in Arduino

5.LED Strip lights<br />
	I am using strip lights named 2812B, it could be brought from Amazon , LED strip lights are varies, knowing strip product name will help coding and trouble shooting as we moving on

6.Teensy 3.2 OctoWS2811 Adaptor<br />
	It is highly recommended to have one, it can be found both on PJRC store or Amazon. The output voltage of Teensy is 3.3 V, some new LED strips are taking 5V digital signal. This adaptor convert voltage for you.

7.Solder Teensy 3.2 OctoWS2811 Adaptor to Teesny 3.2 board<br />
	If you are using Teesny 3.6 board, you have to use  double insulator pins and sockets. 

8.Cat#5 or Cat#6 ethernet cable<br />
	Cut RJ45 jack , following instruction here

9.Power for LED strips<br />
	If you are using less LED, it could be directly powered by Teensy board 5V output. If you are working on relatively big scale installation, 5V power supply is recommend. NO USE power over 5V, It would damage whole strip. 
	Caculate how much Amp you need befor make decision on what kind of power supply you need, each LED has RGB, if you get power consumption for one LED, it needs to multiply 3. 
	[How to](https://learn.adafruit.com/adafruit-neopixel-uberguide/powering-neopixels) 

10.Write Code in Arduino for Receiving Messages<br />
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
11. Write Code to Send message from TouchDesigner to Arduino<br />
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
  
 12. About 74HCT245 or similar buffer<br />
 
 	Please be aware, it could featured as Teensy 3.2 OctoWS2811 Adaptor. but in my case, it does not work well. Some of buffer just not fast enough to transmit.will cause LED lights flicking or runing ramdon effects. 
 
 13. Color order<br />
 	Not all LED strip lights are in RGB color order , it is alwasy wise to ask manufactor for more details.
	
	



           
