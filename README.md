# Raspberry Pi 12864 LCD library
AKA RPi-12864-LCD-ST7920-lib

## Example & documentation
Import the library from st7920.py. Documentation can be found within that file.
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

## About the screen & wiring
You can find this on AliExpress or Amazon or anything; it is a 128x64 res LCD screen. They come in a variety of colors. This library is to assist with using the screen. Wiring can be found at recommended_wiring.pdf and is taken from [this repository](https://github.com/panahbiru/raspberrypi-12864LCD).


## About the library
This library was originally published [on Github](https://github.com/ftzi/RPi-12864-LCD-ST7920-lib) as a translation of Czech code from [astromik.org](http://www.astromik.org/raspi/42.htm), largely translated with [Google Translate](https://translate.google.com/translate?hl=&sl=cs&tl=en&u=http%3A%2F%2Fwww.astromik.org%2Fraspi%2F42.htm). The website can be found on [the Internet Archive](https://web.archive.org/web/20160323175419/http://www.astromik.org/raspi/42.htm). Also check out [here](http://www.astromik.org/raspi/glcd12864-zw-a.jpg)(?).