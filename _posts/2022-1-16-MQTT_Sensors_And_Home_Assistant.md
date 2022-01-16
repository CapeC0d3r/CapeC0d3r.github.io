---
layout: post
title: MQTT Sensors and Home Assistant
---

![MQTT_Diagram]({{ site.baseurl }}/images/2022-1-16-MQTT_Sensors_And_Home_Assistant/MQTT.jpg)

I have been meaning to set up [Home Assistant](https://www.home-assistant.io/) at my house.  I had it installed on a raspberry pi, but I already have a computer that is running 24/7 recording IP Cameras using Blue Iris, so I just spun up a [Home Assistant virtual machine](https://www.home-assistant.io/installation/windows/) on that computer and saved the pi for another project. 

I have a handful of ESP modules collecting dust, so I thought I would put them to use and learn a thing or two about [MQTT](https://mqtt.org/).

I installed the [Mosquitto Broker](https://www.home-assistant.io/docs/mqtt/broker/) add-on in Home Assistant. 

These links were helpful: 

https://github.com/home-assistant/addons/blob/master/mosquitto/DOCS.md

https://cyan-automation.medium.com/setting-up-mqtt-and-mosquitto-in-home-assistant-20eb810a91e6

https://www.youtube.com/watch?v=dqTn-Gk4Qeo

Then I read about [configuring MQTT sensors in Home Assistant](https://www.home-assistant.io/integrations/sensor.mqtt/) and came to the conclusion that I should read up on [YAML](https://www.cloudbees.com/blog/yaml-tutorial-everything-you-need-get-started).  I found this [ESP module MQTT example](https://github.com/smrtnt/Open-Home-Automation/tree/master/ha_mqtt_sensor_dht22) at the bottom of the page, and decided to load it onto an ESP32 dev board to see if I could get it reporting some fake data on my Home Assistant dashboard.  I cloned the repository it was in and was stoked to see it had a bunch of different MQTT examples. 

I noticed that the repository is  no longer maintained and that it points you to [ESPHome](https://esphome.io/guides/getting_started_hassio.html).  Normally I would jump down that rabbit hole immediately, but today I am going to focus on getting some basics accomplished before moving on to ESPHome. 

I hacked up the DHT22 example in the repository that I pointed to, and made an example that publishes data to a topic and subscribes to a topic for incoming communication. 

Here is a template to get started with.  Install / setup mosquitto broker on home assistant, and update the network / mqtt broker information at the top of main.cpp. 

https://github.com/CapeC0d3r/ESP32_MQTT_Template_Example

This example was for a ESP32 dev board I have.  You'll have to configure the platformio.ini file to work with whatever modules you are trying to work with. 

I added an entry to my Home Assistant "configuration.yaml" file using the file editor plug-in.  The Home Assistant layout seems to change pretty often, but right now if you want to install it go to Configuration - Add-ons, Backups, Supervisor, then go to the add-on store.  Once installed, start it, open the web ui, and open the configuration.yaml file.  

To read the data my ESP program was publishing I added this to my configuration.yaml file:

```
sensor:
  - platform: mqtt
    name: "ESP32 Sensor 1 - Temperature"
    state_topic: "ESP32_MQTT_Sensor_1/Data"
    unit_of_measurement: "°F"
    value_template: "{{ value_json.Temperature }}"
  - platform: mqtt
    name: "ESP32 Sensor 1 - Humidity"
    state_topic: "ESP32_MQTT_Sensor_1/Data"
    unit_of_measurement: "%"
    value_template: "{{ value_json.Humidity }}"
```

 Then I saved the file, and went to Configuration - Settings.  I clicked "Check Configuration" to make sure that I didn't screw up any yaml syntax, then I restarted the server so my new configuration would be loaded up.  

After restarting I edited my dashboard and added a sensor card.  In the card configuration I saw a sensor entity for the "ESP Sensor 1 - Temperature" and "ESP Sensor 1 - Humidity"

I created a sensor card for each, and it displayed the data I was publishing. 

![MQTT_Diagram]({{ site.baseurl }}/images/2022-1-16-MQTT_Sensors_And_Home_Assistant/terminal_and_dashboard_sensor_data.JPG)

I noticed that my ESP Module needed to be connected to a terminal in order to work.  I didn't see a 

```cpp
while(!Serial) {;}
```

anywhere in the code, so I assume it has something to do with hardware. I don't really care right now, and it's unrelated to this post, but I saw a [post](https://www.esp8266.com/viewtopic.php?p=53827) that seemed relevant.  

![MQTT_Diagram]({{ site.baseurl }}/images/2022-1-16-MQTT_Sensors_And_Home_Assistant/esp_only_works_with_terminal_connected.JPG)

I'll figure that out later. 

I'll buy some sensors today and start working with some real data soon.  
