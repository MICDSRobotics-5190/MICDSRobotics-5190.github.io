---
layout: post
title: "Hello World!"
date: 2017-11-04
author: "Blake Abel"
---

Hello anybody viewing this! This post is meant to both show off this website and to help people who are looking to write posts format their own, so let's see how well I can pull this off.

Last practice we started by coming up with a few things for programming to do:

1. Make sure that the grabber code is switched over to a servo instead of a motor
2. Clean up some of the stuff I had lying around in code. *(sorry team)*
3. **PLAN AUTONOMOUS!**
    1. How we're going to move precise distances
        * IMU's Accelerometer
        * Encoders
    2. How we're turning precisely
        * IMU's Gyroscope
    3. How we're managing all four starting positions
        * Writing four
        * Writing one master one

Right now, we decided on the first options for all of those. Since it's *fairly* easy to write just one opmode if you have some experience (and if you have all the sweet stuff we put in [robotplus](https://github.com/MICDSRobotics/robotplus)), we decided to make four branches and each try to write an OpMode that will do everything for just one starting position. Once we test those, we'll regroup and write the others based on how everone's did. So far, mine's coming along nicely-- here's a preview:

{% highlight java %}
while (Math.abs(imuWrapper.getPosition().toUnit(DistanceUnit.INCH).y) < 4) {
    //Checks that blue jewel is closer towards the cryptoboxes (assuming color sensor is facing forward)
    if (colorSensorWrapper.getRGBValues()[2] > colorSensorWrapper.getRGBValues()[0]) {
        drivetrain.complexDrive(MecanumDrive.Direction.UP.angle(), 0.2, 0);
    } else {
        drivetrain.complexDrive(MecanumDrive.Direction.DOWN.angle(), 0.2, 0);
    }
}
{% endhighlight %}

I'm excited to see how the full thing turns out. Stay tuned for more in the future!
