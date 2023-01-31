# Moteino USB as RFM69Pi
Open Energy Monitor sensors use HopeRF RFM69CW radios to send data to emonHub, where it is processed, stored, and visualized with emonCMS. (https://openenergymonitor.org/)

The RFM69Pi hat is the gateway that receives the radio packets and writes them via serial to emonHub. The device is comprised of an ATmega328 with RFM69CW radio. Technical documentation and firmware for RFM69Pi can be found here. https://docs.openenergymonitor.org/emonbase/rfm69-pi.html

Moteinos from Low Power Lab (lowpowerlab.com) are also ATmega328 devices and they support several different HopeRF transceivers, including the RFM69CW. They have all the necessary parts to act as an RFM69Pi, and can communicate via serial USB to emonHub.

# JeeLib Classic and LowPowerLab Radio Libraries
New Open Energy Monitor devices such as the emonTx v4 use a radio library developed by Felix Ruso at Low Power Lab. Older devices use the JeeLib library from JeeLabs. The RFM69Pi has firmware for each radio library, but is only compatible with one or the other at any given time. Moteinos can also run both versions of the firmware. Using an RFM69Pi and Moteino USB (or two Moteino USBs) it is possible to listen to both kinds of radio packets at once.
![Moteino USB RFM69Pi](https://user-images.githubusercontent.com/17953028/215905802-72d38021-0e80-4fe2-b613-6ec0da3623e4.png)

Similarly, new emonBases are shipped with the RFM69SPI hat, which occupies the GPIO instead of the serial-based RFM69Pi. Add a Moteino USB and the emonBase can listen to JeeLib packets as well.
![Moteino USB RFM69Pi JeeLib](https://user-images.githubusercontent.com/17953028/213807911-efee877b-3453-48ba-8c6d-aa49f9e7cad3.png)

# Flashing Moteino USB with RFM69Pi Firmware
In this repository are the current versions of both the LowPowerLab and JeeLib firmware. The .hex files have been compiled to work with the Moteino USB and have the bootloader included for ongoing serial programming, and .ino files can be flashed to the Moteino USB with Arduino IDE.

RFM69Pi firmware is released by Open Energy Monitor as compiled binaries (.hex) and also as source code compatible with Arduino IDE (.ino). The compiled binaries are not compatible with Moteino USB because the two devices use diffent oscillators (8mhz internal in the case of RFM69Pi, 16mhz external for Moteino USB). So the easiest way to flash the Moteino USB is to use Arduino IDE and upload the sketch as you would to an Arduino Uno. Or you can use the compiled binaries in this repository, which are compatible with Moteino USB.

The OEM repositories are here:
https://github.com/openenergymonitor/RFM2Pi/tree/master/firmware/RFM69CW_RF_Demo_ATmega328
https://github.com/openenergymonitor/emonBase_rfm69pi_LPL

# Notes
It works well to use a USB A Male to Micro Male adapter to connect the Moteino to the emonBase or emonPi.

After upload, use the Serial Monitor (or other method) to set the node number of the transciever. This sketch listens to serial input for commands. For example, send 14i to set the node to 14. 

Add the following entry to emonHub. emonCMS > Setup > emonHub > Edit Config > add the text below > save.

<code>
     [[SerialDirect]]
     Type = Type = EmonHubJeeInterfacer
      [[[init_settings]]]
           com_port = /dev/ttyUSB0 
           com_baud = 38400
      [[[runtimesettings]]]
           pubchannels = ToEmonCMS,
           subchannels = ToRFM12,
</code>
