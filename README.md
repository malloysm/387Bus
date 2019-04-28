# Bus - LED Dance Floor

**Background**: Dance Floor has 9 panels each with 9 RGB LEDs, which results in a total of 81 LEDs. Each LED is controlled by 3 bits so there will be 243 bits in total. It is assumed each panel is about 1 foot square.

**Topology**: Ring - reduces the amount of wire needed to connect the LEDs and reduces the distance the distance for the data to travel. Connecting the LEDs with twisted pair cables will allow high speed data transmission (Cat5 - 100Mbps) [How Twisted Pair Works](https://sites.google.com/site/markgurries/home/dcc-general-best-practices/wiring-planing/twisted-pair-wiring/how-twisted-pair-wiring-work) The LEDs will likely be linked in a Serpentine pattern, so estimating each row (9 total) at 3ft requires 27ft of wire. I will round this up to 10 meters.

**Protocol**: SPI - capable of very high speed data communication. However, as the Dance Floor is a student project, they would likely want to use an Arduino which is only capable of 8Mbps data transfer (as the Master device). [Wikipedia: SPI](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface#Data_transmission), [Arduino: SPI](https://www.arduino.cc/en/reference/SPI)

**Time**: The transmission time is limited by the Arduino and not the cable, so to send 243 bits - 243b/8Mbps = 30.4us. Propagation speed in copper is about 2x10^8 meters/s, so propagation delay is 10m/200000000m/s = 0.05us. Thus, it will take about 30.45us for data to be received by the last LED connected in the Ring topology. [Wikipedia: Transmission Time](https://en.wikipedia.org/wiki/Transmission_time)

**Controlling the LEDs**: For this I would cascade shift registers, such as the 74HC595 ([How to Cascade Shift Registers](http://www.learningaboutelectronics.com/Articles/Cascade-shift-registers.php)). One register can receive 8bits, so a total of 31 shift registers would be needed to control all 81 LEDs. The Arduino would send a string of data to the 1st register and then the data would cascade all the way to the 31st register. Once the data reaches the 31st register the Arduino would set the latch pin high to allow the registers to send the data to the output (the LEDs) simultaneously. An external power supply would be necessary to power the LEDs + Shift Registers.

[A cool interactive LED Dance Floor I found](https://hackaday.com/2017/03/24/daunting-interactive-led-dancefloor-build-is-huge-win/)
