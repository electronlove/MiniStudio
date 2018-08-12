For starters, first follow the installation instructions for https://github.com/uozuAho/pilights

Several files need to be modified

File name **flask-app.py**
  1. Add #!/usr/bin/env python
  2. Set the number of LEDS that you will be using.  The default is set to "NUM_LEDS = 8".  
  
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
                 invert=False, brightness=255, **channel=0, strip=ws.SK6812W_STRIP**):
        self.pixels = Adafruit_NeoPixel(num_leds, output_pin, led_freq_hz, dma,
            invert, brightness, **channel, strip**)
        self.pixels.begin()

  ALso change the relevant parts under **class NeoPixelsMock:** to include **channel** and **strip**

  **This is to specify the correct setting for Adafruit #2853 which uses the RGBW pixels**
  
  File name **index.html***
  Replace with new index.html in this directory
  
  **To make a system process**
  
  To autostart on boot:

1. Create a configuration file for neopixels.service:
- sudo nano /lib/systemd/system/neopixles.service

2. Enter the following information into the file:

[Unit]
- Description=NEOPIXEL WEB APP
- After=multi-user.target

[Service]
- Type=idle
- ExecStart=/usr/bin/env python /home/pi/pilights/flask-app.py **<--- Make sure the directory is correct.**

[Install]
- WantedBy=multi-user.target

3. Save and exit the editor.

4. Set permissions by:
- sudo chmod 644 /lib/systemd/system/neopixles.service

5. Configure the service
- sudo systemctl daemon-reload
- sudo systemctl enable neopixles.service

6. Reboot
- sudo reboot

7. Check the status of the running service
- sudo systemctl status neopixles.service

