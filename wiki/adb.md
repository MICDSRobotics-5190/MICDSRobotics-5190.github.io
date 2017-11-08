---
layout: default
title: Remote Debugging from you Computer
---

# {{ page.title }}
## What It's Used For
Wireless uploading to the phone
## Setting Up
Make sure you have the Android SDK Tools installed.
Plug the phone into the computer.
Open up the command line and type
```
adb tcpip 5555
```
This will set the port. Then, find the phone's IP Address and **make sure that the computer and the phone are on the same network**.
Now do:
```
adb connect [IP of the phone]
```
Now you are connected. If this fails, try using
```
adb kill-server
```
and then
```
adb start-server
```
also,
```
adb get-state
```
might also be of assistance.
## Usage
This connection will act as a regular USB connection. So treat it if it were plugged into your computer.

[More Info Here](https://developer.android.com/studio/command-line/adb.html#Enabling)
