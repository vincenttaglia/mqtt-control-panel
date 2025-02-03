# mqtt-control-panel

A simple alarm control panel for Home Assistant's `manual_mqtt` alarm. Designed to run on a Raspberry Pi using an Adafruit 3.5" PiTFT.

Video of the control panel in action: <https://www.youtube.com/watch?v=2Lei8n_aSJI>

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

1. Use Raspberry Pi Imager to create an image already configured with your Wi-Fi SSID/password, user, and SSH key (or password, though that is not recommended for security purposes).

2. Download dependencies and the program

    `git clone https://github.com/vincenttaglia/mqtt-control-panel.git .`

    `sudo apt install git python-pip python-pygame libsdl2-dev`

    `pip install -r requirements.txt`

3. Install Adafruit software
        
    `sudo pip3 install --upgrade --break-system-packages click setuptools adafruit-python-shell`

    `wget https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/refs/heads/main/adafruit-pitft.py`
    
    `sudo python3 adafruit-pitft.py`    
    
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

    

4. Configure the control panel

    `sudo cp .env.dist .env`
    
    `nano .env`

7. Test control panel

    `python main.py`


8. Set application to start on boot (optional)

    ```
    echo "python $(pwd)/main.py &
    exit 0" | sudo tee /etc/rc.local
    ```

    `sudo chmod +x /etc/rc.local`

# Configuration

Copy `.env.dist` to `.env` and update the values accordingly.
