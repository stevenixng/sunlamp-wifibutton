# sunlamp-wifibutton

## Making a sunlamp WiFi enabled
We have a sunlamp that we use when working from home during dreary days. Unfortunately there is a 60 minute timer and the lamp shuts off and we have to walk over to hit the button again. I've wired a Wemos D1 mini to the button and made the device WiFi enabled. These plans could be used to toggle a button press for just about anything!

### Video demo
[![youtube demo](https://img.youtube.com/vi/rt5SqVs1bFE/0.jpg)](https://www.youtube.com/watch?v=rt5SqVs1bFE)

### Hardware
* [Wemos D1 Mini](https://www.banggood.com/3Pcs-Wemos-D1-Mini-V2_3_0-WIFI-Internet-Of-Things-Development-Board-Based-ESP8266-ESP-12S-4MB-FLASH-p-1230988.html?rmmds=myorder)
* [DC 5V 1CH Relay Shield](https://www.banggood.com/3Pcs-DC-5V-1CH-Relay-Shield-V2-Version-2-For-WEMOS-D1-Mini-ESP8266-WiFi-Module-p-1123511.html?rmmds=myorder&cur_warehouse=CN)
* [Switching Power Supply Converter Module](https://www.amazon.com/gp/product/B01FA0KJFA/)
* Solid core wire

### Software
* [Home Assistant](https://home-assistant.io/) but you can use whatever.
* [Tasmota firmware](https://github.com/arendst/Sonoff-Tasmota).

### Instructions
I used esptool to upload the tasmota firmware to the D1 mini: https://github.com/arendst/Sonoff-Tasmota/wiki/Esptool
and configured the relay shield as shown here: https://github.com/tIsGoud/Tasmota-on-WEMOS

This particular device will turn on the lamp with a momentary button press instead of a toggle switch. I had to go through the [tasmota commands](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands) and run ```PulseTime<x>``` to simulate the button press for 1 second.

Use the continuity tester on the voltmeter to find the button contacts. Probe around for the two solder points near where the button is. When the button is pressed, the voltmeter will beep. Solder some wires to this and connect to the relay.

To power the D1 mini, I soldered the power supply to the A/C and the other end to the D1 mini.



![inside](https://github.com/stevenixng/sunlamp-wifibutton/blob/master/pictures/inside.jpg "sorry for the terrible graphics")

## home assistant entry in switches.yml
I have my configuration split up into separate files like switches.yml. This example should be enough to go on if you have been messing with Home Assistant.

```
  - platform: mqtt
    name: "sunlamp"
    state_topic: "stat/sunlamp/POWER"
    command_topic: "cmnd/sunlamp/POWER"
    availability_topic: "tele/sunlamp/LWT"
    payload_on: "ON"
    payload_off: "OFF"
    payload_available: "Online"
    payload_not_available: "Offline"
    qos: 1
    retain: true
```
