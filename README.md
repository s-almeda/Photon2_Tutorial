## Hands-on Photon2 Tutorial

**This tutorial created by Sudhu Tewari 2023**
 - from materials originally created by: Michael Shiloh and Judy Castro for *Teach Me To Make*

#### Tutorial overview
The tutorial will focus on getting you up and running with Particle Photon2 quickly, so that you will understand the basic procedures for working with your [Particle Photon2](https://docs.particle.io/photon-2/)

We will cover how to connect your Photon2 to your laptop; how to understand, modify, and write programs (aka code); how to connect input devices and sensors to your Photon2 and read them from a program; and how to connect actuators (LEDs, motors, speakers) and control them from a program. Other topics will be covered as interest dictates and time permits.

***
#### What is the Photon2 anyway?
 - Read about the Photon2: https://docs.particle.io/reference/datasheets/wi-fi/photon-2-datasheet/#functional-description

<!-- - Photon2 GPIO pins and ports: https://docs.particle.io/reference/datasheets/wi-fi/photon-2-datasheet/#pin-markings -->

 ![photon-2-pinout](/images/photon-2-pinout.svg)

- Getting Started Dashboard: https://docs.particle.io/getting-started/getting-started/
***
### Start Here

<!--- - Install IDE~  -->

#### 1: Initial Set Up (first time only)
- create a Particle account [particle.io/signup](https://login.particle.io/signup)
- follow the steps at [setup.particle.io](https://setup.particle.io/)
  to get your Photon2 set up

#### 2: Configure Wi-Fi (do this at home)
- in order to get started it's easiest if you are connected to wifi
  <!-- This isn't totally true, you _can_ work with the Photon2 not connected to Wi-Fi _BUT_ it's a bit complicated and we're aiming for simple and easy in this tutorial.-->
  - luckily Particle has made it easy to configure wifi for Photon2
- Navigate to [Particle Configure Wi-Fi](https://docs.particle.io/tools/developer-tools/configure-wi-fi/) page
  - follow the prompts to enter your wifi credentials
     - Wi-Fi Network name
     - password

### Configure Photon 2 to connect with Cal IoT Wi-Fi (Do this at home -> required to use your Photon2 on campus)
- In order to use Cal Wi-Fi with our Photons we must register our device with the Berkeley IoT Wi-Fi network (using its MAC address)
- In order to register the device we must first obtain the MAC address of our device

#### 3: Get Mac Address (-> do this at home)
- The code below asks the device to print its own MAC address (to serial):
- First we need to flash the code to our device.
- go here -> [GetMacAddress.ino](https://go.particle.io/shared_apps/6507d59801c67400099a4ce3) (right-click: Open Link in New Tab)
  - or copy the code below into the [Particle Web IDE](https://build.particle.io/build/)
```
void setup() {
  Serial.begin();
}

void loop() {
  byte mac[6];
  WiFi.macAddress(mac);
  for (int i=0; i<6; i++) 
    Serial.printf("%02x%s", mac[i], i != 5 ? ":" : "\r\n");
  delay(1000);
}
```
<!-- ![WebIDE_01](/images/WebIDE_01.png)-->

- We 'll use the [Particle Web IDE](https://build.particle.io/build/) to flash this code to our device.
  - reveal your code sidebar by clicking the <> icon on the left
    
![WebIDE_RevealCode](/images/WebIDE_RevealCode2.png) 

- copy this app to your own file space
    
![WebIDE_CopyApp](/images/WebIDE_CopyApp2.png)  

  - You should see your device name in the lower right of the Web IDE window

![WebIDE_01_Device](/images/WebIDE_01_Device.png)

- Click on the lightning bolt icon to flash the code to your device

![WebIDE_01_Flash](/images/WebIDE_01_Flash.png)

- you should see some evidence of activity.
  - you'll see a spinning arrow animation where the lightnining bolt was
  - the RGB LED on your Photon 2 will blink various colors
  - text at the bottom of the screen will let you know if the flash is successful
- It's working!!
  - but why can't we see anything happening?
- Use the [Particle USB serial debug log](https://docs.particle.io/tools/developer-tools/usb-serial/) to monitor the serial output. (right-click: Open Link in New Tab)
- BAM! There's your device's MAC address
- ```XX:XX:XX:XX:XX:XX```
  
<!-- ![USBserialLog_MACaddress2](/images/USBserialLog_MACaddress2.png)--> 
- Copy this address somewhere safe and bring it with you to campus so that you can get onto the IoT network

#### 4: Register MAC Address with Berkeley IoT Wi-Fi (-> do this anywhere)
- Navigate to the [UC Berkeley Wi-Fi Portal](https://portal.berkeley.edu/people/wifi_access)
- under "Berkeley-IoT Wi-Fi Network Devices" click on Manage devices
- click ```Create New```
- enter in the MAC address you copied in the earlier steps
- issue the device a name
- __copy your Berkeley-IoT password somewhere SAFE so that you will remember it!__
   - this password is only shown once. It will never be shown again...   

#### 5: Connect Photon2 Berkeley IoT Wi-Fi (-> do this on campus)
- Navigate to: https://docs.particle.io/tools/developer-tools/configure-wi-fi/
  - follow the prompts to enter your wifi credentials
     - Wi-Fi network name (Berkeley-IoT)
     - Berkeley-IoT password (generated by UC Berkeley portal in the previous step)'

       
***
### Alternate methods of connecting to Wi-Fi exist! 
- Install VSCode: [TDF-Software: Installing](https://github.com/Berkeley-MDes/desinv-202/wiki/TDF-Software:-Installing)
- Connect to Berkeley IoT Wi-Fi: [Particle: connecting a Photon 2 to the UCB IoT network](https://github.com/Berkeley-MDes/desinv-202/wiki/Particle:-connecting-a-Photon-2-to-the-UCB-IoT-network)

****
### Connected to Berkeley IoT Wi-Fi? Let's continue!

- To keep things simple we'll begin writing code (apps) in the [Particle Web IDE](https://build.particle.io/build/new)

- Later on we'll use VSCode to get into the really fun stuff.


### 7: Hello world -> Is this thing  on?
- Copy the code below into a new app or 
- navigate to this page: [HelloWorld.ino](https://go.particle.io/shared_apps/6507e44623d6c200096a1253) and copy the app.
```
/*
  Hello World
  A "Hello, World!" program generally is a computer program that
	outputs or displays the message "Hello, World!".
	Such a program is very simple in most programming languages,
	and is often used to illustrate the basic syntax of a programming language.
	It is often the first program written by people learning to code
*/

void setup() {
    //initialize serial communications
    Serial.begin();
}

void loop() {
    //send 'Hello, world!' over the serial port
    Serial.println("hello World");
    //wait 1 second between messages so we don't drive ourselves crazy
    delay(1000); 
```

- The Serial commands asks the Photon2 to send a message to your laptop. In order to see this message you'll need to use the [Particle USB serial debug log](https://docs.particle.io/tools/developer-tools/usb-serial/) to monitor the serial output. (right-click: Open Link in New Tab)

  - a little code anatomy:

   - the ``setup()`` function is called when a sketch starts.
     - Use it to initialize variables, pin modes, start using libraries, etc.
     - The ``setup()`` function will only run once, after each powerup or reset of the Photon2 microcontroller.

  - The ``loop()`` function does precisely what its name suggests, and loops consecutively through your list of instructions.
    - Photon2 only executes one instruction at a time.
  - ``//`` indicates a comment line meant for humans to read but ignored by the compiler that turn this text into machine code.
    - code editors will recognize this and format the text accordingly

- More on specific functions and variables soon! Let's make something happen in the real world first.

 
***
## 8: BLINK
use the Particle Tutorial!

https://docs.particle.io/getting-started/hardware-tutorials/hardware-examples/

***
### Connecting to your Microcontroller - Pinouts
In order to connect inputs or outputs to your microcontroller you need to know where the GPIO (general-purpose input/output) pins are!

 - [Photon2 GPIO pins and ports](https://docs.particle.io/reference/datasheets/wi-fi/photon-2-datasheet/#pin-markings)

 ![photon-2-pinout](/images/photon-2-pinout.svg)

***
### Using a solderless Breadboard to connect your microcontroller to other things (LEDs, motors, speakers, sensors, etc.) 
- partial duplicate information of Particle's ["Blink an LED hardware examples"](https://docs.particle.io/getting-started/hardware-tutorials/hardware-examples/)

In order to connect inputs or outputs to your microcontroller you need to have a way of making electrical connections!

The Solderless Breadboard
- [How to use a Breadboard tutorial](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard)
- [Breadboard connections](http://wiring.org.co/learning/tutorials/breadboard/)

![Breadboard](/images/Breadboard_st.jpg)

![Breadboard underside](/images/BreadboardUnderside_st.jpg)

![breadboard_perfBoard_bottom.jpg](/images/breadboard_perfBoard_bottom.jpg) 

***
### 9: Use the breadboard to add an external LED.
- LEDs must always be used with resistors so they don’t burn out.
  - The resistor value can be anywhere from 100 ohm to 1k ohm.
    - The lower the resistance, the brighter the light.
    - Evil Mad Scientist explains it well [here](https://www.evilmadscientist.com/2012/resistors-for-leds/)
  - Resistor Color Code!
    - [Learn the Resistor Color Code in in 5 minutes](http://www.resistorguide.com/resistor-color-code/)

    ![ResistorColorCode](/images/ResistorColorCode.png)

- LEDs are polarized
  - [identifying LED polarity](https://www.youtube.com/watch?v=SRDgNR_yCms)

  ![led_example](/images/led_example.png)

- Connect the anode (+) of the LED to pin 17 and 

- Connect the cathode (-) of the LED the anode to one leg of a 220Ω (ohm) resistor
  - 220Ω = red - red - brown - gold

- Connect the other leg of the resistor to ground (GND)

Here’s a picture showing how to connect the LED and resistor on the breadboard:

![Photon2_LED](/images/Photon2_LED.jpg)

Here is another view of this circuit, breadboard view in [Fritzing](https://fritzing.org/):

![Photon2_LED_bb](/images/Photon2_LED_bb.png)

Here is another view of this circuit, the schematic diagram:

![Photon2_LED_schem](/images/Photon2_LED_schem.png)


find example code here -> [External LED Blink app](https://go.particle.io/shared_apps/650b9c3023d6c200096a1d30) (right-click: Open Link in New Tab)

- copy this app to your own file space
  
- flash the app to your hardware

You should find that the external LED blink on and off just like the onboard LED was doing.

Yay!

_Can we blink both LEDs?_

__Yes!!__

### 10: Light up internal AND external LEDs
find example code here -> [Internal/External LED Blink app](https://go.particle.io/shared_apps/650b9c1901c67400099a5769) (right-click: Open Link in New Tab)

- copy this app to your own file space
  
- flash the app to your hardware

internal and external LED should blink alternately!

![P2T_Blink_IntExt](/images/P2T_Blink_IntExt.gif)

<!-- 
![P2T_Blink_IntExt](https://github.com/loopstick/Photon2_Tutorial/assets/33500510/9f6dc5d2-c129-45d5-ab98-e92e16a1e93c)
-->


***
### 11: How to use a sensor: analogRead() 
- partial duplicate information of Particle's ["Blink an LED hardware examples"]
  - but we're going to do it now withOUT sending data to the cloud because each packet of data we send to the cloud counts towards total monthly use and over a certain amount we have to pay for the data we're sending...

- Don't remove the LED and resistor from the last step

- Connect a LDR (light dependent resistor) anywhere on the breadboard

- Connect one leg of the LDR to 3.3V

- Connect the other leg of the LDR to A1 (analog input 1) AND one leg of a 10kΩ (kilo-ohm) resistor
  - 10kΩ = brown - black - orange - gold

- Connect the other leg of the 10kΩ resistor to ground (GND)

like this:
![Photon2_LED_pic](/Fritzings/Photon2_LED_LDR/Photon2_LED_pic.jpg)

![Photon2_LED_LDR_bb](/Fritzings/Photon2_LED_LDR/Photon2_LED_LDR_bb.png) 

Once you've got the circuit built head over to the Web IDE and let's ~write~ copy some code:

find example code here -> [P2T_LED_LDR_V1](https://go.particle.io/shared_apps/650bb0ad23d6c200156a19c5)  (right-click: Open Link in New Tab)

- copy this app to your own file space
  
- flash the app to your hardware

What happens?

hint: Remember the [USB Serial Monitor](https://docs.particle.io/tools/developer-tools/usb-serial/) from way back when we had to find the Mac address of our device?

![P2T_LED_LDR_v1](/images/P2T_LED_LDR_v1.png)

uh oh, that doesn't look right.

_what's wrong?_   <br><br>

Try this... find example code here -> [P2T_LED_LDR_V2](https://go.particle.io/shared_apps/650bb0ad23d6c200156a19c5)  (right-click: Open Link in New Tab)

- copy this app to your own file space
  
- flash the app to your hardware


***
 
***

## Want More?

# There are a ton of great examples built in to the Arduino IDE

Download and install the standalone [Arduino IDE](https://www.arduino.cc/en/software) or try out the [Arduino Web IDE](https://create.arduino.cc/editor/loopstick/46f2f8c3-a40e-469e-bddd-e2673161a1c2)

Try copying from the Arduibno examples and porting to Photon2







