---
layout: project
title: Spectrum Analyser
image: /images/spectrum_analyser/cover.png
categories: [electronics]
summary: An Arduino powered spectrum analyser (graphic equalizer)
---

I found an old, unused [16x32 RGB led matrix](https://www.adafruit.com/product/420){:target="_blank"} which I bought on a whim years ago and
had completely forgotten about.

![](/images/spectrum_analyser/led_matrix_rgbmatrix.jpg)

After a little googling I found the
[Sparkfun Spectrum Shield](https://www.amazon.com/SparkFun-Sparkfun-Spectrum-Shield/dp/B00X0K30I6){:target="_blank"}
which seemed to do most of the heavy lifting of a 7-band audio spectrum analyser.

![](/images/spectrum_analyser/shield.jpg)

## assembling the shield
I soldered headers onto the shield before reading the spec sheet and later realised that in order
to get the comparatively low-powered arduino to handle the refresh rate on the matrix, certain *liberties*
were taken in the Matrix library; the matrix is *required* to use specific ports for it to work.

The Spectrum Shield *also* uses specific ports for its functionality, and these ports conflict - I would be unable
simply to plug the shield into the arduino, as I'd soldered it.

So I decided to use coloured ribbon cables so I could redirect the 4 pins (2 analogue, 2 digital) that the Shield required
to ports that were avalaible (essentially moving the Shield digital ports 4 & 5 to 12 & 13, and Analogue ports A0 & A1 to A4 & A5).

![](/images/spectrum_analyser/jumper_cables.jpg)

## Power
The Matrix wants 5v but needs up to 4 Amps, averaging about 2A of current, so I split the output of a 5v 4A supply between the
Matrix and the current-limiting Arduino barrel plug.

![](/images/spectrum_analyser/power.jpg)

And went the easy route of simply super-glueing non-solder wires into the ribbon cable of the Matrix

![](/images/spectrum_analyser/ribbon.jpg)

## Sadly, that was most of it

At this point it was all about software. Creating a basic visualiser was quite simple.

![](/images/spectrum_analyser/video.gif)

## The finished product

It's not finished yet, but I like it. I'm adding visualisations and some controls.
At some point there might need to be a containing structure. I'm leaving that decision for later.

## The code

is [here](https://github.com/SachaWheeler/spectrum-analyser/blob/master/spectrum_analyser.ino){:target="_blank"}
