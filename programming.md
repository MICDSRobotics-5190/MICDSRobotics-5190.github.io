---
layout: default
title: Programming Overview
---

# Programming Overview

This is the home for all of the software development being done on the team!

### [Robotplus](https://github.com/MICDSRobotics/robotplus)

Check out robotplus, our repository for all for the reusable code we have developed over the years. It has wrappers for sensors, hardware classes with extra functions, and all sorts of added goodies. If you're ever stuck getting your drivetrain to work or not sur ehow to initilize Vuforia and the sample code doesn't help, you can always dig through here for some tips!

* Javadoc hosted here: [{{ site.url }}/robotplus]({{ site.url }}/robotplus)

### [RelicRecovery](https://github.com/MICDSRobotics/RelicRecovery)

This is our repository for the code we're using on our robots for the 2017-2018 season. It has all of our OpModes, so it's good for getting a feel for our code structure and how the things in Robotplus can be used to write clean, easily maintained code. It focuses more on control flow and finding the most consistent way to accomplish autonomous goals, as well as finding the easiest controls for drivers to operate the robot during TeleOp.

* Javadoc hosted here: [{{ site.url }}/RelicRevovery]({{ site.url }}/RelicRecovery)
* FTC's SDK's most updated Javadoc also hosted here: [http://ftctechnh.github.io/ftc_app/doc/javadoc/index.html](http://ftctechnh.github.io/ftc_app/doc/javadoc/index.html)

### Teaching Resources

All of our resources for teaching new members the basics of Android Studio with FTC's SDK and getting started with Github are found on our wiki below!

{% for entry in site.data.wiki %}
####  {{ entry.title }}
  {% for subpage in entry.subpages %}
* [{{ subpage.title }}]({{ subpage.url }})
  {% endfor %}
{% endfor %}
