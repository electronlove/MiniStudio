For starters, first follow the installation instructions for https://github.com/uozuAho/pilights

Several files need to be modified

File name **flask-app.py**
  Set the number of LEDS that you will be using.  The default is set to "NUM_LEDS = 8".  
  For the Adafruit NeoPixel ring, e.g. https://www.adafruit.com/product/2853, set NUM_LEDS = 12 for a single ring OR 24 for 2 rings in series

File name **neopixels.py**
  Under "class NeoPixels:":
  **CHANGE** 
def __init__(self, num_leds, output_pin=18, led_freq_hz=800000, dma=5,
                 invert=False, brightness=255):
        self.pixels = Adafruit_NeoPixel(num_leds, output_pin, led_freq_hz, dma,
            invert, brightness)
        self.pixels.begin()

  **TO**   
  
  def __init__(self, num_leds, output_pin=18, led_freq_hz=800000, dma=5,
                 invert=False, brightness=255, channel=0, strip=ws.SK6812W_STRIP):
        self.pixels = Adafruit_NeoPixel(num_leds, output_pin, led_freq_hz, dma,
            invert, brightness, channel, strip)
        self.pixels.begin()

  **This is to specify the correct setting for Adafruit #2853 which uses the RGBW pixels**
  
  File name **index.html***
  Replace with index.html in this directory
  
