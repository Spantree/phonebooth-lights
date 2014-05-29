phonebooth-lights
=================

## What it does:

Allows everyone to change colors and animations on a string of multi-colour LED lights in the Spantree Phonebooth.


## How it works

![Alt text](/../master/docs/overview.png?raw=true "Overview")

### UI

The user interface resides on the browser in an HTML file. The HTML has Javascript to connect to a websocket interface. The Javascript sends pixel data as RGB (red, green blue) values for each of the 60 pixels/LEDs of the LED lights string. It can push a different colour to each pixel and create animations, or change them all at once.

### Web Server 

On the other side of that Websocket is a little Linux router, running OpenWRT Linux (http://openwrt.org/). It connects to Spantree office's wifi as a client and gets its own IP. This is the IP the Websocket connects to, on port 7890. The router itself is a TP-Link WR703n (http://wiki.openwrt.org/toh/tp-link/tl-wr703n) with an Atheros SoC (System on a Chip) with: a 400 MHz MIPS CPU, 32 MB of RAM, 4 MB of flash memory, wifi, and USB.

The WR703n router is running a special software that listens for HTTP and websockets on port 7890. That software is called Fadecandy (https://github.com/scanlime/fadecandy) and was written by Micah Elizabeth Scott. She also created the USB-based Fadecandy controller.

### Low level lights control - USB

The WR703n router has to somehow send the pixel data it receives over its websocket interface to the actual LEDs. It does this using the Fadecandy controller (https://www.adafruit.com/products/1689) which is a physical USB device. It is connected to the WR703n's USB port. It has 8 channels that can control 64 pixels each. We are only using channel 0 for our 60-pixel LED string.

The special Fadecandy server-side software knows also how to talk to the USB device and does so when it received pixel data over its websocket connection. The USB device then sends a special protocol to the LED's themselves to change each of their colour values.

### LEDs

Each pixel in the string is not actually just an LED, but rather 3 LEDs (Red, Green, and Blue) as well as a tiny microcontroller, built into each of these pixels! It is designed to take RGB colour values for its pixel, then push the rest of the pixel data-stream to the rest of the pixels down the string. That way each pixel takes only its values and passes on the rest to the others after it. 
These particular LED pixel modules are produced by a company in China called WorldSemi (http://www.world-semi.com/en/) and are the WS2812 model. There is a datasheet for them here: https://www.adafruit.com/datasheets/WS2812.pdf


## Extending it

The color.html file shows how to change all the pixels' colours using a Javascript colour picker. 
The chase.html file demonstrates a basic animation example, where an LED 'chases' itself around the string.

There are more example in more languages in the Fadecandy repository: https://github.com/scanlime/fadecandy/tree/master/examples
