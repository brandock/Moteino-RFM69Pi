# Moteino-RFM69Pi
Open Energy Monitor JeeLib Classic RFM69Pi using a Moteino with RFM69CW Radio.

# Moteino USB as RFM69Pi
This sketch can be used with Moteino USB as a replacement (or addition to) the RFM69Pi hat.

It is exactly the JeeLib Classic RFM12Pi/RFM69Pi firmware, with the addition of instructions for using it with a Moteino USB.

Technical documentation and firmware for RFM69Pi can be found here. https://wiki.openenergymonitor.org/index.php/RFM69Pi_V3

This firmware used the JeeLib Classic radio library, compatible with the original firmware for emonTx V3 and emonTh.

You might use this if you convert your emonPi/emonBase to the LowPowerLabs radio library (compatible with emonTx V4) but need to hear from some devices via the JeeLib Classic format. For example, if you replace your RFM69Pi hat with the RFM69SPI hat.
![Moteino USB RFM69Pi JeeLib](https://user-images.githubusercontent.com/17953028/213807204-10e6be02-8a5d-44ca-a522-83c1fddec3ff.png)


# Notes
It works well to use a USB A Male to Micro Male adapter to connect the Moteino to the emonBase or emonPi.

After upload, use the Serial Monitor (or other method) to set the node number of the transciever. This sketch listens to serial input for commands. For example, send 14i to set the node to 14. 

Add the following entry to emonHub. emonCMS > Setup > emonHub > Edit Config > add the text below > save.

[[SerialDirect]]
     Type = EmonHubSerialInterfacer
      [[[init_settings]]]
           com_port = /dev/ttyUSB0 
           com_baud = 38400
      [[[runtimesettings]]]
           pubchannels = ToEmonCMS,
           subchannels = ToRFM12,
