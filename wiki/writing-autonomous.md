---
layout: default
title: Writing Autonomous
---

# {{ page.title }}

**_Important Documentation:_**

* [Everything](http://ftctechnh.github.io/ftc_app/doc/javadoc/index.html)
* [LinearOpMode](http://ftctechnh.github.io/ftc_app/doc/javadoc/com/qualcomm/robotcore/eventloop/opmode/LinearOpMode.html) - The basic flow of an Autonomous OpMode

(Usually, it's a good idea to start with creating TeleOp before Autonomous)

### Import Statements
There are a lot of things you'll probably want to import, but you'll almost always want some of these:

{% highlight java %}
import com.qualcomm.robotcore.eventloop.opmode.Disabled;
import com.qualcomm.robotcore.eventloop.opmode.Autonomous;
import com.qualcomm.robotcore.eventloop.opmode.TeleOp;
import com.qualcomm.robotcore.eventloop.opmode.LinearOpMode;
import com.qualcomm.robotcore.hardware.DcMotor;
import com.qualcomm.robotcore.hardware.HardwareMap;
import com.qualcomm.robotcore.util.ElapsedTime;
import com.qualcomm.robotcore.util.RobotLog;
{% endhighlight %}

Android Studio will also usually prompt you if something's not imported.

## Making the Class

For Autonomous, you start with an [Annotation](https://docs.oracle.com/javase/tutorial/java/annotations/) to tell the app to show the OpMode on the DriverStation on the Autonomous side:

{% highlight java %}
@Autonomous(name="<What you want it to show as on the app>", group="<The group you want it in (useful for sorting and grouping)>")
{% endhighlight %}

You can replace `@Autonomous` with `@Disabled` if you want it to not show up in the app.

Then you start it off with your actual class declaration:

{% highlight java %}
public class selfDriving extends LinearOpMode { ... }
{% endhighlight %}

LinearOpMode has a few methods that OpMode doesn't have, such as `sleep()`, which really help out when you can't rely on a driver to control the robot.

You could also just copy the one of the template OpModes from the sample OpModes - I use BasicOpmode_Linear.

## Declaring Hardware

Just like writing a Driver-Controlled OpMode, we have to declare our hardware. It's almost the exact same process here, although it's very important to make sure that your two OpModes line up. If they do, you can use the same configuration file without any issue, but if you don't there could be issues if you attempt to use the same configuration file. You could make two and remember to switch it when you want to run the other program, but it's a lot easier just to match the hardware in the code.

So, anyway, we just declare an object like normal, of whatever we need. Their value should be set to null for now, because it's better to set the hardware values after initializing so that you can change the configuration file on the fly from the phone. It'll look something like this:

{% highlight java %}
    private DcMotor leftMotor = null;
    private DcMotor rightMotor = null;
{% endhighlight %}

{% highlight java %}
    private CRServo spinner = null;
    private Servo hitter = null;
{% endhighlight %}

It's also a good idea to define any constants you want to use during autonomous here. Some good ideas we usually use are numbers for motor encoders, or specific times if you're not using any of them. Here's an example:

{% highlight java %}
    final int MOTOR_PULSE_PER_REVOLUTION = 7;
    final int MOTOR_GEAR_RATIO = 80;
    final int FULL_REVOLUTION = 1200;
    final int FLOOR_BLOCK = 2292;
{% endhighlight %}

Now on to initialization!

## Initialization

During initialization, we link those objects we just created to actual hardware connected to the robot (based on the configuration file). Make sure to remember what you put as parameters for `get()`, because that's the key to linking this to the configuration file.

This time though, there's no `init()` to use. Instead, we just put the code inside `runOpMode()`, which is called right when the OpMode is ran.

{% highlight java %}
    @Override
    public void runOpMode() throws InterruptedException {
{% endhighlight %}

The `InterruptedException` allows for methods like `sleep()` to run without an issue and without messing with other things like runtime.

### Assigning our objects to real objects

To link the hardware to the objects, we'll set the objects equal to one of these statements: `hardwareMap.<object type>.get("<name in config>");`, which effectively just sets the object to the actual object in real life that we need it to be. All the motor values and positions and everything matches up with what's happening in real life, so the object in the code basically becomes the object in real life for all of our purposes. Now we can work with those values.

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

* `DcMotor.RunMode.RUN_WITHOUT_ENCODER` - If you don't have encoders, this is the only mode you can really use. It just sets the power to whatever you pass it in the program directly, with no bells and whistles. This means that you're skipping out on some of the precision and data that encoders have to offer, but it means saving money if your team doesn't have any and less work for construction.  
* `DcMotor.RunMode.RUN_USING_ENCODER` - All the simplicity of not using encoders, with all of the potential of encoders. This allows for very precise movement, with a way to measure the distance traveled without other complicated sensors. Used with other sensors and gyroscopes, the robot's movement can become even more precise.
* `DcMotor.RunMode.RUN_TO_POSITION` - Not always the best for drivetrains in TeleOp, because it's more difficult to control direction. This will move the motor until it reaches a specific point on the encoders, moving either direction based on which would be more efficient. Still important to keep in mind though, and could come in handy for some routines.
* `DcMotor.RunMode.STOP_AND_RESET_ENCODERS` - Super useful for some Autonomous programs, as it resets the encoder values. It lets teams use the encoders in steps, without having to keep track of all the distance the robot has traveled so far. Also nice if you want to reuse some code, like hitting one beacon and then hitting the other; you can just reset and run the exact same stuff.

And that's all of them. So, once you pick the one you think would be most suited for the program, we can actually set it. It's going to be very similar to setting the direction, just this time we're using `setMode(<mode>)`. The end result will be something like this:

{% highlight java %}
    leftMotor.setMode(DcMotor.RunMode.RUN_TO_POSITION);
    rightMotor.setMode(DcMotor.RunMode.RUN_USING_ENCODER);
{% endhighlight %}

You're all set now! Time to get into the fun stuff!

## The Body

Now that we're ready to start, call `waitForStart();`, which will wait until the user hits play on the phone, and start coding the main body of your autonomous program.

After that, put all of your remaining code inside of a `while (opModeIsActive()) { ... }` loop, which will just make sure that your autonomous program will keep running until someone hits stop.

At the end of the loop, call `idle();`. It's not actually necessary to run, but it just makes the robot wait for a little bit as the hardware cycles again, which lessens the load on the phone's processor.

Your skeleton Autonomous should look something like this.

{% highlight java %}
    while (opModeIsActive()) {
        // All the code for your routines
        idle();
    }
{% endhighlight %}

### Telemetry

Telemetry, the main way of printing things to the phone, is extra important during Autonomous. With no driver to tell you that the motor is working but it's just not catching on the gear or that the servo isn't responding at all, it's super, super important to give yourself some hints as to what's happening to the robot.

To use telemetry, we will use `telemetry.addData(<title>, <value>)`, just like we did for TeleOp.

This takes in two strings as it's parameters: a title and a value. The title is any string you want, but usually it's good to just type one in yourself that has some relevance to what it represents. No one's stopping you from making it a meme though. With a title, it'll look something like this:

{% highlight java %}
    telemetry.addData("Left Motor", <value>);
{% endhighlight %}

The value can be almost anything as well, from Strings to floats. The ones that are used the most are values returned from `getPower()` and `getCurrentPosition()` (for encoders) or `getPosition()` (for servos), which help a lot for making sure that motors and encoders are working as intended. With the value inserted as well, the statement will look something like this:

{% highlight java %}
    telemetry.addData("Left Motor", leftMotor.getPower());
{% endhighlight %}

All that's left is to call `telemtry.update();`. During Autonomous, it will update the other statements as well when it's called, so you don't have to rewrite everything. The final code should look something like this, when you're done:

{% highlight java %}
    telemetry.addData("Left Motor", leftMotor.getPower());
    telemetry.addData("Right Motor", rightMotor.getPower());
    telemetry.addData("Spinner", spinner.getPosition());
    telemetry.update();
    . . . //Other parts of the autonomous
    telemetry.update();
{% endhighlight %}

### Moving the Robot

You move the robot the same way that you would in TeleOp, but this time you can't use the gamepad to cause things to happen. Instead, you will want to write your code based off of sensor data (including motor encoders), time, or perhaps another system entirely.

* Sensor Data: There is a huge amount of sensors to use for this, that I couldn't explain them all. Luckily, FTC provided all of the documentation for them in the links at the top of the guide. They also made sample OpModes using many of the sensors in `ftc_app-master\FtcRobotController\src\main\java\org\firstinspires\ftc\robotcontroller\external\samples` that can help demonstrate how to use them effectively.

* Encoders: Encoders are a good idea, especially paired with a gyroscope or other sensor to make sure turns are handled effectively. Depending on which mode you want it to run in (`RUN_USING_ENCODERS` or `RUN_TO_POSITION`), you will set it's target position and it's power. If you're using `RUN_TO_POSITION`, it will move until it gets to the specified target position, and then stop itself. If you're using `RUN_USING_ENCODERS`, You can set the target position then set the power like normal, but you will have to stop it once it reaches the target position.

* Time: Using time is pretty easy. You just set the motors to your desired power level, then call `sleep()` for however many seconds you want it to do that. The downside is that you have to time every distance you want it to go, which could change if the robot's hardware changes or if you bump into something while the autonomous program is running. Generally, this method isn't very accurate at all so it's not the best decision to _only_ use this.

There are many different ways to structure an autonomous opmode. It's best to just try a lot of things and use what works the best. Using as many sensors and data as possible is also a great idea to improve accuracy.

## Recap

You know some of the differences and similarities between TeleOp OpModes and Autonomous OpModes, and how to control a robot using predetermined steps. You also know how to communicate data about the robot in real-time to the phone to debug your programs and help figure out where issues may have arisen. You know all of the structuring for a basic linear OpMode, and can use it to create your own programs.

You know how to make an Autonomous OpMode! Way to go. :)

If you're still stuck or want to make sure you're heading in the right direction, you can check [our Basic Autonomous OpMode](https://github.com/blabel3/MicdsRobotics/blob/master/ftc_app-master/TeamCode/src/main/java/org/firstinspires/ftc/teamcode/RedTeamUnbeacon.java).
