# PaPiRus
Resources for PaPiRus ePaper eInk display

# Enabling SPI and I2C interfaces on Raspberry Pi
Before using PaPiRus, do not forget to enable the SPI and the I2C interfaces.
You can enable the SPI by typing `sudo raspi-config` at the command line and then selecting `Interfacing options` > `SPI` and then selecting Enable. Without exiting the tool still in `Interfacing options` > `I2C` and then selecting Enable.

# Setup PaPiRus
```bash
# Run this line and PaPiRus will be setup and installed
curl -sSL https://goo.gl/i1Imel | sudo bash
```

# Getting Started
```bash
# Select your screen size
sudo papirus-set [1.44 | 1.9 | 2.0 | 2.6 | 2.7 ]
or
sudo papirus-config
# System will now reboot
```

# Manual Installation

#### Install Python API
```bash

# Install dependencies
sudo apt-get install python-imaging

git clone https://github.com/PiSupply/PaPiRus.git
cd PaPiRus
sudo python setup.py install    # Install PaPirRus python library
```

#### Install Driver (Option 1)
```bash
papirus-setup    # This will auto install the driver
````

#### Install Driver (Option 2)
```bash
# Install fuse driver
sudo apt-get install libfuse-dev -y

sudo mkdir /tmp/papirus
cd /tmp/papirus
git clone https://github.com/repaper/gratis.git

cd /tmp/papirus/gratis-master/PlatformWithOS
make rpi-epd_fuse
sudo make rpi-install
sudo service epd-fuse start
```

# Python API

#### The Basic API

```python
from papirus import Papirus

# The epaper screen object
screen = Papirus()

# Write a bitmap to the epaper screen
screen.display('./path/to/bmp/image')

# Perform a full update to the screen (slower)
screen.update()

# Update only the changed pixels (faster)
screen.partial_update()

# Change screen size
# SCREEN SIZES 1_44INCH | 1_9INCH | 2_0INCH | 2_6INCH | 2_7INCH
screen.set_size(papirus.2_7INCH)

```

#### The Text API
```python
from papirus import PapirusText

text = PapirusText()

# Write text to the screen
# text.write(text)
text.write("hello world")

# Write text to the screen at selected point
# text.write(text, (x,y))
text.write("hello world", (10, 10) )
```

#### The Image API
```python
from papirus import PapirusImage

image = PapirusImage()

# easy write image to screen
# image.write(path)
image.write('/path/to/image')

# write image to the screen with size and position
# image.write(path, width, (x,y))
image.write('/path/to/image', 20, (10, 10) )
```
#### Notes

Your python script must be running with root privileges to update the screen and change sizes.
This code will only allow the script to run as root

```python
import os
import sys

user = os.getuid()
if user != 0:
    print "Please run script as root"
    sys.exit()
```

# Command Line

```bash
# Set the screen size you are using
papirus-set [1.44 | 1.9 | 2.0 | 2.6 | 2.7 ]

# Write data to the screen
papirus-write "Some text to write"

# Draw image on the screen
papirus-draw /path/to/image -t [resize | crop]

# Clear the screen
papirus-clear

#Fill screen with centered text (less than 40 characters)
papirus-textfill "Some text to write"
```

#### Demos
All demos can be seen by running the following commands. Code can be found in the repo for the python bin directory. 

```bash
# Show clock
papirus-clock

# Run game of life
papirus-gol

# Show system information
papirus-system (coming soon)

# Push framebuffer to screen
papirus-framepush (coming soon)

# Demo of using the buttons
papirus-buttons

# Demo of getting temperature from LM75
papirus-temp

# Snakes game
papirus-snakes (coming soon)
```

### Tips for using images
The PaPiRus can only display Bitmap images (.BMP) in black and white (2 bit colour).

Use the following screen resolutions:
```
1.44"     128 x 96
1.9"      144 x 128
2.0"      200 x 96
2.6"      232 x 128
2.7"      264 x 176
```
Also try using the method partial_update() instead of the update() one if you want to refresh the screen faster and mayb want to create some simple animations. Remember thought that the partial method cannot be used indefinitely and you will have to refresh the screen every once in a while.
