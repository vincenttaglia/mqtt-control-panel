# mqtt-control-panel

A simple alarm control panel for Home Assistant's `manual_mqtt` alarm. Designed to run on a Raspberry Pi using an Adafruit 3.5" PiTFT.

Video of the control panel in action: <https://www.youtube.com/watch?v=2Lei8n_aSJI>

Instructions for building your own: <https://www.hackster.io/colinodell/diy-alarm-control-panel-for-home-assistant-ac1813>

Display mockup:

![](screenshot.png)

# Hardware

 - Raspberry Pi Zero Wireless (other modern Pis will likely work fine)
 - [Adafruit PiTFT 3.5" display](https://www.adafruit.com/product/2441)

# Requirements

This project requires Python 3 with the following packages:

 - paho-mqtt
 - pygame
 - python-dotenv

# Instructions

1. Use Raspberry Pi Imager to create a Raspberry Pi OS Lite image pre-configured with your Wi-Fi SSID/password, user, and SSH key (or password, though that is not recommended for security purposes).

2. Update OS and install pip

    ```
    sudo apt update && sudo apt upgrade

    sudo apt install python3-pip
    ```

3. Install Adafruit software
        
    ```
    sudo pip3 install --upgrade --break-system-packages click setuptools adafruit-python-shell

    wget https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/refs/heads/main/adafruit-pitft.py
    
    sudo python3 adafruit-pitft.py    
    ```
    Select PiTFT 3.5" configuration:
    > Select configuration:
    >
    > [1] PiTFT 2.4", 2.8" or 3.2" resistive (240x320) (320x240)
    >
    > [2] PiTFT 2.2" no touch (320x240)
    >
    > [3] PiTFT 2.8" capacitive touch (320x240)
    >
    > ***[4] PiTFT 3.5" resistive touch (480x320)***
    >
    >[5] PiTFT Mini 1.3" or 1.54" display (240x240)
    >
    > [6] ST7789V 2.0" no touch (320x240)
    >
    >[7] MiniPiTFT 1.14" display (240x135)
    >
    >[8] BrainCraft HAT or 1.3" TFT Bonnet + Joystick (240x240)
    >
    >[9] Uninstall PiTFT
    >
    >[10] Quit without installing


    Select 90 degree rotation
    
    > Display Type: PiTFT 3.5" resistive touch
    > 
    > Select rotation:
    > 
    > ***[1] 90 degrees (landscape)***
    > 
    > [2] 180 degrees (portrait)
    > 
    > [3] 270 degrees (landscape)
    > 
    > [4] 0 degrees (portrait)

4. Create virtual env, download program and dependencies
    
    ```
    sudo apt install libsdl2-dev libsdl2-image-2.0-0 libsdl2-ttf-2.0-0

    git clone https://github.com/vincenttaglia/mqtt-control-panel.git .

    python3 -m venv .venv

    source .venv/bin/activate

    pip3 install -r requirements.txt
    ```

    

4. Configure the control panel

    ```
    cp .env.dist .env
    
    nano .env
    ```

7. Test control panel

    ```
    python3 main.py
    ```


8. Set application to start on boot (optional)

    Normally on a Pi, I would use rc.local like this:

    ```
    echo "#!/bin/sh -e
    source $(pwd)/.venv/bin/activate && $(pwd)/.venv/bin/python3 $(pwd)/main.py &
    exit 0" | sudo tee /etc/rc.local

    sudo chmod +x /etc/rc.local
    ```
    But I was having trouble getting rc.local to start the program correctly at boot. I was able to get it to work consistently with a custom service though:
    ```
    echo "[Unit]
    Description=Alarm Panel
    After=multi-user.target
    Wants=multi-user.target
    [Service]
    User=root
    Group=root
    Type=simple
    Restart=always
    RestartSec=5
    ExecStart=/bin/bash -c 'source $(pwd)/.venv/bin/activate && $(pwd)/.venv/bin/python3 $(pwd)/main.py'
    ExecStartPre=/bin/sleep 10
    [Install]
    WantedBy=default.target" | sudo tee /lib/systemd/system/alarm-panel.service
    ```
    Reload the daemon and enable the service to start at boot.
    ```
    sudo systemctl daemon-reload
    sudo systemctl enable alarm-panel.service
    ```

9. Reboot
    ```
    sudo reboot
    ```

# Configuration

Copy `.env.dist` to `.env` and update the values accordingly.
