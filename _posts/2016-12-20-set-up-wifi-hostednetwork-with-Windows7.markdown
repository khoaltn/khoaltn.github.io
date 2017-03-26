---
layout: post
title:  "Configure hostednetwork (WiFi Hotspot) on Windows 7"
date:   2016-12-20 22:41:19 
categories: "Computer Networking"
tag: 
- computer networking
- windows7
headerImage: true
blog: true
hidden: false # count this post in blog pagination
author: site.author
description: "How to set up a WiFi hotspot/hosted network on Windows 7 if your desktop hardware supports it"
---

A few years ago I visited a home where there was only a Dell desktop with Windows 7 and cabled Internet in the house, and no WiFi. My host gave me the administrator account on the computer to use while she went on vacation. I wanted to use wifi on my laptop and phone so badly, so I tried to figure out whether it was possible to set up a WiFi hotspot on this desktop. It took me more hours than I'd like to admit, so to save you some pain in the posterior, below are the steps to follow. (It might work on other Windows versions too, but no guarantee).

1. Make sure your hardware supports a wireless hotspot, and is connected to the Internet. Google if needed. You probably will need to know the brand and model number of your computer. If yes, continue. If no, sorry - I guess you'll have to buy a WiFi hub/router/etc.

2. In File Explorer, search for a folder named `RtBackup`, delete all files in it, then changed the folder's name to ANYTHING other than its original name. The last time I did this hack, this folder resides in `C:\Windows\System32\LogFiles\WMI`.

3. Go to **Start** and search for **Windows Services** tool. _*IMPORTANT:_ Start the **Windows Event Log** service FIRST. Then start **SSTP (Secure Socket Tunneling Protocol Service)**. Without these 2, the rest won't work. By "the rest," I mean to start the following services:

* Application Layer Gateway Service
* Network Connections
* Network Location Awareness (NLA)
* Plug And Play
* Remote Access Auto Connection Manager
* Remote Access Connection Manager
* Remote Procedure Call (RPC)
* Telephony 

After the little hack above, we are ready to follow the official instructions posted on Dell's website at http://www.dell.com/support/article/us/en/04/SLN208643/EN. The entire content of this article is reproduced here for your convenience. 

4. The adapter for the connection is Microsoft Virtual WiFi Miniport Adapter. If there is problem setting this up, go to Start -> search for Device Manager -> Network -> reinstall driver for this adapter.

5. Hit the Start button, and type "Command Prompt" or "cmd" on the search box. If your computer is based on Windows 8, you'll need to press the keyboard's Windows logo key to switch to the system's Modern UI Style and type "Command Prompt" or "cmd." Run Command Prompt with admin rights. To do that, right-click on the Command Prompt icon and select "Run as administrator."

6. Type "netsh wlan show drivers" (without the quotation marks) in Command Prompt to check whether or not your computer supports a hosted network. The "Hosted network supported" field should indicate "Yes" if your unit supports WiFi sharing. If it says "No," you'll have to download the corresponding driver for your WiFi adapter first before proceeding.

![alt text]({{ site.url }}\assets\images\20161220wifiWin1.jpg)

7. To create a hotspot, type "netsh wlan set hostednetwork mode=allow ssid=yournetworkname key=yournetworkpassword," and hit Enter on your keyboard.
Remember, "ssid" refers to the WiFi hotspot's name while "key" is said network's password. You can also use the aforementioned command to change the hotspot's name and password.

![alt text]({{ site.url }}\assets\images\20161220wifiWin2.jpg)

8. To get the hotspot up and running, type "netsh wlan start hostednetwork." Make sure your computer's WiFi adapter is also switched on, or the hotspot won't work at all.

![alt text]({{ site.url }}\assets\images\20161220wifiWin3.jpg)

9. Again, hit the Start button. Type "Network and Sharing Center," and left-click on it. If you're using a Windows-based unit, you probably know what to do by now. (Switch to Modern UI Style, and type "Network and Sharing Center.")

10. Select "Change adapter settings," which can be found on the left-hand side of the window. Both the network connection you want to share and your newly created WiFi hotspot are shown here. Choose the network connection you wish to share. Right click on it, select "Properties," and go to the "Sharing" tab. Check the option "Allow other network users to connect through this computer's Internet connection." This time, select the WiFi hotspot you created earlier.

![alt text]({{ site.url }}\assets\images\20161220wifiWin4.jpg)

11. To turn off your hotspot, type "netsh wlan stop hostednetwork" in Command Prompt. 

Best of luck! I hope you won't have to spend so much time wading through useless answers on Microsoft's help forum or Yahoo! Answers like I did :)
