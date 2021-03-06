# About this fork

This fork is to capture the changes to RFSniffer so that it can be run as a daemon in a node-red flow. The node-red flow can then respond to the 433MHz code received and do cool stuff. Note that you need compatible hardware attached to your Raspberry Pi in order to be able to receive 433MHz signals.

See here for the origins of these changes [https://groups.google.com/forum/#!searchin/node-red/weather$20433/node-red/7EhmxjwnrOc/7ix9vS_In1gJ]

I've also attempted to merge the various updates to RCSwitch, such as the changes made for NinjaBlocks (WT450 support) etc. Whilst this all compiles, I don't have the hardware to test if it actually works.

Additionally, I've modified RFSniffer and RCSwitch to use a proper thread-safe queue rather than a hard loop when receiving 433 input. This stops RFSniffer running at nearly 100% cpu and gets it down to about 30% on my rp1b.

# About 433Utils

433Kit is a collection of code and documentation designed to assist you in the connection and usage of RF 433MHz transmit and receive modules to/with your Arduino and Rapberry Pi.

It consists of two main sections- Arduino sketches and Raspberry Pi command line utilities.  You'll find those in appropriately named folders.

## Requirements

1. An Arduino (two is ideal)
2. A Raspberry Pi
3. The git commandline tool, on both your Arduino IDE machine and your RPi
4. RF 433MHz modules; a transmitter and receiver
5. A Ninja Block and account

## Installation

### Arduino\_Sketches

Place the Sketches in your normal Arduino Sketches location.  Install any required libraries:

1. Servo motor library (Should be included in the Arduino IDE)
2. _Arduino_ [rc_switch](http://code.google.com/p/rc-switch/) library

### RPi\_utils

From your RPi, clone this archive:

1. git clone git://github.com/ninjablocks/433Utils.git

2. cd 433Kit/RPi\_utils

3. (As per the original rc_switch distribution) Follow the instructions to install the  [wiringpi](https://projects.drogon.net/raspberry-pi/wiringpi/download-and-install/) library.
After that you can compile the example programs by executing *make*. 

## Usage

### send 
```sudo ./send systemCode unitCode command```

This command is unaltered from the original rc\_switch distribution.


### codesend 
```sudo ./codesend decimalcode```

This command uses a single Ninja Blocks compatible decimal code.  Sniff out codes using the RF\_Sniffer.ino Arduino sketch, included in this distribution.

You can use codesend to control the wireless servo on your Arduino.
(The sketch is Servo\_Receive\_Demo.ino)

I've implemented the most bare-bones of controls here.  Try:

```sudo ./codesend 500000 # to move the servo up```

```sudo ./codesend 500001 # to move the servo down```

```sudo ./codesend 500002 # to move the servo left```

```sudo ./codesend 500003 # to move the servo right```

```sudo ./codesend 500004 # to move the servo home```

## Issues

Due to limitiations in the implementation of interrupt-driven routines in the underlying RCSwitch library, it is currently not possible to use both the send and receive functionality within the one program.  
