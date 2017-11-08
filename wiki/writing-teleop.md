---
layout: default
title: Writing a Driver-Controlled OpMode
---

# {{ page.title }}

**_Important Documentation:_**

* [Everything](http://ftctechnh.github.io/ftc_app/doc/javadoc/index.html)
* [OpMode](http://ftctechnh.github.io/ftc_app/doc/javadoc/com/qualcomm/robotcore/eventloop/opmode/OpMode.html) - The basic flow of a driver-controlled OpMode

## Import Statements
There are a lot of things you'll probably want to import, but you'll almost always want some of these:

{% highlight java %}
    import com.qualcomm.robotcore.eventloop.opmode.Disabled;  
    import com.qualcomm.robotcore.eventloop.opmode.OpMode;
    import com.qualcomm.robotcore.eventloop.opmode.TeleOp;  
    import com.qualcomm.robotcore.hardware.DcMotor;
    import com.qualcomm.robotcore.hardware.CRServo;
    import com.qualcomm.robotcore.hardware.Servo;
    import com.qualcomm.robotcore.hardware.DcMotorSimple;
    import com.qualcomm.robotcore.hardware.HardwareMap;
    import com.qualcomm.robotcore.util.ElapsedTime;
{% endhighlight %}

Android Studio will also usually prompt you if something's not imported.

## Making the Class

For Teleop, you start with an [Annotation](https://docs.oracle.com/javase/tutorial/java/annotations/) to tell the app to show the OpMode on the DriverStation on the TeleOp side:

{% highlight java %}
@TeleOp(name="<What you want it to show as on the app>", group="<The group you want it in (useful for sorting and grouping)>")
{% endhighlight %}

You can add `@Disabled` if you want it to not show up in the app. Then you start it off with your actual class declaration:

{% highlight java %}
public class driveTest extends OpMode { ... }
{% endhighlight %}

You could also just copy the one of the template OpModes from the sample OpModes - I use BasicOpMode_Iterative.

## Declaring Hardware

To use motors, we need to somehow find a way to send voltages to the Core Power Distribution Module... luckily, FIRST's whole app comes with a lot of tools that we just imported to help us do that. All we need to do is declare the objects that we want to use; their objects do a lot of the work behind the scenes.

So to do that, we just declare an object like normal, of whatever we need. Their value should be set to null in the class, because it's better to get the hardware values after initializing so that you can change the configuration file on the fly. It'll look something like this:

{% highlight java %}
    private DcMotor leftMotor = null;
    private DcMotor rightMotor = null;

    private CRServo spinner = null;
    private Servo hitter = null;
{% endhighlight %}    

Now we can go into the initialization.

## Initialization

Initialization is the first time we're really going to have to think. Now is the time we're going to link those objects we just created to actual hardware connected to the robot (based on the configuration file). Remember what we put inside the strings, because that's the key to linking this to that configuration file, which we will create at the end.

### Assigning our objects to real objects
To do that, we'll set the objects equal to one of these statements: `hardwareMap.<object type>.get("<name in config>");`, which effectively just sets the object to the actual object in real life that we need it to be. All the motor values and positions and everything matches up with what's happening in real life, so the object in the code basically becomes the object in real life for all of our purposes. Now we can work with those values.

The whole statements should look something like this:

{% highlight java %}
    leftMotor = hardwareMap.dcMotor.get("left motor");
    spinner = hardwareMap.crservo.get("spinner");
{% endhighlight %}

### Preparing the motors and servos

Now that they're set up, it's a good idea to prepare them to actually be used. Technically this step isn't required, as there are many other ways to get the motors working properly, but it's a lot easier to reverse directions and set modes here to make the code easier to read and change.

#### Setting Directions

The first big thing is to set the motor directions. They can either be forward or reverse, and it's really difficult to know which one you should use before you test, so I recommend just picking one and seeing if it's correct when you test for the first time. Anyway, to set, you're going to use `setDirection(<direction>)`, a method in DcMotor. We don't really need to understand how this functions directly (although if you're really interested and have nothing better to do you could probably dig it up somehow), but we do need to understand how to use it.

To use it, you call it on a DcMotor object, with a direction in its parameter. The two directions we have are:

{% highlight java %}
    DcMotor.Direction.REVERSE
    DcMotor.Direction.FORWARD
{% endhighlight %}

two constants that correspond to forward and backward. After you're done, the whole statement should look something like this:

{% highlight java %}
    leftMotor.setDirection(DcMotor.Direction.REVERSE);
    spinner.setDirection(Servo.Direction.FORWARD);
{% endhighlight %}

#### Setting Modes

The other big thing is to set the all the motors' modes. There are a few big modes for us to keep in mind:

* `DcMotor.RunMode.RUN_WITHOUT_ENCODER` - This is probably the most common mode for using during TeleOp, which is simply running without any extra interference or data from encoders that may or may not be attached to the motors. Using `setPower(<power>)` directly translates to the voltage needed for the motors (0 is the min, 1 is the max; negative values move it in reverse)
* `DcMotor.RunMode.RUN_USING_ENCODER` - Probably not too common during TeleOp, but still definitely important to cover. The motors use encoder values and a set max speed to run at specific velocities. This allows for very precise movement, but because TeleOp is controlled by a driver, it's usually easier and more consistent to just ignore them since joysticks can get the same result with or without encoders. Requires an encoder.
* `DcMotor.RunMode.RUN_TO_POSITION` - Very niche uses in TeleOp. This forces the motors to rotate in whatever direction is necessary to get to a certain position. There's usually no need during TeleOp when you can just drive it to whatever position you need, but it's possible that this could be used for other motors separate from the drivetrain. Requires an encoder.
* `DcMotor.RunMode.STOP_AND_RESET_ENCODERS` - This mode isn't really intended for use with motors for extended periods of time. It's just used inside of TeleOp or Autonomous programs to reset the encoder values to 0 (doesn't move the wheels, just sets the current state as the new 0) and stops the motors. You have to set them to something else after if you want to use them. Requires an encoder.

And that's all of them. So, once you pick the one you think would be most suited for the program, we can actually set it. It's going to be very similar to setting the direction, just this time we're using `setMode(<mode>)`. The end result will be something like this:

{% highlight java %}
    leftMotor.setMode(DcMotor.RunMode.RUN_WITHOUT_ENCODER);
    rightMotor.setMode(DcMotor.RunMode.RUN_TO_POSITION);
{% endhighlight %}

You're all set now! Time to get into the fun stuff!

## The Loop

There are many different ways to make the heart of your driver-controlled OpMode, so I'm just going to give you the tools to do so. There are a ton of samples in FtcRobotController or our code from previous years too if you get stuck.

Put all your code inside `public void loop(){ ... }` for it to run continuously until the driver hits stop on the app.

### Gamepad

All of these can be accessed by calling them on gamepad1 or gamepad2, corresponding to whichever controllers did Start + A and Start + B on the app, respectively. Every time the loop runs, it will get the current state of the gamepad, so there's not need to update it like in RobotC or anything.

* `gamepad1.a` / `gamepad1.b` / `gamepad1.x` / `gamepad1.y` - All the buttons on the gamepad, represented as boolean variables. True means they're being pushed down, false means that they aren't.
* `gamepad1.dpad_down` / `gamepad1.dpad_up` / `gamepad1.dpad_left` / `gamepad1.dpad_right` - All the values for the directional pad (arrow keys) on the gamepad. Just like the buttons, they're boolean variables that are true when pushed down or false when not pushed down.
* `gamepad1.left_bumper` / `gamepad1.right_bumper` - The two bumpers of the controller, set up as boolean variables just like all the other buttons. These are really nice because it's easy to hit them while using the joysticks, but be prepared: every time you call them a bumper, even though that's what they are, half the team won't know what button that is. (It's in front of the trigger, c'mon!)
* `gamepad1.left_trigger` / `gamepad1.right_trigger` - The two triggers. These aren't buttons, so boolean variables don't work for these. Instead, they're set as floats that range from 0 (completely released) to 1 (held down). These are good for things that require a certain degree of variability to speed, but don't need to go backwards, like spinners or anything that extends out.
* `gamepad1.left_stick_x` / `gamepad1.left_stick_y` / `gamepad1.right_stick_x` / `gamepad1.right_stick_y` - The joysticks on the controllers. There are two separate variables for each stick, one for the x axis and one for the y axis. They both range from -1 to 1, representing all the way down and all the way up. These are almost always used for the drivetrain, for the great control they let us have over the robot and how intuitive it is to use.

There's also other buttons and values you can get from the gamepad, but they're rarely used, since we have two gamepads to work with. It's fun to make a single gamepad version that uses every button possible, but it's a lot easier to operate with two drivers, so I didn't include those other buttons. You can still read about them in the documentation provided by FTC at the top.

### Using the Motors

There are tons of methods to use, especially with encoders, but the one that is used by far the most is `setPower(<power>)`. It sets a motor's voltage to nothing (0), full (1), full reverse (-1), and every value in between. Used with the gamepad's joysticks, it's easy to translate fully up to full power forwards. Using this with if statements and buttons works particularly well too. It'll work something like this many times:

{% highlight java %}
    leftMotor.setPower(-gamepad1.left_stick_y);
    rightMotor.setPower(-gamepad1.right_stick_y);

    if(gamepad1.a){
        slider.setPower(1);
    } else if (gamepad1.b) {
        slider.setPower(-1);
    } else {
        slider.setPower(0);
    }
{% endhighlight %}

There's also `getPower()`, which returns whatever value the power is currently set to. Not super useful when trying to actually move the robot but incredibly nice when debugging and making sure everything works as intended with telemetry...

### Telemetry

This is the main way of printing things to the phone. Could be a string, could be a number, almost anything. To do this, we will use `telemetry.addData(<title>, <value>)`.

This takes in two strings as it's parameters: a title and a value. The title is any string you want, but usually it's good to just type one in yourself that has some relevance to what it represents. No one's stopping you from making it a meme though. With a title, it'll look something like this:

{% highlight java %}
    telemetry.addData("Left Motor", <value>);
{% endhighlight %}

The value can be almost anything as well, from Strings to floats. The ones that are used the most are values returned from `getPower()` and `getCurrentPosition()` (for encoders) or `getPosition()` (for servos), which help a lot for making sure that motors and encoders are working as intended. With the value inserted as well, the statement'll look something like this:

{% highlight java %}
    telemetry.addData("Left Motor", leftMotor.getPower());
{% endhighlight %}

All that's left is to call `telemtry.update();`. You only do this once inside the loop, otherwise the output will look super janky as it tries to update twice a frame. This just makes sure that all the values get refreshed and prints them to the screen. At the end of a few statements, your code will probably look something like this:

{% highlight java %}
    telemetry.addData("Left Motor", leftMotor.getPower());
    telemetry.addData("Right Motor", rightMotor.getPower());
    telemetry.addData("Spinner", spinner.getPosition());
    telemetry.update();
{% endhighlight %}

## Recap

Alright, now you know which files you need to import into many TeleOp programs and how to check to make sure you've got them all. You also know how to define objects on the robot and link them to their real-world counterparts to allow them to be used in the code. You know how to access values on the gamepad and how to use those to move the motors.

You know how to make a Driver-Controlled OpMode! Nice. :)

If you're still stuck or want to make sure you're heading in the right direction, you can check out a driver controlled opmode we're using in [our season's code](https://github.com/MICDSRobotics/RelicRecovery).
