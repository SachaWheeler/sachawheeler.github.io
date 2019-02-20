---
layout: project
title: Custom USB Controller
image: /images/missile/cover.jpg
summary: A Custom USB control panel for controlling audio recording software
---

I use computers a lot, so when I'm not using computers, I really don't want to
use computers.

I also play mandolin (and other stringed instruments) with a few friends once a week,
and we want to be able to start (and stop) recording without having to use a computer
keyboard or mouse. It's also about 5 feet away from where we play, and I'm pretty lazy,
so a custom USB controller seemed to be a solution.

The [Sparkfun Pro Micro](https://www.sparkfun.com/products/12640) comes
equipped with a full-speed USB transceiver, which I don't think Arduino does (yet).

![](/images/missile/IMG_7708.JPG)

A great little board, but it is tiny - soldering header pins made me feel old.

![](/images/missile/IMG_7709.JPG)

I had a couple of *fun* switches which I'd wanted to use for something silly.

![](/images/missile/IMG_7678.JPG)

And bought a [small enclosure](https://www.ebay.co.uk/itm/Retex-33020101-Abox-1-Grey-195-x-62-x-120mm/132106419492?ssPageName=STRK%3AMEBIDX%3AIT&_trksid=p2057872.m2749.l2649) from ebay

![](/images/missile/enclosure.png)

And a [USB-b to micro panel mount cable](https://www.amazon.co.uk/gp/product/B01BMRTURS/)

![](/images/missile/cable.jpg)

To mount inside

The code is very simple. The *missile* trigger switch sends a custom sequence
which activates an AppleScript on my server which opens Audacity

```cpp
  Keyboard.press(KEY_LEFT_GUI);
  Keyboard.press(KEY_LEFT_SHIFT);
  Keyboard.press('1');  // send a command to the computer via Keyboard HID
  Keyboard.releaseAll();
```

The *fire* button sends:

```cpp
  Keyboard.write('r');
```

to record, and:
```cpp
Keyboard.write('p');
```

to pause.

## Leds

I added LEDs. One to tell me when the controller is plugged in

![](/images/missile/IMG_7756.JPG)

One for when it's activated

![](/images/missile/IMG_7757.JPG)

And a blinking LED when we're recording

![](/images/missile/IMG_7759.JPG)

## The finished product

![](/images/missile/ezgif-4-5b23530c7df3.gif)

The [code is here](https://github.com/SachaWheeler/USB-controlller).

## Thoughts

This was a fun project. Computer hardware interfaces are very generic
(compared, say with engineering hardware, which is intricately specific),
but the tasks we need them for often are not.

It turns out, making your own interfaces is fun, easy and cheap, and makes the
creative process more productive, more informative and more personal.