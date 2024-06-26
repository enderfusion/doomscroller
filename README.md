# Pairing Instructions:

* Verify that the blue LED can be seen blinking when looking into the USB-C connector. This indicates that the DoomScroller is in discovery/pairing mode with no active connection. If no light is seen, battery probably needs recharging. 

	![AndrewMcCalipsVideo-Jun22024-ezgif com-crop](https://github.com/enderfusion/doomscroller/assets/18008735/13dc09b1-36b5-4126-853e-91fdfe486cd4)

* Swipe the magnet once over the back PCB lid. The hall sensor sits directly under the letter "C". This resets the board and puts the NRF chip into pairing mode

 	![image](https://github.com/enderfusion/doomscroller/assets/18008735/b828ad0d-6260-4293-a379-4f88b7064f22)

* Verify that the blue light can be seen in USB-C

* Pair device on your Android / PC

* *VERY IMPORTANT* Perform a second magnetic reset swipe. This is critical for Android to properly recognize the device. Eventually we'll move this into firmware...





<br/> 
<br/> 
<br/> 
<br/> 

## Troubleshooting and FAQ

### Not scrolling after pairing ###
* Perform a magnetic reset on the back PCB before AND after pairing. 

### Device not working with iPhone: ###
* You're out of luck until our cracked squad of software engineers
  gets iOS functionality working. Talk to Andrew about getting store
  credit or a return.

  
### Can I adjust scroll speed and responsiveness? ###
* Absolutely! There are a set of user adjustable values in the top of
  the main .ino file. Most important factors are scale and max
  velocity. Suggested values are provided as a starting point.
* The buffer lengths of the moving averages buffers can also be
  adjusted. A shorter buffer will result in a more snappy response,
  but will have more velocity ripple.

### It appears to be accidentally clicking 
* This is the main bug I've been battling with the v0.1 firmware. It
  happens mostly on Twitter, where the entire post is clickable. The
  issue stems from using a single finger click and drag approach. If a
  scroll event starts and stops within a 40px diameter on the screen,
  the kernel registers it as a click instead of a scroll. This tends
  to happen when the wheel is suddenly jolted. It is a balancing act
  between a long moving average buffer to filter out these transients,
  and the responsiveness of a short buffer. The ideal fix for this is
  to implement a two finger HID event. the PC driver doesn't suffer
  from this issue because its using the enhanced resolution scroll
  wheel driver of the Microsoft surface dial.

### Connection is laggy: ###
* If there is latency and chunky feeling scroll action, there is a
  chance that the Bluetooth pairing rate has been negotiated
  incorrectly. BLE operates at a range of update rates, depending on
  what the phone determines. I've noticed that if you're pairing while
  the device is in idle mode, it can put it into a low rate mode which
  results in chunky scrolling. Best practice is to perform a single
  swipe magnetic swipe right before pairing

### Entering Bootloader Mode: ###
* Perform a double magnetic tap on the "C" letter of the PCB to enter
  bootloader mode, mimicking a double-click of the onboard reset
  switch. This will present the device as a removable USB drive for
  firmware updates.

<br/> 
<br/> 
<br/> 
<br/> 

# Flashing custom firmware

## Step 1: Set Up the Seeed nRF52840 on Arduino IDE
Install the Arduino IDE: Download from the official Arduino website.

Add the Seeed Board to Arduino IDE:

Go to File > Preferences, and insert this URL into "Additional Boards Manager URLs":

	https://files.seeedstudio.com/arduino/package_seeeduino_boards_index.json

Navigate to Tools > Board > Boards Manager, search for "Seeed nRF52 Boards", and install it.

Choose Tools> Board > Seeed nRF52 Boards > Seeed XIAO nRF52840 Sense

You may also need to install adafruit-nrfutil:

    pip3 install adafruit-nrfutil

## Step 2: Install Required Libraries
MPS MagAlpha and MovingAveragePlus Libraries are needed.

Open Sketch > Include Library > Manage Libraries....

Search and install "MPS MagAlpha" and "MovingAveragePlus".

## Step 3: Set Debug Level

Tools > Debug Level > Level 2 (Full Debug)

## Step 4: Upload Code
Connect the Doomscroller via USB C.  If the device isn't recognized as
a COM port, perform a magnetic triggered reset by swiping a magnet
across the back PCB before uploading your code.

Usually the sketch starts running after flashing.  If it doesn't,
perform a magnetic reset right afterwards.


