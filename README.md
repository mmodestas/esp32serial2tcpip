# esp32serial2tcpip
ESP32 serial to to TCP adapter. 
Wifi bridge for Paradox Alarm Interface (PAI).

# What is it intended for
I made this adapter for easier integration between my PARADOX SP7000 alarm panel and PAI (Paradox Alarm Interface) - a wonderful peace of software. I run PAI as add-on in my Home Assistant instalation, but there as many other ways how to run it.

# Hardware
I designed and ordered a custom pcb from JLCPCB. Here is a link to my project.

Connecting to Paradox panel:

Serial on Panel           esp32serial2tcpip

    ┌───────┐
    │ Rx   ┌╵ =================> RX 
    │ Tx   │  =================> TX 
    │ GND  │  =================> GND 
    │ AUX+ └╷ =================> 12V
    └───────┘
# Firmware
I use Tasmota. You can use my compiled version or compile your own - serial to TCP is not included in standard pre-compiled builds.

Tasmota template:

    {"NAME":"esp32serial2tcpip","GPIO":[1,1,1,1,1,1,1,1,1,1,1,1,5504,5472,1,1,0,1,1,1,0,1,1,1,0,0,0,0,1,1,1,1,1,0,0,1],"FLAG":0,"BASE":1}

Tasmota rule: 
You need to create rule so serial to TCP/IP is started every time tasmota boots. Go to tasmota console and issue this command:

    rule1 ON System#Boot do TCPStart 23 endon ON System#Boot do TCPBaudRate 9600 endon

Don't forget to enable rule by issuing the following command in console:

    rule1 1

PAI configuration:

The following settings needs to be set in your PAI configuration file.

    # Connection Type
    CONNECTION_TYPE: IP          # Serial or IP
    # IP Connection Details
    IP_CONNECTION_HOST: '192.168.1.10' # Address of the ESP32
    IP_CONNECTION_PORT: 23             # Port of the ESP32
    IP_CONNECTION_PASSWORD: 'paradox'  # IP Module password. "paradox" is factory default.
    IP_CONNECTION_BARE: true           # Used this for base Serial over TCP tunnels
