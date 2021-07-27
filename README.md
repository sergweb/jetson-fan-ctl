# jetson-fan-ctl
Automagic fan control for the Nvidia Jetson Nano with parameterized polinomial fan curve. Fork from [Pyrestone's project](https://github.com/Pyrestone/jetson-fan-ctl).

## Requirements:

### Hardware
You will need a 5V PWM fan for this to make any sense.  
I used the **Noctua nf-a4x20 5V PWM** fan.

Additionally, I recommend you use the barrel jack with a 4A power supply.  

### Software
I will assume you use the standard image on your jetson nano.

Python 3 should be pre-installed on the jetson nano.  
You can check this using <code>python3 --version</code>  
(3.5 or higher should be fine.)  
otherwise, you can install it with  

    sudo apt install python3-dev

Package [jetson_stats](https://github.com/rbonghi/jetson_stats) is installed:

    sudo -H pip install -U jetson-stats

Service "automagic-fan" will be configured to start after "jetson_stats" service.

## How to install:
run

    sudo ./install.sh

The script will automatically run at boot time.

It's a set-it-and-forget-it type thing, unless you want to mess with the fan speeds.

## How to customize:
open /etc/automagic-fan/config.json with your favorite editor (I'm using nano):  

    sudo nano /etc/automagic-fan/config.json

you will find the following lines:

    {
    "FAN_OFF_TEMP":35,
    "FAN_MAX_TEMP":53,
    "UPDATE_INTERVAL":2,
    "MAX_PERF":1,
    "FAN_CURVE_POWER":0.5
    }

<code>FAN_OFF_TEMP</code> is the temperature (°C) below which the fan is turned off.  
<code>FAN_MAX_TEMP</code> is the temperature (°C) above which the fan is at 100% speed.  
The script interpolates linearly between these two points.

<code>UPDATE_INTERVAL</code> tells the script how often to update the fan speed (in seconds).  
<code>MAX_PERF</code> values greater than 0 maximize system performance by setting the CPU and GPU clock speeds to the maximum. 

<code>FAN_CURVE_POWER</code> is the power of polinom in fan curve. 
Prioritizes fan silence over board temperature if greater than 1 (e.g. 2) and board temperature over fan silence if less than 1 (e.g. 0.5).
The value is equal to 1 if not specified explicitely in config.json file.

You can use either integers (like 20) or floating point numbers (like 20.125) in each of these fields.  
The temperature precision of the thermal sensors is 0.5 (°C), so don't expect this to be too precise.

Any changes in the script will be will be applied after the next reboot.  
You can run

    sudo service automagic-fan restart

to apply changes immediately.

If you suspect something went wrong, please check:

    sudo service automagic-fan status

## How to contribute:
Feel Free to open Issues or Pull requests on this Repository.

If you find this tool useful, you can buy me a coffe:

[![](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://paypal.me/pyrestone)
