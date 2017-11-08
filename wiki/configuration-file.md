---
layout: default
title: Creating a Configuration file
---

# {{ page.title }}

Configuration files are needed so that the robot knows how to interact with all of the harware you connected to it. It handles all of the little things about actually connecting the code we just wrote to actual, physical movement on the robot. FTC provided us with a very simple way of editing the configuration files through the phone, so let's start there.

## Getting to the Configuration Editor

In either the Robot Controller app or the Driver Station app, click the three dots in the top right corner to open the menu.

![Driver Station](http://i.imgur.com/sjxfnFP.png)

![Driver Station Menu](http://i.imgur.com/RftdjKF.png)

From there, you want to hit "Configure Robot", which will send you to the configuration manager. Here you can make new configurations, delete old ones, edit ones, and activate different ones you're using. To make a new file, you have to connect the Robot Controller phone to the Core Power Distribution Module and turn it on while connected to all of the other modules, then hit `New`, and then scan for the hardware connected to the robot.

![Inside Configuration](http://i.imgur.com/5uFrAFw.png)

## Using the Editor

After you scan, all of your hardware should show up in the editor. If you don't see it, make sure that the core power distribution module is turned on and that all of your other modules are securely connected.

Next, you're going to set every peice of hardware's name. It has to correspond to what you put in your code -- so if your code looks like this:

    leftMotor  = hardwareMap.dcMotor.get("left motor");
    rightMotor = hardwareMap.dcMotor.get("right motor");

    spinner = hardwareMap.dcMotor.get("spinner");
    flywheel = hardwareMap.dcMotor.get("flywheel");

    beaconSlider = hardwareMap.crservo.get("slider");

Then you want to make sure in your configuration file, you name your motors `left motor` and `right motor`, or else you won't be able to use those motors. Once you're done, your modules will look something like either of these:

![Module Confiuration](http://i.imgur.com/xiVbmcq.png)

![Servo Module config](http://i.imgur.com/KJUu0kW.png)

## Save & Activate

Once you're done, don't forget to hit save and activate it when you're back at the main menu for the configuration editor! Otherwise you'll have to re-do all of your file, or the robot might not even use it!

## Recap

Create the configuration file through either of the phones while all of the DC Motor Controllers, Servo Controllers, Legacy Module Controllers, etc. are connected, then save it and activate your new file. You're ready to go!
