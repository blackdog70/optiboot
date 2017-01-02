## Optiboot Bootloader for Arduino and Atmel AVR ##

![http://optiboot.googlecode.com/files/optiboot.png](http://optiboot.googlecode.com/files/optiboot.png)

Optiboot is an easy to install upgrade to the Arduino bootloader within Arduino boards. It provides the following features:

  * Allows larger sketches. Optiboot is a quarter of the size of the default bootloader, freeing 1.5k of extra space.
  * Makes your sketches upload faster. Optiboot operates at higher baud rates and has streamlined programming.
  * Adaboot performance improvements. Optiboot runs your sketches sooner, with no watchdog issues.
  * Compatible with 168 and 328 Arduinos including Lilypad, Pro, Nano
  * Believed to work with ATmega1280 ("Mega"), ATmega644 ("Sanguino"), and ATmega1284 ("Mighty")
  * Supports several additional AVR chips (ATmega88, ATmega32)

Optiboot is now installed by default on the Arduino Uno. It can be installed on all older mega8, 168 or 328 based Arduinos.

## Additional Documentation
More detailed documentation is being added (slowly) to the [repository wiki](https://github.com/Optiboot/optiboot/wiki).

## Notes on IDE Version compatability
Optiboot in "compatible", it a loose sense, will all versions of the Arduino IDE.  It was originally written at about the same time as v1.0, and has some "quirks" that date back to that timeframe.  Most significantly, install procedures and locations change between releases, and the ability to compile optiboot using only the tools installed with the IDE broke in the v1.5 timeframe.

## To install into the Arduino software ##
You do NOT need to "install" Optiboot if you are trying to update an installed platform that already uses some form of Optiboot.  In that case, you should probably just find and replace the existing .hex files from the platform support directories.  Using the optiboot "install" procedure does not install any cores or variants, so it is only useful for cpus that are already supported by the standard Arduino core. (or, if all you want to do is install bootloaders.)

The following instructions are based on using the Arduino "Board Manager", present in IDE versions 1.6.5 and later.

  1. Find the desired Optiboot release on the [Optiboot Rlease page] (https://github.com/Optiboot/optiboot/releases).
  2. Use the "Copy Link" feature of your browswer copy the URL of the associated **.json** file.
  3. Paste this url into the "Additional Boards Manager URLs" field in the Arduino IDE "Preferences" pane. (Separate it from other URLs that might be present with a comma.)
  4. After closing the preferences pain, the **Tools/Boards/Boards Manager** menu should include an entry for that version of Optiboot.  Select that entry and click the **Install** button.

For addition installation information, see the [Optiboot AddingOptibootChipsToIde Wiki page] (https://github.com/Optiboot/optiboot/wiki/AddingOptibootChipsToIde)


<!---
  1. Download the latest using Git or the Zip download feature of GitHub.  If you download as a zip, also extract it.
  1. You will need to be using a recent version of the [Arduino environment](http://arduino.cc), version 18 or later.
  1. Create a 'hardware' directory inside your sketches folder.
  1. Copy the optiboot directory into the hardware directory.
  1. Restart the Arduino software. New boards will appear in the Tools>Board menu.
--->

## To burn Optiboot onto an Arduino board ##
  1. Select the appropriate Optiboot board type (or non-Optiboot if you want to change back)
  1. Connect your Arduino to an ISP programmer [[Installing]]
  1. Use the 'Burn Bootloader' item in Arduino.
  1. You can then upload sketches as normal, using the Optiboot board type.
 
## RS485 HALF DUPLEX support

Only for hardware serial.
PINB1 is used to enable/disable transmission and it is fixed in optiboot.c (not yet an option in Makefile).

## Software reset support

Byte 0 on EEprom is used to recognize software reset, so it is possible to start bootloader directly from arduino apps.

```
void start_bootoader()
{
	EEPROM.write(0, 0); // Set start bootloader
	wdt_reset();
	wdt_enable(WDTO_15MS);
	exit (1);  // loop forever
}
```
The bootloader will start app after a **STK_LEAVE_PROGRAM**

## Device serial number

**SN_MAJOR** and **SN_MINOR** are used to set a serial number for arduino device.
Serial number is read-only can be inspected with new custom STK codes 0x92 and 0x93.
Serial number is intended as unsigned int where SN_MAJOR is the MSB and SN_MINOR is the LSB.

i.e. make ENV=sloeber atmega328_pro8 BAUD_RATE=19200 SN_MAJOR=1 SN_MINOR=1 

----

> Although it has evolved considerably, Optiboot builds on the original work of Jason P. Kyle (stk500boot.c), [Arduino group (bootloader)](http://arduino.cc), [Spiff (1K bootloader)](http://spiffie.org/know/arduino_1k_bootloader/bootloader.shtml), [AVR-Libc group](http://nongnu.org/avr-libc) and [Ladyada (Adaboot)](http://www.ladyada.net/library/arduino/bootloader.html).

> _Optiboot is the work of Peter Knight (aka Cathedrow). Despite some misattributions, it is not sponsored or supported by any organisation or company including Tinker London, Tinker.it! and Arduino._  
> Maintenance of optiboot was taken over by Bill Westfield (aka WestfW) in 2011.
