phonebooth-lights
=================

What it does:

Allows everyone to change colors and animations on a string of multi-colour LED lights in the Spantree Phonebooth.


How it works:

![Alt text](/../master/docs/overview.png?raw=true "Overview")

The user interface resides on the browser in an HTML file. The HTML has Javascript to connect to a websocket interface.


The controller listens on port 7890 for HTTP and websocket requests.
The JS examples in this case connect to that IP and push pixel data to it (in RGB) 
which sends it to the LED string.
