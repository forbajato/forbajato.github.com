---
layout: post
title: "Raspberry Pi GPIO "Hello World"
description: "The 'Hello World' of the physical computing realm is blinking an LED.  Here is how to do it with an RPi running RPi.GPIO on Raspbian Wheezy."
summary: "The first step in the world of physical computing is generally to wire up an LED and make it blink.  The RPi's GPIO pins make this quite simple, even with older models.  Here is how to do it with an Raspberry Pi B+."
category: "Tutorials" 
tags: [blog, physical computing, python, raspberrypi]
---
{% include JB/setup %}

# Blinking, blinking, everywhere blinking lights!

LEDs are ubiquitous.  Everywhere you turn you see some LED sign or other blinking encouragement to buy something, eat somewhere, run because the Cylons are attacking ... lots of things!  How do all those things work?  LEDs are quite simple but how do you control them with a Raspberry Pi?  That is what I set out to figure out.

# Introduction to LEDs and resistors

LED stands for "light emitting diaode."  For that to make total sense you need to know what a "diode" is.  A diode is an electronic device that allows electricity to pass one direction through it but not the other.  Like driving in New Orleans - it is a one way street.  This is important if you do your project and find the LED doesn't light up - check to be sure the juice is flowing the proper direction, if won't light if you are trying to go the wrong way on a one way street.

LEDs come in all sorts of colors.  Red is the most common but you don't need to be limited to that. It is important to keep in mind that different LEDs have different tolerances which result in different sized resistors to protect them.  You can check out a great article on choosing resistors for LEDs [here](http://www.evilmadscientist.com/2012/resistors-for-leds/).  As you can see from the chart at the bottom of that page, most LEDs can be protected with 39 - 150 Ohm resistors.  It turns out a slightly larger resistor is no problem.  I had a boat load of 220 Ohm resistors laying around and that worked fine. If you put too much juice through your LED you will get to see some very interesting things, and perhaps release the magic blue smoke.  It is a truth in physical computing that if the smoke escapes it is time to start over (and maybe flip a breaker).

How do you know how much resistance your resistor gives?  Check the color bands.  There are a number of easy to use resistor "calculators" out there, [this](http://www.digikey.com/en/resources/conversion-calculators/conversion-calculator-resistor-color-code-4-band) one is quite simple to use.  Again, if you are using a multimeter you can also measure resistance directly on the resistor you have.

Since LEDs only conduct electricity in one direction it is good to figure out what direction that is.  If your LED is new then you want the longer wire coming out toward the juice, the shorter wire toward the ground.  If (like me) you have a box of salvage components that you are using, you can't count on that long wire still being the original long one.  To verify you can use a multimeter on its conductance setting.  This allows you to put the probes of the multimeter to test whether a circuit is conducting electricity.  Put the probes on one way and then the other, which ever direction conducts the juice is the direction to plug in the LED.

TODO: Put in photos of LED and resistor

# The circuit

Now to wire up the circuit.  If you want to follow along closely (i.e. you are not familiar with the GPIO pin out of a RPi and want to be sure the code here works) then make sure you plug everything in just the way it is in the photo below.  I am using an old RPi B+, the pin out can be found [here](https://www.raspberrypi.org/documentation/usage/gpio/).  I have the third pin from the top (if holding the RPi with the ethernet and USB ports down) on the outside row of pins plugged in as ground.  Pin 17 is being used as the control pin.  Keep in mind that there are two ways to number the pins, I am referring to pin 17 because that is the Broadcom (BCM) way of referrign to the pins.  This will matter when we get to coding with RPi.GPIO.  It also happens to be the way the pins are identified in the picture at the top of the linked page on pinouts.

So when electricity flows it goes from live pin -> through the circuit -> to the ground pin. I like to think of it in that direction because that is the way the electrons are flowing.  In our circuit they will flow out of pin 17 through the resistor, then the LED, then back to the ground pin on the RPi.  It turns out that resistance is a function of the whole circuit so you can put the resistor on either side of the LED that is convenient.

TODO: Put in photos of the circuit

## An aside about breadboards

Since I haven't talked about breadboards before I will offer a quick intro. Breadboards have holes in rows and columns.  Usually there are two columns along the lateral edge of the board and then lots of rows (4 holes long usually) alone the face of the breadboard, then a gutter then another set of 4 hole rows, then another set of 2 columns.  Keep in mind the columns are connected to each other as are the rows.  So if you put juice into a column you can tap into that column anywhere along the breadboard to get the juice out.  Similarly if you wire one column to ground.  In fact, wiring one column hot and the other as ground is quite standard.  Our circuit is so simple I didn't do that but it is a useful practice.

The rows on the breadboard connect with each other as well.  If you plug a component into one row you can connect it to another component by pluging into another hole in the row.  This is only true for rows on a side, the gutter in the middle interrupts those row connections.

If this seems confusing and your breadboard is cheap you can probably peal off the back of the breadboard and see how things are connected under there. I know when I destroyed a breadboard this way (OK, it still works, sometimes) it was very enlighting.

# Preparing the RPi

We are going to use RPi.GPIO for this project.  I started out with the plan to use [GPIOZero](http://gpiozero.readthedocs.org/en/v1.1.0/index.html) but ran into trouble getting it installed.  GPIOZero looks like a great package and I hope to check it out someday but I ended up falling back to the older [RPi.GPIO](https://sourceforge.net/p/raspberry-gpio-python/wiki/Home/). RPi.GPIO is a bit more complicated but still not too difficult (beats using command line dumps to the GPIO pins to turn the LED on and off!).

Before you install RPi.GPIO make sure you are working on an updated system. I am using a Raspbian Wheezy install so all commands will be with apt-get.  If you are using a different OS to drive your RPi adjust accordingly.

    sudo apt-get update
	 sudo apt-get upgrade

Then you need to install the RPi.GPIO (maybe).

    sudo apt-get install python-rpi.gpio python3-rpi.gpio


