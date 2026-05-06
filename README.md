# Raspberry Pi 12864 LCD library
AKA RPi-12864-LCD-ST7920-lib

## Example
Import the library from st7920.py. More documentation can be found there.
An example is as follows:
```
from st7920 import ST7920
import time 

display=ST7920()

display.initTextMode()

display.examples()
		
for x in range(0,1000):
	display.printStringTextMode(str(x),0,0)
	time.sleep(0.1)
```

The original files can be found in the "old" directory.

## Documentation
### Original functions
Notes:
* These translations are not very good.
* The Y position is inverted --- (0,0) is the top left.
* All the functions that have the use_memPlot argument, can use the memplot instead of plot; read self.memplot() explanation.
```
init()
   Basic GPIO port settings - just run it once at the beginning of the program

self.initTextMode()
  Switches the display to text mode. Necessary in order to use functions that use self.plot().

self.initGraphicMode()
  Switches display to graphic mode.

self.clearText()
  Deletes the content of the text part of the display (the graphics portion remains unchanged)

self.clearGraphic(pattern)
  Fills the entire contents of the graphical part of the display by the byte. When 0x00 is deleted, 0xFF will fill it with white dots, other values will fill the display with different vertical lines. The text part of the display remains unchanged.

self.clearDisplay()
  Performs both previous deletions at the same time. After returning the display is switched to text mode.

self.defineIcon(iconId, self.iconData)
  Defines one of four custom icons
  iconId = icon identifier number 0 to 3
  self.iconData = a variable that contains an array: 16x double-byte value

self.printIcon(iconId, x, y)
  The [x, y] coordinates print one of the four custom icons.
  iconId = Icon identifier number 0 to 3
  x = column 0 to 7
  y = line 0 to 3
```
#### Graphic font 8x8 pixels
```
self.printCharGraphicMode(code, x, y, invert)
  Displays one character with the ASCII code *code* at the the coordinates provided. x=0 to 15 (maybe 0 to 127?), y=0 to 63. When "inverse" = True, a dark self.printCharGraphicMode appears on a light background.

self.printStringGraphicMode(text, x, y, invert)
  Displays text (multiple characters) at (x,y) (parameters same as previous).
```
#### Inside display font
```
self.printCharTextMode(code, x, y) 
  Displays one character in text mode at [x, y].
  Code is in the range 1 to 127 (from 32 to 126 it is a classic ASCII)
  X is from 0 to 15
  Y is a line 0 to 3

self.printStringTextMode(string, x, y)
  Use large characters to display text on the display. Parameters are the same as for previous.

self.plot(posX, posY, style)
  At "posX" (0 to 127) and "posY" (0 to 63), it drawns, deletes, or inverts one pixel. If style = 0 , point deletion is performed, style = 1 is drawn, and style = 2 changes the point status on the display.

self.memplot(posX, posY, style)
  The same function as the previous "self.plot()", but the points do not appear directly on the display, only in the temporary storage space. This feature allows faster drawing. After use, however, it is necessary to transfer the contents of that temporary memory to the display using the "self.memdump()" function.

self.memdump()
  Transfer the memory contents to the display after using the self.memplot() command.

self.drawHorizontalLine(posY, fromX, toX, style, use_memPlot = 0)
  Drawing a simple horizontal line at a "supery" distance from the top edge with the ability to define the beginning and end of the line (variables "from" to "to"). The "style" parameter is the same as the memplot function.

self.drawHorizontalLine2(posY, fromX, toX, pattern)
  Faster drawing of the horizontal line. In this case, the parameters "fromX" and "toX" are in the range 0 to 7 (they are sixteen pixels on the display). Therefore, the line can begin and end only at the coordinates on which the icons are printed. The minimum length of such a line is 16 points. The "pattern" parameter specifies the style of the line. Depending on the individual bits of that parameter, you can set the line full, dashed, dotted, dashed ...

self.drawVerticalLine(posX, fromY, toY, pattern, use_memPlot = 0)
  Vertical line at arbitrary coordinates.
  posX = X line coordinates in the range 0 to 127
  fromY, toY = y coordinates of the beginning and end of the line in the range 0 to 63
  The "pattern" parameter is the same as in the previous case.

self.loadBMP12864(fileName)
  Load a two-color image from the file into the display. Beware of the correct file format!

self.sendByte(rs, byte)
  Sends 1 byte to the display. The data (1) or the command (0) register is selected using the "rs" parameter.

self.send2bytes(rs, byte1, byte2)
  Sends 2 bytes at a time. The data (1) or the command (0) register is selected using the "rs" parameter.
```

### New functions
They are not present on the original code. They aren't so complicated, but are handy.
```
drawGenericLine(fromX, fromY, toX, toY, style = 1, use_memPlot = 0)
   Draws a line from and to the specified coordinates. Based on this code: http://itsaboutcs.blogspot.com.br/2015/04/bresenhams-line-drawing-algorithm.html

drawCircle(circleCenterX, circleCenterY, radius, startDegree = 0, stopDegree = 360, stepDegree = 1, style = 1, use_memPlot = 0):
   The arguments are self-explaining. Increasing stepDegree increases the speed of drawing, but may result in missing pixels.
   
drawRadiusLine(fromX, fromY, degree, radius, style = 1, use_memPlot = 0):
    Draws a line like a clock hand, where you enter the initial coordinate, the angle in degrees and the radius (the size of the line)

hideShowDisplay(state)
    Hide all active pixels on display, but doesn't remove them from memory. If you hide them (True or any value except 0), and then show them again (False or 0), all active pixels will show again, on the same place.

printString3x5(string, leftX, topY, rotation = 0, use_memPlot = 0):
    Prints a string with the font 3x5 pixels.
    rotation = 0: no change in the text.
    rotation = 1: text is turned 90degrees counter-clockwise (and keeps writing up)
    rotation = 2: text is turned 180degrees counter-clockwise (and keeps writing left)
    rotation = 3: text is turned 270degrees counter-clockwise (and keeps writing down)
    leftX and topY are the "top-left" position of the first char that you want to print.
```

## About the screen & wiring
You can find this on AliExpress or Amazon or anything; it is a 128x64 res LCD screen. They come in a variety of colors. This library is to assist with using the screen.

## Pinout
The following pinout sets the screen in serial mode, which is used by this project. It is adapted from the original project's st7920.py as well as a wiring diagram provided by [this repository](https://github.com/panahbiru/raspberrypi-12864LCD) (able to be found in /docs) and a datasheet referenced by that wiring diagram (also able to be found in /docs). If your screen is in parallel mode (PSB=5V or high or 1 or whatever) then your pins will be a bit different.

| LCD pin # | Pin name | Pin purpose | Connect to |
| --- | --- | --- | --- |
| 1 | VSS / GND | Ground | Ground |
| 2 | VDD / ? | Power supply | 5V. Technically can take 4.5V to 5.5V |
| 3 | VO | Maybe used to adjust contrast | Nothing, but maybe worth testing |
| 4 | CS | Chip select | 5V |
| 5 | SID | Serial data input | GPIO7 (pin 26) |
| 6 | SCLK | Serial clock | GPIO8 (pin 24) |
| 7 | D0 | Data bit 0 | Nothing |
| 8 | D1 | Data bit 1 | Nothing |
| 9 | D2 | Data bit 2 | Nothing |
| 10 | D3 | Data bit 3 | Nothing |
| 11 | D4 | Data bit 4 | Nothing |
| 12 | D5 | Data bit 5 | Nothing |
| 13 | D6 | Data bit 6 | Nothing |
| 14 | D7 | Data bit 7 | Nothing |
| 15 | PSB | Interface selection | For serial, set to ground |
| 16 | NC | Not connected | Nothing |
| 17 | RST | Reset (a brief 0 will reset display) | GPIO25 (pin 22) |
| 18 | VOUT | Voltage doubler output. Maybe adjusts contrast? | Nothing |
| 19 | BLA | "Underground anode". Powers backlight(?) | 5V; LEDs take 60mA) |
| 20 | BLK | "Underworld cathode". Serves as ground for backlight(?) | Ground |


## About the library
This library was originally published [on Github](https://github.com/ftzi/RPi-12864-LCD-ST7920-lib) as a translation of Czech code from [astromik.org](http://www.astromik.org/raspi/42.htm), largely translated with [Google Translate](https://translate.google.com/translate?hl=&sl=cs&tl=en&u=http%3A%2F%2Fwww.astromik.org%2Fraspi%2F42.htm). The website can be found on [the Internet Archive](https://web.archive.org/web/20160323175419/http://www.astromik.org/raspi/42.htm). Also check out [here](http://www.astromik.org/raspi/glcd12864-zw-a.jpg)(?).