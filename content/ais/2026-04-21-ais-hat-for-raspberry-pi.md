---
title: AIS Hat for Raspberry Pi 4B
subtitle: "How to receive AIS data on your own device "
date: 2026-04-21
tags:
  - ais
  - raspberry pi
author: Blake Roberts
draft: false
summary: "This is a complete guide to setting up your own dAISy Hat for the
  Raspberry Pi 4B. "
---
# **What this achieves**  

This helps create access via the internet to vessel locations that are normally unobtainable due to remoteness. This allows the viewer to see the data through their own webpage and better understand vessel movement in those areas.

## **List of Commands:**

- **Accessing the Raspberry Pi**

```
hostname -I
ssh pi@<IP_ADDRESS>
```

- **System Setup and Updates**

```
sudo apt update
sudo apt upgrade -y
```

- **Install Signal K**

```
bash <(curl -sL [https://deb.nodesource.com/setup_20.x])
sudo apt install -y nodejs
sudo npm install -g --unsafe-perm signalk-server
signalk-server
```

- **Enable Signal K on Boot**

```
sudo nano /etc/systemd/system/signalk.service
sudo systemctl enable signalk
sudo systemctl start signal's
sudo systemctl status signalk
```

- **Access Signal K Dashboard**

```
http://<IP_ADDRESS>:3000
```

- **Check AIS Data (dAISy HAT)**

```
sudo cat /dev/serial0
screen /dev/serial0 38400
```

- **WiFi Setup**

```
sudo raspi-config
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

- **Reboot and Shutdown**

```
sudo reboot
sudo shutdown now
```

- **Testing and Troubleshooting**

```
top
journalctl -u signalk -f
curl [http://localhost:3000](http://localhost:3000)
```

- **Signal K Setup (AIS Integration)**
  - Once Signal K is running, follow these steps to connect your AIS data:

- Open the Signal K dashboard in your browser:
  - http://<IP_ADDRESS>:3000
- Go to Server -> Connections
- Add a new connection with the following settings:
  - Type: Serial
  - Device: /dev/serial0
  - Baud Rate: 38400
  - Data Type: NMEA0183
- Save and enable the connection.
- Check the Data Browser to confirm AIS messages are being received.
- (Optional) Install plugins for data forwarding such as MarineTraffic or OpenAIS.

### **Why you should do it**

This is super easy to install and set up, and in no time you are sending data to websites like MarineTraffic and helping other mariners stay safe and accountable.

**Low cost**  
The total cost of the setup is relatively low. The Raspberry Pi 4B is about $40 and the dAISy HAT is about $75. After that, all you need is a decent antenna, power, and access to a wired or wireless network.

**Low effort**  
The installation and setup of this device is super easy and can be done in less than an hour. There are also many resources available online, so if you run into an issue you should have no problem finding a solution. The use of AI when troubleshooting can also be helpful for this application.

**Works really well**  
Once up and running, this is a super low-cost and low-maintenance system that can basically run indefinitely without needing much attention. It is also fairly versatile. If you want to take the analysis a step further, you can pull the raw data from the receiver and use it in other software however you choose.