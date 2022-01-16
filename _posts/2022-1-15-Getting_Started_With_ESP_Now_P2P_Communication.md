---
layout: post
title: Getting Started With ESPNOW P2P Communication
---

![ESP_P2P]({{ site.baseurl }}/images/esp32p2p.jfif)

[ESP-NOW Documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/network/esp_now.html)

*ESP-NOW is a kind of connectionless Wi-Fi communication protocol that is defined by Espressif. In ESP-NOW, application data is encapsulated in a vendor-specific action frame and then transmitted from one Wi-Fi device to another without connection. CTR with CBC-MAC Protocol(CCMP) is used to protect the action frame for security. ESP-NOW is widely used in smart light, remote controlling, sensor, etc.*

[yoursunny](https://github.com/yoursunny) was nice enough to write and share an [Arduino wrapper](https://github.com/yoursunny/WifiEspNow) for it. 

I used it to make this [ESP P2P Example](https://github.com/CapeC0d3r/ESP_NOW_Example).  

I loaded it on to a few ESP modules and they communicated with each other.  There is one in my basement controlling an LED in my office right now.  If I do more with it, I'll write about it. 

That's all I got for now.  I will write more when I have time to mess around with it more. 

