---
layout: default
title: Robot Overview
---

# Robot Status: Cool Mom (v2)

![Robot](https://github.com/MICDSRobotics/MICDSRobotics.github.io/blob/master/assets/img/SRPre-Match.jpg?raw=true)

## Hardware

### Drivetrain

We watched a number of teams at worlds dominate with mecanum wheels: the ability to move in any direction is so powerful, especially in autonomous. So this year, we decided to try mecanum wheels on our robot, and have been very pleased with the results. Our robot can now move precisely to the side without sacrificing time and accuracy turning in autonomous, and it's super easy to line up for scoring glyphs in teleop. The chain system we use on it optimizes a mixture of speed and torque that helps the robot move at a competent speed while not sacrificing power due to weight. Getting up on the balancing stone was difficult, but we added custom 3D printed skid plates to allow our robot to mount the balancing stones without redoing our already built drivetrain

### Lifting System

Our goal was to make a vertical lift that was the strongest, most space efficient, and fastest that it could possibly be. We decided early on that we needed a rack and pinion to be a good balance between strength, speed, and precision. We used to use a complixated group of chains to move both raisers together, but have since switched it out for a much simpler (and consistent) mechanism that just has the motor set up with chains and a sprocket to rotate the pinion gear, and a large axle connecting to the pinion gear across from it, raising them both together.

### Intake

Our old intake system was a gripper that would extend out and grab a glyph. Since then, we've switched to an intake with wheels that can take in glyphs from many more angles, which helps for speed and consistency. We 3D printed a flexible gear like wheel using TPU Flexible Filament that helps the glyphs to enter the intake from any angle, and has secure grip on the glyph.

### Outtake

We now use a sheet of lexan fixed to a high-powered servo under it to flip our glyphs from the center of the robot out the back of our robot in a column. It's been very consistent, and the servo has been very reliable at making the glyphs leave the robot straight and vertical.

## Software

### Autonomous

We hit the jewel using our two-servo jewel arm with a color sensor fixed to it. If it doesn't detect a large enough difference in color, the arm will retract, to help avoid basically throwing the match by guessing at a jewel.

We also scan the cryptobox key using Vuforia while still on the balancing stone, which is quick and hasn't failed us.

Then we move using our mecanum wheels and precisely timed movements -- aided by our function that adjusts for the battery voltage. Once we're in front of the cryptobox, we move to the correct column we scanned earlier and score our glyph.

If we started on a side closer to the relic scoring pads, the robot will head back towards the center glyph pile, attempt to intake glyphs, and attempt to score them. It's not as consistent, but we don't lose any points for going for it either.

### Teleop

All the controls are accessible by both drivers in case of controller failure. Control of the drivetrain is swappable between drivers with the start button, to help for some drivers that are good at picking up glyphs but bad at scoring them, while also preventing any accidental movements. Most of the intake, jewel arm, and outtake is accessible through buttons on the controller, and the rest is accessibly by holding Y, our "Shift" key, that changes the functionality of the other buttons. This is made possible by our controller wrapper that we developed for that purpose.

## Results

* Consistent 85 point autonomous from anywhere, shaky 100 point autonomous from the sides closer to the relic scoring.
* Average about 8 glyphs at the moment, for a total of 48 points
* Park on the balancing stone for 20 points.
