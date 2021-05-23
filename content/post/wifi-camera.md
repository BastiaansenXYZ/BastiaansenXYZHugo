+++
author = "Sander Bastiaansen"
title = "Wifi Camera"
date = "2021-03-25T21:20:00+01:00"
description = "This post is to describe how to get a cheap M709 Shenzhen camera running in your Domotica systems"
tags = [
    "blogpost",
    "domotica",
    "hardwarehacking",
]
draft = false
+++

# Overview
This post is created to show you how you can "hack" in to the videostream of a cheap M709 Shenzhen video-camera. These camera's are great for use with Domotica systems or indoor surveillance. These camera's come with an app that is used to controll them. But you`ll be able to connect them to your WiFi and stream direct video to the device of your choice. In my situation I need to get access to the RTSP stream of the camera so that it can be picked up by my MotionEye installation. In turn that MotionEye installation is able to forward snapshots or a stream of it's own to my Domotica (Domoticz) and notification systems. I will use this camera as a cheap replacement for a "Ring" like camera. The doorbell is already automated in a cheap manner with a Wemos D1.

## Preparation
In order to get the camera working you`ll need a few things. First, obviously, a PC that runs Wireshark in your own network. The OS isn't important for that matter. Second, a camera on itself. You can find one on the well known Chinese webshops. And lastly a phone that is able to run the app that comes with the camera (Some recent Android or IOS device that has WiFi should be fine). For this how-to I have used the following [camera](https://nl.aliexpress.com/item/32976087407.html).

## Setting up
Connect the camera to the app that comes with your camera. In my case the app was called "HIDVCAM". Just run through the set-up as the app suggests. Second in setting up, download the following [package](https://drive.google.com/file/d/1e2Gxs5iznQ5yBFzzHJAk5JadsrQYasY4/view) and install HRS-1.1.1_install.exe. This will install the application called HRS on your PC. Run it, and check if you can fetch the stream with the HRS application on your PC. Once set-up on your phone the camera will be visible in the network via WiFi and the application should automatically pick up on the camera.

![HRS Application](../static/images/WireShark.png)

## Sniffing the RTSP connectionstring from WireShark
This part is the most tricky, hacky and exciting part of the process. You`ll be able to fetch the RTSP connectionstring from a TCP package in Wireshark. Open up WireShark and connect to a networkadapter that is running within the same network as the camera. This doesn't mean that you need WiFi. Connect with the Ethernet adapter that connects to that same network if you wish. Just double-click the proper adapter and you will see that data will start flowing in. Now use the following format to set up a filter so that you will only see data that is relevant to this issue *ip.src==192.168.1.100*. Replace the IP after the equal signs with the IP of the camera. You can find that IP in the app on your phone. After that the image you see should look a little like the image below.

![Wireshark Capture](../static/images/WireShark.png)

Now fire up the HRS application on your PC, and connect to the camera. Go back to WireShark and sort the Protocol column and scroll up and down till you find RTSP entries in the list. Find one where the Info column starts with *PLAY*. You should immediatly notice that there is a well human-readable RTSP connectionstring there. Rightclick the entry, go to copy, and copy the summary as text. And paste in the text-editor of your choice. From there you`ll be able to copy the RTSP connectionstring. It will look something like rtsp://IP_ADRESSS:554/LONGSTRINGOFNUMBERSANDOTHERCHARACTERS_0.

## Testing the RTSP connection
I assume that you have VLC Media Player installed on your PC for this part. Just open up VLC, go to media and open a network stream. Paste the RTSP string in the field that pops up. And VLC should automatically pick up on it including sound from the camera!
