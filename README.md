# mqtt-control-panel

A simple alarm control panel for Home Assistant's `manual_mqtt` alarm. Designed to run on a Raspberry Pi using an Adafruit 3.5" PiTFT.

Video of the control panel in action: <https://www.youtube.com/watch?v=2Lei8n_aSJI>

Display mockup:

![](screenshot.png)

# Hardware

 - Raspberry Pi Zero Wireless (other modern Pis will likely work fine)
 - [Adafruit PiTFT 3.5" display](https://www.adafruit.com/product/2441)

# Requirements

This project requires Python 2.7 with the following packages:

 - paho-mqtt
 - pygame
 - python-dotenv

**IMPORTANT:** SDL 2.x and SDL 1.2.15-10 have some serious incompatibilities with touchscreen. You can force SDL 1.2 by ~~running a script: https://learn.adafruit.com/adafruit-pitft-28-inch-resistive-touchscreen-display-raspberry-pi/pitft-pygame-tips#ensure-you-are-running-sdl-1-dot-2~~

Update: script no longer works. New solution was originally published in english here: https://www.raspberrypi.org/forums/viewtopic.php?t=250001. Instructions are gone over below.

# Instructions

1. Start with a fresh install of the latest Raspberry Pi OS lite release: <https://www.raspberrypi.org/downloads/raspberry-pi-os/>

2. Use `sudo raspi-config` to set your keyboard layout and Wi-Fi SSID/password.

3. Download dependencies and the program

    `sudo apt-get install git python-pip python-pygame`
    
    `pip install paho-mqtt python-dotenv`
    
    `wget https://www.dropbox.com/s/0tkdym8ojhcmbu2/libsdl1.2debian_1.2.15+veloci1-1_armhf.deb`
    
    `wget https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/master/adafruit-pitft.sh`
    
    `chmod +x adafruit-pitft.sh`
    
    `cd /srv`
    
    `git clone https://github.com/vincenttaglia/mqtt-control-panel.git .`

4. Configure the control panel

    `sudo cp .env.dist .env`
    
    `sudo nano .env`

5. Install an old SDL 1.2 package that works with touchscreens

    `cd ~`
    
    `sudo dpkg -i libsdl1.2debian_1.2.15+veloci1-1_armhf.deb`
    
    `sudo apt-get -f install`

6. Setup Adafruit

    `sudo ./adafruit-pitft.sh`

7. Test control panel

    `python /srv/main.py`


8. Set application to start on boot (optional)

    `sudo nano /etc/rc.local`
    
    Add `python /srv/main.py &` the line before `exit 0`
    
    You may have to either have to use `sudo pip install paho-mqtt python-dotenv` or add `pip install paho-mqtt python-dotenv &` to rc.local before executing main.py for a single startup. I have only confirmed that the latter works.

# Configuration

Copy `.env.dist` to `.env` and update the values accordingly.
